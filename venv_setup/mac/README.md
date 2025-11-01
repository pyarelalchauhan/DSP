# macOS Virtual Environment Setup - Step-by-Step Guide

This guide provides detailed, step-by-step instructions for setting up Python virtual environments on macOS. Each method is tested and documented for classroom demonstration.

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
10. [Method 8: Homebrew + pyenv](#method-8-homebrew--pyenv-mac-specific)
11. [Comparison of Results](#comparison-of-results)
12. [Troubleshooting](#troubleshooting)

---

## Prerequisites

Before starting, ensure you have the following tools and check your system:

### System Checks

```bash
# Check your current directory
pwd

# Check macOS version
sw_vers

# Check available disk space
df -h ~

# Check architecture (Intel vs Apple Silicon)
uname -m
# x86_64 = Intel Mac
# arm64 = Apple Silicon (M1/M2/M3)
```

### Required Tools Check

```bash
# Check if Python3 is installed (macOS 10.15+ has Python 3)
python3 --version

# Check if pip is installed
pip3 --version

# Check if curl is installed (pre-installed on macOS)
curl --version

# Check if git is installed
git --version
# If not found, it will prompt you to install Xcode Command Line Tools

# Check if Homebrew is installed (recommended for macOS)
brew --version
```

### Installing Homebrew (Recommended for macOS)

```bash
# Check if Homebrew is installed
brew --version

# If not installed, install Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# For Apple Silicon Macs, add Homebrew to PATH:
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"

# Verify installation
brew --version
```

### Installing Command Line Tools

```bash
# Install Xcode Command Line Tools (includes git, make, etc.)
xcode-select --install

# Verify installation
xcode-select -p
# Should output: /Library/Developer/CommandLineTools
```

### Installing Missing Tools

```bash
# Install Python 3 using Homebrew (if needed)
brew install python3

# Verify all tools
python3 --version && pip3 --version && git --version

# Install wget (useful for downloads)
brew install wget
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
# Expected: Python 3.9+ on recent macOS

# Check if venv module is available
python3 -m venv --help

# If python3 is not installed or too old:
brew install python3

# Verify again
python3 --version
```

### Step 2: Create Virtual Environment

```bash
# Navigate to venv directory
cd ~/path/to/venv_setup/mac/venv

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
# Should show: ~/path/to/venv_setup/mac/venv/demo_env/bin/python

# Check pip location
which pip
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
pip install -r ../../requirements.txt

# This will install:
# - numpy, pandas, matplotlib, seaborn
# - scipy, scikit-learn
# - jupyter, jupyterlab
# - plotly, requests
```

### Step 6: Verify Installation

```bash
# List installed packages
pip list

# Test import in Python
python -c "import numpy; print(f'NumPy version: {numpy.__version__}')"
python -c "import pandas; print(f'Pandas version: {pandas.__version__}')"
```

### Step 7: Deactivate Environment

```bash
# Deactivate
deactivate

# Verify deactivation
which python
# Should show: /usr/bin/python3 or /opt/homebrew/bin/python3
```

### Step 8: Clean Up (Optional)

```bash
# To remove the environment, simply delete the directory
rm -rf demo_env
```

### Summary for venv

**Time taken**: ~2-5 minutes
**Disk space**: ~500 MB (with packages)
**Pros**: Built-in, simple, no installation needed
**Cons**: Cannot manage Python versions

---

## Method 2: virtualenv

**Best for**: Enhanced features, multiple Python versions

### Step 1: Install virtualenv

```bash
# Check if virtualenv is already installed
virtualenv --version

# If not installed, install using pip
pip3 install --user virtualenv

# Or install using Homebrew
brew install virtualenv

# Verify installation
virtualenv --version
```

### Step 2: Create Virtual Environment

```bash
# Navigate to virtualenv directory
cd ~/path/to/venv_setup/mac/virtualenv

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
pip install -r ../../requirements.txt
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
**Cons**: Requires installation

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
# Download Anaconda installer for macOS
cd ~/Downloads

# For Intel Macs:
wget https://repo.anaconda.com/archive/Anaconda3-2024.02-1-MacOSX-x86_64.sh

# For Apple Silicon Macs (M1/M2/M3):
wget https://repo.anaconda.com/archive/Anaconda3-2024.02-1-MacOSX-arm64.sh

# Alternatively, check for latest version at:
# https://www.anaconda.com/download

# Run installer (replace with your downloaded file)
bash Anaconda3-2024.02-1-MacOSX-*.sh

# Follow prompts:
# - Press Enter to read license
# - Type 'yes' to accept
# - Press Enter for default location or specify custom path
# - Type 'yes' to initialize conda

# Reload shell
source ~/.zshrc  # or ~/.bash_profile for older macOS

# Verify installation
conda --version
conda info

# If conda command is still not found, manually add to PATH:
echo 'export PATH="$HOME/anaconda3/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### Step 3: Create Environment from environment.yml

```bash
# Navigate to conda directory
cd ~/path/to/venv_setup/mac/conda

# Create environment from YAML file
conda env create -f ../../environment.yml

# This creates an environment named 'DSE315_Demo' with all packages
```

### Step 4: Activate Environment

```bash
# Activate the environment
conda activate DSE315_Demo

# Verify activation
which python

# Check conda info
conda info
```

### Step 5: Verify Installed Packages

```bash
# List all packages
conda list

# Test import
python -c "import numpy, pandas, matplotlib; print('Success!')"
```

### Step 6: Deactivate and Clean Up

```bash
# Deactivate
conda deactivate

# List all environments
conda env list

# Remove environment (optional)
conda env remove -n DSE315_Demo

# Clean up cached packages
conda clean --all
```

### Summary for Conda

**Time taken**: ~5-15 minutes
**Disk space**: ~3 GB (Anaconda) + ~1 GB (environment)
**Pros**: Excellent for Data Science, handles system dependencies
**Cons**: Slow, large installation

---

## Method 4: Miniconda

**Best for**: Lightweight Conda, custom Data Science stack

### Step 1: Install Miniconda

```bash
# Check if conda is already installed
conda --version

# If conda is not found, install Miniconda
cd ~/Downloads

# For Intel Macs:
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh

# For Apple Silicon Macs (M1/M2/M3):
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh

# Alternatively, download from:
# https://docs.conda.io/en/latest/miniconda.html

# Run installer
bash Miniconda3-latest-MacOSX-*.sh

# Follow prompts
# Reload shell
source ~/.zshrc

# Verify installation
conda --version

# If conda command is still not found:
echo 'export PATH="$HOME/miniconda3/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
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
cd ~/path/to/venv_setup/mac/miniconda

# From environment.yml
conda env create -f ../../environment.yml

# Or create manually
conda create -n DSE315_Demo python=3.11 numpy pandas matplotlib seaborn scipy scikit-learn jupyter jupyterlab plotly requests
```

### Step 4: Activate and Test

```bash
# Activate
conda activate DSE315_Demo

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

**Best for**: Fast Conda alternative, large projects

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
# Download Miniforge
cd ~/Downloads

# For Intel Macs:
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-x86_64.sh

# For Apple Silicon Macs (M1/M2/M3):
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh

# Or use curl:
# curl -L -O https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh

# Run installer
bash Miniforge3-MacOSX-*.sh

# Follow prompts
# Reload shell
source ~/.zshrc

# Verify installation
mamba --version
conda --version

# If commands not found, manually add to PATH:
echo 'export PATH="$HOME/miniforge3/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### Step 2: Create Environment (Fast!)

```bash
# Navigate to mamba directory
cd ~/path/to/venv_setup/mac/mamba

# Create from environment.yml (MUCH FASTER than conda)
mamba env create -f ../../environment.yml

# Or create manually
mamba create -n DSE315_Demo python=3.11 numpy pandas matplotlib seaborn scipy scikit-learn jupyter jupyterlab plotly requests
```

### Step 3: Activate Environment

```bash
# Still use conda to activate (not mamba)
conda activate DSE315_Demo

# Verify
which python
mamba list
```

### Step 4: Deactivate

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

### Step 2: Install pyenv using Homebrew (Easiest on macOS)

```bash
# Install pyenv using Homebrew
brew install pyenv

# Install pyenv-virtualenv plugin
brew install pyenv-virtualenv

# Add to shell configuration
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc

# For bash users, replace ~/.zshrc with ~/.bash_profile

# Reload shell
source ~/.zshrc

# Verify installation
pyenv --version
```

### Step 3: Install Python Versions

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

### Step 4: Create Virtual Environment

```bash
# Navigate to pyenv directory
cd ~/path/to/venv_setup/mac/pyenv

# Create virtualenv with Python 3.11
pyenv virtualenv 3.11.7 datasci_demo

# Or create another with Python 3.10
pyenv virtualenv 3.10.13 datasci_demo_py310
```

### Step 5: Activate Environment

```bash
# Activate
pyenv activate datasci_demo

# Verify Python version
python --version

# Verify location
which python
```

### Step 6: Install Packages

```bash
# Upgrade pip
pip install --upgrade pip

# Install from requirements.txt
pip install -r ../../requirements.txt
```

### Step 7: Set Local Environment

```bash
# Set this environment as default for this directory
pyenv local datasci_demo

# Now, whenever you cd into this directory, the environment activates automatically
```

### Step 8: Deactivate

```bash
# Deactivate
pyenv deactivate
```

### Summary for pyenv

**Time taken**: ~5-10 minutes (precompiled binaries on macOS)
**Disk space**: ~200 MB per Python version + ~500 MB per environment
**Pros**: Multiple Python versions, automatic activation, Homebrew integration
**Cons**: Requires Homebrew

---

## Method 7: uv (Modern & Fast)

**Best for**: Speed-critical workflows, modern projects, CI/CD

### Step 1: Install uv

```bash
# Check if uv is already installed
uv --version

# If not installed, choose one of the installation methods:

# Option 1: Install using the official installer (recommended)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Option 2: Install with Homebrew
brew install uv

# Option 3: Install with pip
pip3 install --user uv

# Option 4: Install with pipx
pipx install uv

# Reload shell to update PATH
source ~/.zshrc

# Verify installation
uv --version

# If uv command not found, manually add to PATH:
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### Step 2: Create Virtual Environment

```bash
# Navigate to uv directory
cd ~/path/to/venv_setup/mac/uv

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
uv pip install -r ../../requirements.txt

# Note: This is 10-100x faster than regular pip!

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

### Step 6: Deactivate

```bash
deactivate
```

### Summary for uv

**Time taken**: ~30 seconds - 1 minute (10-100x faster!)
**Disk space**: ~500 MB (with packages)
**Pros**: Extremely fast, modern, pip-compatible
**Cons**: Relatively new, smaller community

---

## Method 8: Homebrew + pyenv (Mac-Specific)

**Best for**: macOS users who want the native package manager experience

This is the recommended approach for macOS users combining Homebrew's ease of use with pyenv's Python version management.

### Complete Setup

```bash
# 1. Install Homebrew (if not already installed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 2. Install pyenv and pyenv-virtualenv
brew install pyenv pyenv-virtualenv

# 3. Configure shell
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc
source ~/.zshrc

# 4. Install Python versions
pyenv install 3.11.7

# 5. Create environment
pyenv virtualenv 3.11.7 datasci_demo

# 6. Activate
pyenv activate datasci_demo

# 7. Install packages
pip install -r requirements.txt
```

---

## Comparison of Results

After testing all methods, here's a comparison:

| Method | Setup Time | Install Time | Total Time | Disk Space | Difficulty | macOS Native |
|--------|------------|--------------|------------|------------|------------|--------------|
| **venv** | 10 sec | 3-5 min | ~5 min | 500 MB | Easy |  |
| **virtualenv** | 30 sec | 3-5 min | ~5 min | 500 MB | Easy |  |
| **Conda** | 10 min | 10-15 min | ~25 min | 4 GB | Medium |  |
| **Miniconda** | 5 min | 10-15 min | ~20 min | 1.5 GB | Medium |  |
| **Mamba** | 5 min | 1-3 min | ~8 min | 1.5 GB | Medium |  |
| **pyenv** | 2 min | 3-5 min | ~7 min | 700 MB | Easy |  |
| **uv** | 30 sec | 30-60 sec | ~1.5 min | 500 MB | Easy |  |
| **Homebrew+pyenv** | 3 min | 3-5 min | ~8 min | 700 MB | Easy |  |

### Recommendations by Scenario:

1. **Quick Demo in Class**: `uv` or `venv`
2. **Data Science Course**: `Mamba` or `Miniconda`
3. **macOS Native Experience**: `Homebrew + pyenv`
4. **GPU/Deep Learning**: `Mamba` or `Conda`
5. **Multiple Python Versions**: `pyenv` (via Homebrew)
6. **Production Deployment**: `venv` or `uv`
7. **Maximum Speed**: `uv`

---

## Troubleshooting

### Issue 1: "command not found: python"

```bash
# Install Python using Homebrew
brew install python3

# Or use system Python
python3 --version
```

### Issue 2: "command not found: conda"

```bash
# Check if conda is in PATH
echo $PATH | grep conda

# Manually add to PATH
echo 'export PATH="$HOME/anaconda3/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### Issue 3: Apple Silicon (M1/M2/M3) compatibility issues

```bash
# Check architecture
uname -m

# For Rosetta apps on Apple Silicon:
arch -x86_64 /bin/bash

# Install x86_64 version of tools if needed
arch -x86_64 brew install python3
```

### Issue 4: Permission denied errors

```bash
# Don't use sudo with virtual environments
# Use --user flag for global installations
pip install --user package_name

# Or use Homebrew
brew install package_name
```

### Issue 5: SSL certificate verification failed

```bash
# Update certificates
pip install --upgrade certifi

# Or use Homebrew Python which includes updated certificates
brew install python3
```

### Issue 6: pyenv build fails

```bash
# Install build dependencies using Homebrew
brew install openssl readline sqlite3 xz zlib

# Set compiler flags for pyenv
export LDFLAGS="-L/opt/homebrew/opt/openssl@3/lib"
export CPPFLAGS="-I/opt/homebrew/opt/openssl@3/include"
```

### Issue 7: Shell configuration (zsh vs bash)

```bash
# Check current shell
echo $SHELL

# For zsh (default on macOS Catalina+)
# Use ~/.zshrc

# For bash (older macOS)
# Use ~/.bash_profile or ~/.bashrc
```

---

## macOS-Specific Tips

### 1. Use Homebrew for Package Management

```bash
# Install Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install common tools
brew install python3 git wget
```

### 2. Apple Silicon Considerations

```bash
# Check if you're on Apple Silicon
uname -m
# arm64 = Apple Silicon
# x86_64 = Intel

# Some packages may need Rosetta 2
softwareupdate --install-rosetta
```

### 3. Use pyenv via Homebrew

```bash
# Easiest Python version management on macOS
brew install pyenv pyenv-virtualenv
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

**For Course**: DSP315 - Data Science
