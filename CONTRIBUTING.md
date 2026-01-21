# Contributing to Preharvest Tree Yield Estimation

We welcome contributions to improve this project! Here are the guidelines:

## Getting Started

1. Fork the repository on GitHub
2. Clone your fork locally:
   ```bash
   git clone https://github.com/YOUR-USERNAME/PFE-ModelsCode-SarraBejja.git
   cd PFE-ModelsCode-SarraBejja
   ```

3. Create a new branch for your feature:
   ```bash
   git checkout -b feature/your-feature-name
   ```

## Development Setup

1. Set up Python environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

2. Install dependencies:
   ```bash
   pip install torch torchvision
   pip install transformers scikit-learn ultralytics roboflow pycocotools
   pip install pandas matplotlib tqdm jupyter
   ```

3. For Google Colab execution, ensure you have Colab Pro access and the necessary API keys

## Code Style

- Follow PEP 8 guidelines
- Use meaningful variable names
- Add comments for complex operations
- Document new functions with docstrings

## Making Changes

1. **For Jupyter Notebooks**:
   - Keep cells focused on single tasks
   - Add markdown cells explaining each section
   - Include inline comments for complex logic
   - Ensure all outputs are cleared before committing

2. **For New Models**:
   - Follow the existing project structure
   - Create a new notebook with descriptive name
   - Include data loading, preprocessing, model definition, training, and evaluation
   - Add evaluation metrics and visualizations

## Testing

Before submitting:
1. Run notebooks end-to-end in a fresh Colab session
2. Verify all outputs are reproducible
3. Check file paths are relative to Google Drive structure
4. Validate model weights save correctly

## Submitting Changes

1. Commit your changes with clear messages:
   ```bash
   git commit -m "Add: Feature description"
   git commit -m "Fix: Bug description"
   git commit -m "Improve: Performance enhancement"
   ```

2. Push to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```

3. Create a Pull Request with:
   - Clear description of changes
   - Link to related issues
   - Screenshot of results if applicable
   - Explanation of approach

## Documentation

When adding new features, update:
- This README.md with new model descriptions
- Notebook markdown cells with explanations
- Any relevant API documentation

## Issues & Discussions

- Report bugs with reproduction steps
- Suggest improvements with use cases
- Share results and benchmarks
- Discuss architectural decisions

## Code of Conduct

- Be respectful and inclusive
- Provide constructive feedback
- Help others learn and improve
- Credit and acknowledge contributions

---

Thank you for contributing to this research project!
