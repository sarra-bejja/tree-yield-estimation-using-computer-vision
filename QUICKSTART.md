# Quick Start Guide

## 🚀 Running This Project

### Option 1: Google Colab (Recommended)

This project is designed to run in **Google Colab Pro** for easy access to GPUs and cloud storage.

1. **Open any notebook in Colab**:
   - Go to [colab.research.google.com](https://colab.research.google.com)
   - Click "Upload" and select any `.ipynb` file from this repository
   - Or navigate to GitHub and open directly: `colab.research.google.com/github/YOUR-USERNAME/PFE-ModelsCode-SarraBejja`

2. **Mount Google Drive**:
   ```python
   from google.colab import drive
   drive.mount('/content/drive')
   ```

3. **Install dependencies** (first cell usually handles this):
   ```bash
   !pip install torch torchvision
   !pip install transformers scikit-learn ultralytics roboflow pycocotools
   ```

4. **Follow the notebook cells in order**

### Option 2: Local Development

#### Prerequisites
- Python 3.8+
- GPU recommended (NVIDIA with CUDA support)
- 8GB+ RAM

#### Setup

```bash
# Clone the repository
git clone https://github.com/YOUR-USERNAME/PFE-ModelsCode-SarraBejja.git
cd PFE-ModelsCode-SarraBejja

# Create virtual environment
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

#### Running Notebooks Locally
```bash
jupyter notebook
# Then open any .ipynb file
```

---

## 📊 Model Selection Guide

**Which model should I use?**

| Goal | Model | File |
|------|-------|------|
| **Fast baseline** | Random Forest | `RandomForestModel.ipynb` |
| **Best accuracy** | Vision Transformer | `VITsegmentation.ipynb` |
| **Good balance** | ResNet18 CNN | `CNNmodel.ipynb` |
| **Object counting** | YOLOv11 | `YoloModel.ipynb` |
| **Advanced (segmented)** | CNN Segmentation | `CnnSegmentation.ipynb` |

---

## 🔑 API Keys Required

### Roboflow (for YOLOv11 dataset)
1. Go to [Roboflow](https://roboflow.com)
2. Create account and project
3. Copy your API key
4. Replace in `YoloModel.ipynb`:
   ```python
   rf = Roboflow(api_key="YOUR_API_KEY_HERE")
   ```

---

## 📁 Data Organization

Before running, organize data as:
```
Google Drive/
└── MyDrive/
    ├── oliveTreeDataset/
    │   ├── train/
    │   ├── valid/
    │   └── test/
    └── YOLO_OrangeTreeFinal/
        └── Dataset/
```

Image filenames must contain yield values:
- ✅ Good: `tree_300kg.jpg`, `fruit_150kg.jpg`
- ❌ Bad: `image1.jpg`, `photo.png`

---

## ⚡ Quick Training Example (CNN)

```python
import torch
from torch.utils.data import DataLoader
import torch.optim as optim

# Load model (see CNNmodel.ipynb)
model = create_model()  # Your model creation function
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
model = model.to(device)

# Training loop
optimizer = optim.Adam(model.parameters(), lr=1e-4)
criterion = torch.nn.MSELoss()

for epoch in range(num_epochs):
    for images, yields in train_loader:
        images, yields = images.to(device), yields.to(device)
        
        # Forward pass
        predictions = model(images)
        loss = criterion(predictions, yields)
        
        # Backward pass
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
```

---

## 🎯 Expected Performance Metrics

These are approximate baselines (actual results may vary):

| Model | MAE (kg) | R² Score | Training Time |
|-------|----------|----------|---------------|
| Random Forest | 15-20 | 0.75-0.85 | 5-10 min |
| ResNet18 CNN | 10-15 | 0.80-0.90 | 30-60 min |
| Vision Transformer | 8-12 | 0.85-0.95 | 60-120 min |
| YOLOv11 + Formula | 12-18 | 0.70-0.80 | 90-180 min |

*Times based on ~1000 images with Colab GPU*

---

## 🐛 Troubleshooting

### "ImportError: No module named torch"
```bash
pip install torch torchvision torchaudio
```

### "CUDA out of memory"
- Reduce batch size: `batch_size = 8`
- Use smaller model: `YOLOv11n` instead of `YOLOv11m`

### "Image paths don't match"
- Verify CSV has correct `image_path` column
- Check paths are absolute (e.g., `/content/drive/MyDrive/...`)

### "Roboflow download fails"
- Verify API key is correct
- Check internet connection
- Ensure project exists and is public/accessible

### "COCO masks not decoded properly"
- Verify JSON format: `{"images": [...], "annotations": [...]}`
- Check RLE encoding: `"segmentation": {"size": [h, w], "counts": "..."}`

---

## 📚 Useful Resources

- [PyTorch Documentation](https://pytorch.org/docs/)
- [Hugging Face Transformers](https://huggingface.co/docs/transformers/)
- [YOLOv11 Docs](https://docs.ultralytics.com/)
- [scikit-learn](https://scikit-learn.org/)
- [Google Colab Tips](https://colab.research.google.com/notebooks/snippets/importing_libraries.ipynb)

---

## 💾 Saving & Loading Models

### PyTorch Models
```python
# Save
torch.save(model.state_dict(), '/path/to/model.pth')

# Load
model.load_state_dict(torch.load('/path/to/model.pth'))
```

### Random Forest
```python
import joblib

# Save
joblib.dump(rf_model, '/path/to/model.pkl')

# Load
rf_model = joblib.load('/path/to/model.pkl')
```

### YOLOv11
```python
# Save (automatic)
results = model.train(...)  # Saves to runs/detect/trainX/weights/best.pt

# Load
model = YOLO('runs/detect/train/weights/best.pt')
```

---

## 🚀 Next Steps

1. **Try the CNN model first** - it's well-documented and beginner-friendly
2. **Compare different architectures** - see which works best for your data
3. **Optimize hyperparameters** - tune learning rate, batch size, etc.
4. **Ensemble methods** - combine predictions from multiple models
5. **Deploy** - save best model and create inference pipeline

---

## 📞 Getting Help

- Check inline comments in notebooks
- Review README.md for architecture details
- Open an issue on GitHub with:
  - Error message (full traceback)
  - Steps to reproduce
  - Your environment (GPU, Colab/Local, Python version)

---

Happy training! 🌱🍊
