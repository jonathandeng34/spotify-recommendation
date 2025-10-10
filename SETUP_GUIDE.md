# Spotify Recommendation Analysis - Setup Guide

## Project Overview
This repository contains a Jupyter notebook for analyzing gym membership churn patterns using Python data science libraries.

## Prerequisites
Before setting up this project, ensure you have the following installed:
- **Python 3.8+** - Download from [python.org](https://python.org)
- **Git** - Download from [git-scm.com](https://git-scm.com)
- **VS Code** - Download from [code.visualstudio.com](https://code.visualstudio.com)
- **VS Code Extensions**:
  - Python (by Microsoft)
  - Jupyter (by Microsoft)

## Step-by-Step Setup Instructions

### 1. Clone the Repository
Open your terminal/command prompt and run:
```bash
git clone https://github.com/jonathandeng34/gym-churn.git
cd gym-churn
```

### 2. Create Virtual Environment
Create an isolated Python environment for this project:

**On macOS/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

**On Windows:**
```cmd
python -m venv venv
venv\Scripts\activate
```

### 3. Install Dependencies
With your virtual environment activated, install all required packages:
```bash
# Upgrade pip to latest version
pip install --upgrade pip

# Install all project dependencies
pip install -r requirements.txt
```

This will install the following packages:
- jupyter, notebook, jupyterlab, ipykernel
- pandas, numpy
- matplotlib, seaborn, plotly
- scikit-learn
- ipywidgets

### 4. Configure Jupyter Kernel
Make your virtual environment available to Jupyter:
```bash
python -m ipykernel install --user --name=gym-churn --display-name="Python (gym-churn)"
```

### 5. VS Code Configuration
1. **Open the project in VS Code:**
   ```bash
   code .
   ```

2. **Select Python Interpreter:**
   - Press `Cmd+Shift+P` (Mac) or `Ctrl+Shift+P` (Windows/Linux)
   - Type "Python: Select Interpreter"
   - Choose `./venv/bin/python` (Mac/Linux) or `.\venv\Scripts\python.exe` (Windows)

3. **Configure Notebook Kernel:**
   - Open `cleaning_and_eda.ipynb`
   - Click the kernel selector in the top-right corner of the notebook
   - Select "Select Another Kernel"
   - Choose "Jupyter Kernel"
   - Select "Python (gym-churn)"

### 6. Verify Your Setup
Test that everything is working correctly:

1. **Run the first cell** (imports):
   - Should complete without errors
   - May show "Matplotlib is building the font cache" message (normal)

2. **Run the second cell** (load dataset):
   - Should display "Dataset Shape: (114000, 21)"
   - Should show the first 5 rows of data

## Project Structure
After setup, your project directory should look like this:
```
gym-churn/
├── .git/                 # Git version control
├── .vscode/
│   └── settings.json     # VS Code configuration
├── venv/                 # Virtual environment (don't commit!)
├── dataset.csv           # The gym membership data
├── cleaning_and_eda.ipynb  # Main analysis notebook
├── requirements.txt      # Python dependencies
├── README.md            # Project documentation
└── .gitignore           # Files to ignore in Git
```

## Daily Workflow

### Starting Work
1. **Navigate to project directory:**
   ```bash
   cd gym-churn
   ```

2. **Activate virtual environment:**
   ```bash
   # Mac/Linux
   source venv/bin/activate
   
   # Windows
   venv\Scripts\activate
   ```

3. **Open VS Code:**
   ```bash
   code .
   ```

### Ending Work
1. **Deactivate virtual environment:**
   ```bash
   deactivate
   ```

## Troubleshooting

### Kernel Issues
**Problem:** "The kernel failed to start" or "Python Environment 'Python' is no longer available"

**Solutions:**
1. **Refresh Kernels:**
   - `Cmd+Shift+P` → "Jupyter: Refresh Kernels"
   - Select kernel again

2. **Reinstall Kernel:**
   ```bash
   python -m ipykernel install --user --name=gym-churn --display-name="Python (gym-churn)"
   ```

3. **Restart VS Code** and reselect interpreter

### Missing Packages
**Problem:** ImportError or ModuleNotFoundError

**Solution:**
```bash
# Ensure virtual environment is activated
source venv/bin/activate  # or venv\Scripts\activate on Windows

# Reinstall requirements
pip install -r requirements.txt
```

### Virtual Environment Issues
**Problem:** Commands not found or wrong Python version

**Solution:**
1. **Verify activation:**
   - Terminal prompt should show `(venv)` prefix
   - Check Python path: `which python` (should point to venv)

2. **Recreate if necessary:**
   ```bash
   deactivate
   rm -rf venv  # rmdir /s venv on Windows
   python3 -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```

## Adding New Packages
If you need to install additional packages:

1. **Install the package:**
   ```bash
   pip install package-name
   ```

2. **Update requirements.txt:**
   ```bash
   pip freeze > requirements.txt
   ```

3. **Commit the updated requirements.txt** to share with team

## Best Practices

### Git Workflow
- **Never commit the `venv/` directory** (already in .gitignore)
- **Always update requirements.txt** when adding new packages
- **Commit notebook files with cleared outputs** for cleaner diffs

### Environment Management
- **Always activate the virtual environment** before working
- **Keep requirements.txt updated** for team synchronization
- **Use the same Python version** across team members when possible

## Contact & Support
For issues specific to this project setup:
1. Check this troubleshooting guide first
2. Ensure all prerequisites are properly installed
3. Verify virtual environment is activated
4. Contact the project maintainer if issues persist

---

**Repository:** https://github.com/jonathandeng34/gym-churn
**Last Updated:** October 10, 2025