# AI Coding Instructions for PFE ML Models

This is a **Google Colab-based machine learning project** for olive/orange tree yield prediction and segmentation using multiple deep learning architectures.

## Project Architecture

### Data Flow & Models

The project implements **4 parallel modeling approaches** for yield prediction, each in separate Jupyter notebooks:

1. **CNNmodel.ipynb** - CNN-based regression (ResNet18 pretrained)
   - Extracts yield values from image filenames using regex pattern matching (e.g., "300kg")
   - Custom `OliveYieldDataset` class loads images and labels
   - Output: Single regression value (kg yield) per image

2. **RandomForestModel.ipynb** - Traditional ML with GLCM features
   - Extracts hand-crafted GLCM (Gray Level Co-occurrence Matrix) features: contrast, dissimilarity, homogeneity, energy, correlation
   - Uses sklearn RandomForest for regression
   - Simpler baseline compared to deep learning models

3. **VITsegmentation.ipynb** - Vision Transformer-based regression
   - Uses `google/vit-base-patch16-224-in21k` from Hugging Face Transformers
   - Adds custom regression head (Linear → ReLU → Dropout → Linear)
   - Identical data pipeline as CNN model

4. **YoloModel.ipynb** - Object detection for tree/fruit detection
   - Uses YOLOv11 via ultralytics library
   - Dataset downloaded from Roboflow annotation platform
   - Expects YOLO format: images + corresponding .txt label files

### Segmentation Models (CNN & ViT variants)

- **CnnSegmentation.ipynb** - Semantic segmentation with COCO dataset format
  - Loads COCO JSON annotations with RLE-encoded masks
  - Applies masks to original images using pycocotools
  - Outputs masked images for training

- **RandomForestModelSegmentation.ipynb** - Feature extraction from masked images
  - Processes segmented olive tree images
  - Extracts GLCM features from masked regions only

## Critical Conventions

### Data Organization Pattern

All notebooks follow this Google Drive folder structure:
```
MyDrive/
├── oliveTreeDataset/{train,valid,test}/           # Raw images
├── oliveTreeRealdata/                             # CSVs with image_path + yield_kg
├── YOLO_OrangeTreeFinal/                          # YOLO datasets
│   ├── Dataset/{train,valid,test}/{images,labels}/
│   ├── Weights/                                   # Model checkpoints
│   └── Results/                                   # Inference outputs
├── OliveTreeFinalCNNsegmentation/
│   ├── DatasetOlive/_annotations.coco.json        # COCO format annotations
│   ├── MaskedImages/                              # Processed segmentation masks
│   └── ModelWeights/
```

### Key Implementation Patterns

1. **CSV-based Dataset Management**
   - Extract yields from filenames → save to CSV with `image_path` and `yield_kg` columns
   - Use pandas DataFrame as source of truth for all train/valid/test splits
   - See: CNNmodel.ipynb cells 3-4 for extraction pattern

2. **Standard Image Preprocessing Pipeline**
   - Resize to 224×224 (standard for ImageNet-pretrained models)
   - Convert to tensor and normalize using ImageNet stats: `[0.485, 0.456, 0.406]`, `[0.229, 0.224, 0.225]`
   - Applied via `torchvision.transforms.Compose()`

3. **Google Colab-Specific Code**
   - Always mount drive early: `from google.colab import drive; drive.mount('/content/drive')`
   - Use `!pip install` for dependencies (cleanvision, ultralytics, roboflow, pycocotools)
   - Check GPU with `!nvidia-smi` before training
   - All data stored in `/content/drive/MyDrive/` paths

4. **Regression Output Format**
   - All models output single continuous values (yield in kg)
   - Use PyTorch float tensors: `torch.tensor(value, dtype=torch.float32)`
   - Evaluation metrics: MSE, MAE, R² score from sklearn

### Common Debugging Points

- **Image Loading Issues**: Always check image path strings extracted from CSV; verify images exist at those paths before training
- **GLCM Feature Extraction**: Resizes images to 224×224 before feature computation; returns 5 GLCM features
- **COCO Mask Processing**: RLE masks must be decoded, then accumulated with `np.maximum()` for multi-instance objects
- **Roboflow Dataset Downloads**: Requires valid API key in notebook; verifies label/image file counts match after download

## Development Workflow

All notebooks are designed for iterative development in Google Colab:
1. Mount drive and create output folders
2. Load/prepare data (CSV creation or COCO parsing)
3. Define custom Dataset class
4. Build model architecture
5. Training loop with validation
6. Save weights and results to `/content/drive/`

No build system or testing framework used; each notebook is standalone executable in Colab environment.

## Conventions to Preserve

- **Yield extraction regex**: `r'(\d+)\s*kg'` (case-insensitive, tolerates whitespace)
- **Batch size**: Typically 16 for regression tasks
- **Train/valid/test split**: Always use three separate CSVs, never split dynamically
- **Output directories**: Create mirrored folder structure for weights, results, logs
