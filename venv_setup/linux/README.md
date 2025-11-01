# Linux Virtual Environment Setup - Step-by-Step Guide

This guide provides detailed, step-by-step instructions for setting up Python virtual environments on Linux. Each method is tested and documented for classroom demonstration.

> **📌 Advanced Users**: Need to store cache in project directory instead of home? See **[ADVANCED_CACHE_SETUP.md](./ADVANCED_CACHE_SETUP.md)** for complete cache configuration guide.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Demonstration Files](#demonstration-files)
3. [Method 1: Python venv (Built-in)](#method-1-python-venv-built-in)
4. [Method 2: virtualenv](#method-2-virtualenv)
5. [Method 3: Conda](#method-3-conda)
6. [Method 4: Miniconda](#method-4-miniconda)
7. [Method 5: Mamba](#method-5-mamba)
8. [Method 6: pyenv](#method-6-pyenv)
9. [Method 7: uv (Modern & Fast)](#method-7-uv-modern--fast)
10. [Comparison of Results](#comparison-of-results)
11. [Troubleshooting](#troubleshooting)
12. [Advanced: Custom Cache Directory](./ADVANCED_CACHE_SETUP.md)

---

## Prerequisites

Before starting, ensure you have the following tools and check your system:

### System Checks

```bash
# Check your current directory
pwd
# Should be: /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux
# This is my directory, for you it will be where you start your project

# Check Linux distribution
cat /etc/os-release

# Check available disk space
df -h /NFSDISK2/pyare/TAship/DSP315/

# Update package lists (recommended before installing anything)
sudo apt update  # For Ubuntu/Debian
# For other distributions, use: yum update, dnf update, pacman -Syu, etc.
```

### Required Tools Check

```bash
# Check if Python3 is installed
python3 --version
# If not found: sudo apt install python3

# Check if pip is installed
pip --version
# or
pip3 --version
# If not found: sudo apt install python3-pip

# Check if curl/wget are installed (for downloading installers)
curl --version
wget --version
# If not found: sudo apt install curl wget

# Check if git is installed (needed for pyenv)
git --version
# If not found: sudo apt install git

# Check available space in home directory
du -sh ~
df -h ~
```

### Installing Missing Tools

```bash
# Install all common prerequisites at once (Ubuntu/Debian)
sudo apt update
sudo apt install -y python3 python3-pip python3-venv curl wget git

# Verify all tools
python3 --version && pip3 --version && curl --version && git --version
```

---

## Demonstration Files

This directory contains three configuration files for different tools:

1. **`requirements.txt`** - For pip-based tools (venv, virtualenv, pyenv, uv)
2. **`environment.yml`** - For conda-based tools (Conda, Miniconda, Mamba)
3. **`pyproject.toml`** - For modern tools (uv, poetry)

```bash
# View the demonstration files
ls -lh *.txt *.yml *.toml

# Check contents
cat requirements.txt
cat environment.yml
cat pyproject.toml
```

---

## Method 1: Python venv (Built-in)

**Best for**: Simple projects, beginners, quick setup

### Step 1: Check Python Installation

```bash
# Check Python version
python3 --version
# Expected: Python 3.10.12 or higher

# Check if venv module is available
python3 -m venv --help

# If python3 is not installed:
sudo apt update
sudo apt install python3 python3-pip python3-venv

# Verify again
python3 --version
```

### Step 2: Create Virtual Environment

```bash
# Navigate to venv directory
cd /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/venv

# Create virtual environment named 'demo_env'
python3 -m venv demo_env

# Check created structure
ls -la demo_env/
# You should see: bin/, include/, lib/, pyvenv.cfg
```

### Step 3: Activate the Environment

```bash
# Activate the environment
source demo_env/bin/activate

# Verify activation (prompt should change)
# Your prompt should now show: (demo_env)

# Check Python location
which python
# Should show: /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/venv/demo_env/bin/python

# Check pip location
which pip
# Should show: /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/venv/demo_env/bin/pip
```

### Step 4: Upgrade pip

```bash
# Upgrade pip to latest version
pip install --upgrade pip

# Check pip version
pip --version
```

### Step 5: Install Packages from requirements.txt

```bash
# Install from requirements.txt
pip install -r ../requirements.txt

# This will install:
# - numpy, pandas, matplotlib, seaborn
# - scipy, scikit-learn
# - jupyter, jupyterlab
# - plotly, requests

# Monitor installation progress
```

### Step 6: Verify Installation

```bash
# List installed packages
pip list

# Check specific packages
pip show numpy
pip show pandas

# Test import in Python
python -c "import numpy; print(f'NumPy version: {numpy.__version__}')"
python -c "import pandas; print(f'Pandas version: {pandas.__version__}')"
```

### Step 7: Save Current Environment

```bash
# Export installed packages
pip freeze > installed_packages.txt

# View the file
cat installed_packages.txt
```

### Step 8: Deactivate Environment

```bash
# Deactivate
deactivate

# Verify deactivation
which python3
# Should show: /usr/bin/python3 (system Python)
```

### Step 9: Clean Up (Optional)

```bash
# To remove the environment, simply delete the directory
rm -rf demo_env
```

### Summary for venv

**Time taken**: ~2-5 minutes
**Disk space**: ~500 MB (with packages)
**Pros**: Built-in, simple, no installation needed
**Cons**: Cannot manage Python versions, pip is slower

---

## Method 2: virtualenv

**Best for**: Enhanced features, Python 2.7 support, legacy projects

### Step 1: Install virtualenv

```bash
# Check if virtualenv is already installed
virtualenv --version

# If not installed, install using pip (recommended for user)
pip install --user virtualenv

# Or use system package manager
sudo apt install python3-virtualenv  # Ubuntu/Debian

# If pip is not available:
sudo apt update
sudo apt install python3-pip

# Then install virtualenv
pip install --user virtualenv

# Verify installation
virtualenv --version
```

### Step 2: Create Virtual Environment

```bash
# Navigate to virtualenv directory
cd /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/virtualenv

# Create environment
virtualenv demo_env

# Or specify Python version explicitly
virtualenv -p python3 demo_env

# Check structure
ls -la demo_env/
```

### Step 3: Activate Environment

```bash
# Activate
source demo_env/bin/activate

# Verify
which python
which pip
```

### Step 4: Install Packages

```bash
# Upgrade pip
pip install --upgrade pip

# Install from requirements.txt
pip install -r ../requirements.txt
```

### Step 5: Test and Verify

```bash
# List packages
pip list

# Test imports
python -c "import numpy, pandas, matplotlib; print('All packages imported successfully!')"
```

### Step 6: Deactivate

```bash
deactivate
```

### Summary for virtualenv

**Time taken**: ~2-5 minutes
**Disk space**: ~500 MB (with packages)
**Pros**: More features than venv, faster creation
**Cons**: Requires installation, similar speed to venv

---

## Method 3: Conda

**Best for**: Data Science, ML/DL projects, complex dependencies

### Step 1: Check if Conda is Installed

```bash
# Check conda installation
conda --version

# If conda is not found, proceed to Step 2 to install
# If conda is already installed, skip to Step 3
```

### Step 2: Install Anaconda (if not installed)

```bash
# Download Anaconda installer (latest version)
cd ~/Downloads
wget https://repo.anaconda.com/archive/Anaconda3-2024.02-1-Linux-x86_64.sh

# Alternatively, check for the latest version at:
# https://www.anaconda.com/download

# Run installer
bash Anaconda3-2024.02-1-Linux-x86_64.sh

# Follow prompts:
# - Press Enter to read license
# - Type 'yes' to accept
# - Press Enter for default location or specify custom path
# - Type 'yes' to initialize conda

# Reload shell
source ~/.bashrc

# Verify installation
conda --version
conda info

# If conda command is still not found, manually add to PATH:
echo 'export PATH="$HOME/anaconda3/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Step 3: Create Environment from environment.yml

```bash
# Navigate to conda directory
cd /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/conda

# Create environment from YAML file
conda env create -f ../environment.yml

# This creates an environment named 'demo_env' with all packages
# Note: This may take several minutes
```

### Step 4: Activate Environment

```bash
# Activate the environment
conda activate demo_env

# Verify activation
which python
# Should show conda environment path

# Check conda info
conda info
```

### Step 5: Verify Installed Packages

```bash
# List all packages
conda list

# Check specific packages
conda list numpy
conda list pandas

# Test import
python -c "import numpy, pandas, matplotlib; print('Success!')"
```

### Step 6: Alternative - Create Environment Manually

```bash
# Create environment with specific Python version
conda create -n manual_demo python=3.11

# Activate
conda activate manual_demo

# Install packages one by one
conda install numpy pandas matplotlib seaborn scipy scikit-learn

# Or install from requirements.txt using pip
pip install -r ../requirements.txt
```

### Step 7: Export Environment

```bash
# Export current environment
conda env export > my_environment.yml

# View the exported file
cat my_environment.yml
```

### Step 8: Deactivate and Clean Up

```bash
# Deactivate
conda deactivate

# List all environments
conda env list

# Remove environment (optional)
conda env remove -n demo_env

# Clean up cached packages
conda clean --all
```

### Summary for Conda

**Time taken**: ~5-15 minutes (slow dependency resolution)
**Disk space**: ~3 GB (Anaconda) + ~1 GB (environment)
**Pros**: Excellent for Data Science, handles system dependencies
**Cons**: Slow, large installation, dependency resolution can be slow

---

## Method 4: Miniconda

**Best for**: Lightweight Conda, custom Data Science stack, limited disk space

### Step 1: Install Miniconda

```bash
# Check if conda is already installed
conda --version

# If conda is not found, install Miniconda
# Download Miniconda installer
cd ~/Downloads
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

# Alternatively, check for latest version at:
# https://docs.conda.io/en/latest/miniconda.html

# Run installer
bash Miniconda3-latest-Linux-x86_64.sh

# Follow prompts:
# - Press Enter to read license
# - Type 'yes' to accept
# - Press Enter for default location or specify custom path
# - Type 'yes' to initialize conda

# Reload shell
source ~/.bashrc

# Verify installation
conda --version

# If conda command is still not found:
echo 'export PATH="$HOME/miniconda3/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Step 2: Configure Conda Channels

```bash
# Add conda-forge channel (recommended for Data Science)
conda config --add channels conda-forge
conda config --set channel_priority strict

# View configuration
conda config --show channels
```

### Step 3: Create Environment

```bash
# Navigate to miniconda directory
cd /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/miniconda

# Option 1: From environment.yml
conda env create -f ../environment.yml

# Option 2: Manual creation
conda create -n datasci_demo python=3.11 numpy pandas matplotlib seaborn scipy scikit-learn jupyter jupyterlab plotly requests
```

### Step 4: Activate and Test

```bash
# Activate
conda activate datasci_demo

# Verify
conda list

# Test
python -c "import numpy; print(numpy.__version__)"
```

### Step 5: Deactivate

```bash
conda deactivate
```

### Summary for Miniconda

**Time taken**: ~5-15 minutes
**Disk space**: ~400 MB (Miniconda) + ~1 GB (environment)
**Pros**: Lightweight, all Conda features
**Cons**: Still slow dependency resolution

---

## Method 5: Mamba

**Best for**: Fast Conda alternative, large projects, frequent updates

### Step 1: Install Mamba

```bash
# Check if mamba is already installed
mamba --version

# If mamba is not found, choose one of the options below
```

**Option A: Install in existing Conda/Miniconda (if you already have Conda)**

```bash
# Check if conda exists
conda --version

# Install mamba in base environment
conda install mamba -n base -c conda-forge

# Verify
mamba --version
```

**Option B: Install Miniforge (Recommended - Fresh Installation)**

```bash
# Download Miniforge (includes Mamba)
cd ~/Downloads
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh

# Run installer
bash Miniforge3-Linux-x86_64.sh

# Follow prompts:
# - Press Enter to read license
# - Type 'yes' to accept
# - Press Enter for default location or specify custom path
# - Type 'yes' to initialize

# Reload shell
source ~/.bashrc

# Verify installation
mamba --version
conda --version

# If commands not found, manually add to PATH:
echo 'export PATH="$HOME/miniforge3/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Step 2: Create Environment (Fast!)

```bash
# Navigate to mamba directory
cd /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/mamba

# Create from environment.yml (MUCH FASTER than conda)
mamba env create -f ../environment.yml

# Or create manually
mamba create -n datasci_demo python=3.11 numpy pandas matplotlib seaborn scipy scikit-learn jupyter jupyterlab plotly requests

# Note the speed difference!
```

### Step 3: Activate Environment

```bash
# Still use conda to activate (not mamba)
conda activate demo_env

# Verify
which python
mamba list
```

### Step 4: Install Additional Packages

```bash
# Use mamba for faster installation
mamba install plotly

# Compare with conda (if you want to see the difference)
# conda install plotly  # This would be slower
```

### Step 5: Update Packages

```bash
# Update all packages (fast)
mamba update --all

# Or update specific package
mamba update numpy
```

### Step 6: Deactivate

```bash
conda deactivate
```

### Summary for Mamba

**Time taken**: ~1-3 minutes (10-100x faster than Conda!)
**Disk space**: ~400 MB (Miniforge) + ~1 GB (environment)
**Pros**: Extremely fast, fully compatible with Conda
**Cons**: Slightly more complex setup

---

## Method 6: pyenv

**Best for**: Managing multiple Python versions, version-specific testing

### Step 1: Check if pyenv is Installed

```bash
# Check if pyenv is already installed
pyenv --version

# If pyenv is not found, proceed to install
```

### Step 2: Install Build Dependencies (Required for pyenv)

```bash
# Install build dependencies for compiling Python
sudo apt update
sudo apt install -y make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev \
libffi-dev liblzma-dev

# If you don't have sudo access, skip this and use system Python with venv instead
```

### Step 3: Install pyenv

```bash
# Check if git is installed (required for pyenv installer)
git --version

# If git is not installed:
sudo apt install git

# Install pyenv using the installer
curl https://pyenv.run | bash

# Add to ~/.bashrc
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc

# Reload shell
source ~/.bashrc

# Verify installation
pyenv --version

# If pyenv command not found after reloading:
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
```

### Step 4: Install pyenv-virtualenv Plugin

```bash
# Check if plugin is already installed
ls $(pyenv root)/plugins/

# Clone the plugin if not present
git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv

# Add to ~/.bashrc
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc

# Reload shell
source ~/.bashrc

# Verify plugin is loaded
pyenv commands | grep virtualenv
```

### Step 5: Install Python Versions

```bash
# List available Python versions
pyenv install --list | grep "^\s*3\.[0-9]*\.[0-9]*$" | tail -10

# Install Python 3.11
pyenv install 3.11.7

# Install Python 3.10 (for testing multiple versions)
pyenv install 3.10.13

# List installed versions
pyenv versions
```

### Step 6: Create Virtual Environment

```bash
# Navigate to pyenv directory
cd /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/pyenv

# Create virtualenv with Python 3.11
pyenv virtualenv 3.11.7 datasci_demo

# Or create another with Python 3.10
pyenv virtualenv 3.10.13 datasci_demo_py310
```

### Step 7: Activate Environment

```bash
# Activate
pyenv activate datasci_demo

# Verify Python version
python --version
# Should show: Python 3.11.7

# Verify location
which python
```

### Step 8: Install Packages

```bash
# Upgrade pip
pip install --upgrade pip

# Install from requirements.txt
pip install -r ../requirements.txt
```

### Step 9: Set Local Environment

```bash
# Set this environment as default for this directory
pyenv local datasci_demo

# Now, whenever you cd into this directory, the environment activates automatically
cd ..
cd pyenv
# Environment should activate automatically

# Check
python --version
```

### Step 10: Deactivate

```bash
# Deactivate
pyenv deactivate

# Or just cd out of the directory (if using local)
```

### Step 11: Clean Up

```bash
# List virtualenvs
pyenv virtualenvs

# Delete virtualenv
pyenv virtualenv-delete datasci_demo

# Uninstall Python version (optional)
pyenv uninstall 3.11.7
```

### Summary for pyenv

**Time taken**: ~10-20 minutes (including Python compilation)
**Disk space**: ~200 MB per Python version + ~500 MB per environment
**Pros**: Multiple Python versions, automatic activation
**Cons**: Complex setup, requires compilation

---

## Method 7: uv (Modern & Fast)

**Best for**: Speed-critical workflows, modern projects, CI/CD

### Step 1: Install uv

```bash
# Check if uv is already installed
uv --version

# If not installed, choose one of the installation methods below:

# Option 1: Install using the official installer (recommended)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Option 2: Install with pip
pip install --user uv

# Option 3: Install with pipx (if you have pipx)
pipx install uv

# If pipx is not installed and you want to use it:
pip install --user pipx
pipx install uv

# Reload shell to update PATH
source ~/.bashrc

# Verify installation
uv --version

# If uv command not found, manually add to PATH:
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
uv --version
```

### Step 2: Create Virtual Environment

```bash
# Navigate to uv directory
cd /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/uv

# Create virtual environment (VERY FAST!)
uv venv demo_env

# Or specify Python version
uv venv --python 3.11 demo_env

# Check structure
ls -la demo_env/
```

### Step 3: Activate Environment

```bash
# Activate
source demo_env/bin/activate

# Verify
which python
which pip
```

### Step 4: Install Packages (Lightning Fast!)

```bash
# Install from requirements.txt using uv
uv pip install -r ../requirements.txt

# Note: This is 10-100x faster than regular pip!
# Watch the speed difference

# Or install packages directly
uv pip install numpy pandas matplotlib seaborn
```

### Step 5: Verify Installation

```bash
# List packages
uv pip list

# Or use regular pip
pip list

# Test imports
python -c "import numpy, pandas, matplotlib; print('Success!')"
```

### Step 6: Advanced Features

```bash
# Compile requirements with exact versions
uv pip compile ../requirements.txt -o requirements-locked.txt

# Install from locked file
uv pip sync requirements-locked.txt

# Upgrade all packages
uv pip install --upgrade -r ../requirements.txt

# Freeze current state
uv pip freeze > installed.txt
```

### Step 7: Deactivate

```bash
deactivate
```

### Step 8: Clean Up

```bash
# Remove environment
rm -rf demo_env
```

### Summary for uv

**Time taken**: ~30 seconds - 1 minute (10-100x faster!)
**Disk space**: ~500 MB (with packages)
**Pros**: Extremely fast, modern, pip-compatible
**Cons**: Relatively new, smaller community

---

## Comparison of Results

After testing all methods, here's a comparison:

| Method | Setup Time | Install Time | Total Time | Disk Space | Difficulty |
|--------|------------|--------------|------------|------------|------------|
| **venv** | 10 sec | 3-5 min | ~5 min | 500 MB | Easy |
| **virtualenv** | 30 sec | 3-5 min | ~5 min | 500 MB | Easy |
| **Conda** | 10 min | 10-15 min | ~25 min | 4 GB | Medium |
| **Miniconda** | 5 min | 10-15 min | ~20 min | 1.5 GB | Medium |
| **Mamba** | 5 min | 1-3 min | ~8 min | 1.5 GB | Medium |
| **pyenv** | 15 min | 3-5 min | ~20 min | 700 MB | Hard |
| **uv** | 30 sec | 30-60 sec | ~1.5 min | 500 MB | Easy |

### Recommendations by Scenario:

1. **Quick Demo in Class**: `uv` or `venv`
2. **Data Science Course**: `Mamba` or `Miniconda`
3. **GPU/Deep Learning**: `Mamba` or `Conda`
4. **Multiple Python Versions**: `pyenv`
5. **Production Deployment**: `venv` or `uv`
6. **Limited Disk Space**: `uv` or `venv`
7. **Maximum Speed**: `uv`

---

## Troubleshooting

### Issue 1: "No space left on device"

```bash
# Check disk usage
df -h /NFSDISK2/pyare/TAship/DSP315/

# Clean up conda/mamba caches
conda clean --all
mamba clean --all

# Clean pip cache
pip cache purge

# Use custom cache directory (see main README)
```

### Issue 2: "python3: command not found"

```bash
# Install Python
sudo apt update
sudo apt install python3 python3-pip python3-venv
```

### Issue 3: "Permission denied"

```bash
# Don't use sudo with virtual environments
# Use --user flag for system-wide installations
pip install --user virtualenv
```

### Issue 4: Conda/Mamba is slow

```bash
# Use Mamba instead of Conda
conda install mamba -c conda-forge

# Or install Miniforge
```

### Issue 5: uv not found after installation

```bash
# Add to PATH in ~/.bashrc
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Issue 6: pyenv build fails

```bash
# Install all build dependencies
sudo apt install -y make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev \
libffi-dev liblzma-dev
```

---

## Testing Checklist

Use this checklist when demonstrating each method:

- [ ] Environment created successfully
- [ ] Environment activated (prompt changed)
- [ ] Correct Python version (`python --version`)
- [ ] Correct pip location (`which pip`)
- [ ] Packages installed from requirements.txt
- [ ] Package import successful (`import numpy, pandas`)
- [ ] Environment can be deactivated
- [ ] Environment can be reactivated
- [ ] Environment can be deleted/removed

---

## Classroom Demonstration Script

### Quick 5-Minute Demo (venv)

```bash
# 1. Show current Python
python3 --version

# 2. Create environment
python3 -m venv quick_demo

# 3. Activate
source quick_demo/bin/activate

# 4. Install one package
pip install numpy

# 5. Test
python -c "import numpy; print(numpy.__version__)"

# 6. Deactivate
deactivate

# 7. Clean up
rm -rf quick_demo
```

### Quick 2-Minute Demo (uv - Fastest)

```bash
# 1. Create environment
uv venv quick_demo

# 2. Activate
source quick_demo/bin/activate

# 3. Install packages (FAST!)
uv pip install numpy pandas matplotlib

# 4. Test
python -c "import numpy, pandas; print('Success!')"

# 5. Deactivate
deactivate
```

---

**Last Updated**: October 31, 2025
**Tested On**: Ubuntu 22.04 LTS, Python 3.10.12
**For Course**: DSP315 - Data Science
