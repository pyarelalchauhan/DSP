# Windows Virtual Environment Setup - Step-by-Step Guide

This guide provides detailed, step-by-step instructions for setting up Python virtual environments on Windows. Each method is tested and documented for classroom demonstration.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Demonstration Files](#demonstration-files)
3. [Method 1: Python venv (Built-in)](#method-1-python-venv-built-in)
4. [Method 2: virtualenv](#method-2-virtualenv)
5. [Method 3: Conda](#method-3-conda)
6. [Method 4: Miniconda](#method-4-miniconda)
7. [Method 5: Mamba](#method-5-mamba)
8. [Method 6: pyenv-win](#method-6-pyenv-win)
9. [Method 7: uv (Modern & Fast)](#method-7-uv-modern--fast)
10. [Method 8: Windows Terminal + winget](#method-8-windows-terminal--winget-windows-specific)
11. [Comparison of Results](#comparison-of-results)
12. [Troubleshooting](#troubleshooting)

---

## Prerequisites

Before starting, ensure you have the following tools and check your system:

### System Checks

Open **PowerShell** or **Command Prompt** (Run as Administrator):

```powershell
# Check Windows version
winver

# Check available disk space
wmic logicaldisk get size,freespace,caption

# Check if running PowerShell
$PSVersionTable.PSVersion

# Check system architecture
systeminfo | findstr /C:"System Type"
```

### Required Tools Check

```powershell
# Check if Python is installed
python --version
# or
py --version

# Check if pip is installed
pip --version
# or
py -m pip --version

# Check if Git is installed
git --version
```

### Installing Python on Windows

**Option 1: Official Python Installer (Recommended)**

```powershell
# Download from: https://www.python.org/downloads/

# During installation:
#  Check "Add Python to PATH"
#  Check "Install for all users" (optional)
#  Select "Customize installation"
#  Check "pip", "py launcher", "for all users"

# Verify installation
python --version
pip --version
```

**Option 2: Microsoft Store**

```powershell
# Search "Python" in Microsoft Store
# Install Python 3.11 or later

# Verify
python --version
```

**Option 3: winget (Windows Package Manager)**

```powershell
# Check if winget is installed
winget --version

# Install Python
winget install Python.Python.3.11

# Verify
python --version
```

### Installing Git for Windows

```powershell
# Check if git is installed
git --version

# If not installed:
# Option 1: Download from https://git-scm.com/download/win
# Option 2: Use winget
winget install Git.Git

# Verify
git --version
```

### Installing Windows Terminal (Recommended)

```powershell
# Install Windows Terminal for better experience
winget install Microsoft.WindowsTerminal

# Or install from Microsoft Store: "Windows Terminal"
```

---

## Demonstration Files

This directory contains three configuration files for different tools:

1. **`requirements.txt`** - For pip-based tools (venv, virtualenv, pyenv-win, uv)
2. **`environment.yml`** - For conda-based tools (Conda, Miniconda, Mamba)
3. **`pyproject.toml`** - For modern tools (uv, poetry)

```powershell
# View the demonstration files (in PowerShell)
dir *.txt,*.yml,*.toml

# Check contents
type requirements.txt
type environment.yml
type pyproject.toml
```

---

## Method 1: Python venv (Built-in)

**Best for**: Simple projects, beginners, quick setup

### Step 1: Check Python Installation

```powershell
# Check Python version
python --version
# Expected: Python 3.8+ on Windows

# Or use py launcher
py --version

# Check if venv module is available
python -m venv --help

# If python is not installed:
# Download from https://www.python.org/downloads/
# or use: winget install Python.Python.3.11

# Verify again
python --version
```

### Step 2: Create Virtual Environment

```powershell
# Navigate to venv directory
cd C:\path\to\venv_setup\windows\venv

# Create virtual environment named 'demo_env'
python -m venv demo_env

# Check created structure
dir demo_env
# You should see: Scripts\, Lib\, Include\, pyvenv.cfg
```

### Step 3: Activate the Environment

**In PowerShell:**

```powershell
# Activate the environment
.\demo_env\Scripts\Activate.ps1

# If you get execution policy error:
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# Then try activating again
.\demo_env\Scripts\Activate.ps1

# Verify activation (prompt should change)
# Your prompt should now show: (demo_env)

# Check Python location
where python
# Should show: C:\path\to\venv_setup\windows\venv\demo_env\Scripts\python.exe

# Check pip location
where pip
```

**In Command Prompt:**

```cmd
# Activate the environment
demo_env\Scripts\activate.bat

# Verify
where python
where pip
```

### Step 4: Upgrade pip

```powershell
# Upgrade pip to latest version
python -m pip install --upgrade pip

# Check pip version
pip --version
```

### Step 5: Install Packages from requirements.txt

```powershell
# Install from requirements.txt
pip install -r ..\..\requirements.txt

# This will install:
# - numpy, pandas, matplotlib, seaborn
# - scipy, scikit-learn
# - jupyter, jupyterlab
# - plotly, requests
```

### Step 6: Verify Installation

```powershell
# List installed packages
pip list

# Test import in Python
python -c "import numpy; print(f'NumPy version: {numpy.__version__}')"
python -c "import pandas; print(f'Pandas version: {pandas.__version__}')"
```

### Step 7: Deactivate Environment

```powershell
# Deactivate
deactivate

# Verify deactivation
where python
# Should show: C:\Users\<username>\AppData\Local\Programs\Python\...
```

### Step 8: Clean Up (Optional)

```powershell
# To remove the environment, simply delete the directory
rmdir /s demo_env
# or in PowerShell:
Remove-Item -Recurse -Force demo_env
```

### Summary for venv

**Time taken**: ~2-5 minutes
**Disk space**: ~500 MB (with packages)
**Pros**: Built-in, simple, no installation needed
**Cons**: Cannot manage Python versions, execution policy issues

---

## Method 2: virtualenv

**Best for**: Enhanced features, more flexibility than venv

### Step 1: Install virtualenv

```powershell
# Check if virtualenv is already installed
virtualenv --version

# If not installed, install using pip
pip install --user virtualenv

# Or install globally
pip install virtualenv

# Verify installation
virtualenv --version
```

### Step 2: Create Virtual Environment

```powershell
# Navigate to virtualenv directory
cd C:\path\to\venv_setup\windows\virtualenv

# Create environment
virtualenv demo_env

# Or specify Python version explicitly
virtualenv -p python demo_env

# Check structure
dir demo_env
```

### Step 3: Activate Environment

**PowerShell:**

```powershell
# Activate
.\demo_env\Scripts\Activate.ps1

# Verify
where python
where pip
```

**Command Prompt:**

```cmd
# Activate
demo_env\Scripts\activate.bat
```

### Step 4: Install Packages

```powershell
# Upgrade pip
pip install --upgrade pip

# Install from requirements.txt
pip install -r ..\..\requirements.txt
```

### Step 5: Test and Verify

```powershell
# List packages
pip list

# Test imports
python -c "import numpy, pandas, matplotlib; print('All packages imported successfully!')"
```

### Step 6: Deactivate

```powershell
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

```powershell
# Check conda installation
conda --version

# If conda is not found, proceed to Step 2 to install
# If conda is already installed, skip to Step 3
```

### Step 2: Install Anaconda (if not installed)

```powershell
# Download Anaconda installer for Windows
# Visit: https://www.anaconda.com/download

# For 64-bit Windows (most common):
# Download: Anaconda3-2024.02-1-Windows-x86_64.exe

# Run the installer:
# - Choose "Just Me" or "All Users"
# -  Check "Add Anaconda to PATH" (not recommended by installer, but useful)
# -  Check "Register Anaconda as default Python"

# After installation, open NEW PowerShell/CMD window

# Verify installation
conda --version
conda info

# If conda command is still not found, manually add to PATH:
# System Properties > Environment Variables > Path
# Add: C:\Users\<username>\anaconda3\Scripts
# Add: C:\Users\<username>\anaconda3

# Or in PowerShell:
$env:Path += ";C:\Users\$env:USERNAME\anaconda3;C:\Users\$env:USERNAME\anaconda3\Scripts"
```

### Step 3: Initialize Conda for PowerShell

```powershell
# Initialize conda for PowerShell
conda init powershell

# Close and reopen PowerShell

# Verify
conda --version
```

### Step 4: Create Environment from environment.yml

```powershell
# Navigate to conda directory
cd C:\path\to\venv_setup\windows\conda

# Create environment from YAML file
conda env create -f ..\..\environment.yml

# This creates an environment named 'DSE315_Demo' with all packages
```

### Step 5: Activate Environment

```powershell
# Activate the environment
conda activate DSE315_Demo

# Verify activation
where python

# Check conda info
conda info
```

### Step 6: Verify Installed Packages

```powershell
# List all packages
conda list

# Test import
python -c "import numpy, pandas, matplotlib; print('Success!')"
```

### Step 7: Deactivate and Clean Up

```powershell
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
**Cons**: Slow, large installation, PATH configuration

---

## Method 4: Miniconda

**Best for**: Lightweight Conda, custom Data Science stack

### Step 1: Install Miniconda

```powershell
# Check if conda is already installed
conda --version

# If conda is not found, install Miniconda
# Download from: https://docs.conda.io/en/latest/miniconda.html

# For 64-bit Windows:
# Download: Miniconda3-latest-Windows-x86_64.exe

# Run the installer
# - Choose "Just Me" or "All Users"
# - Consider adding to PATH during installation

# Open NEW PowerShell/CMD window

# Initialize conda
conda init powershell

# Restart PowerShell

# Verify installation
conda --version

# If conda command is still not found, add to PATH:
$env:Path += ";C:\Users\$env:USERNAME\miniconda3;C:\Users\$env:USERNAME\miniconda3\Scripts"
```

### Step 2: Configure Conda Channels

```powershell
# Add conda-forge channel (recommended for Data Science)
conda config --add channels conda-forge
conda config --set channel_priority strict

# View configuration
conda config --show channels
```

### Step 3: Create Environment

```powershell
# Navigate to miniconda directory
cd C:\path\to\venv_setup\windows\miniconda

# From environment.yml
conda env create -f ..\..\environment.yml

# Or create manually
conda create -n DSE315_Demo python=3.11 numpy pandas matplotlib seaborn scipy scikit-learn jupyter jupyterlab plotly requests
```

### Step 4: Activate and Test

```powershell
# Activate
conda activate DSE315_Demo

# Verify
conda list

# Test
python -c "import numpy; print(numpy.__version__)"
```

### Step 5: Deactivate

```powershell
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

```powershell
# Check if mamba is already installed
mamba --version

# If mamba is not found, choose one of the options below
```

**Option A: Install in existing Conda/Miniconda (if you already have Conda)**

```powershell
# Check if conda exists
conda --version

# Install mamba in base environment
conda install mamba -n base -c conda-forge

# Verify
mamba --version
```

**Option B: Install Miniforge (Recommended - Fresh Installation)**

```powershell
# Download Miniforge for Windows
# Visit: https://github.com/conda-forge/miniforge

# For 64-bit Windows:
# Download: Miniforge3-Windows-x86_64.exe

# Run the installer
# - Choose "Just Me" or "All Users"
# - Consider adding to PATH

# Open NEW PowerShell/CMD window

# Initialize
conda init powershell

# Restart PowerShell

# Verify installation
mamba --version
conda --version

# If commands not found, manually add to PATH:
$env:Path += ";C:\Users\$env:USERNAME\miniforge3;C:\Users\$env:USERNAME\miniforge3\Scripts"
```

### Step 2: Create Environment (Fast!)

```powershell
# Navigate to mamba directory
cd C:\path\to\venv_setup\windows\mamba

# Create from environment.yml (MUCH FASTER than conda)
mamba env create -f ..\..\environment.yml

# Or create manually
mamba create -n DSE315_Demo python=3.11 numpy pandas matplotlib seaborn scipy scikit-learn jupyter jupyterlab plotly requests
```

### Step 3: Activate Environment

```powershell
# Still use conda to activate (not mamba)
conda activate DSE315_Demo

# Verify
where python
mamba list
```

### Step 4: Deactivate

```powershell
conda deactivate
```

### Summary for Mamba

**Time taken**: ~1-3 minutes (10-100x faster than Conda!)
**Disk space**: ~400 MB (Miniforge) + ~1 GB (environment)
**Pros**: Extremely fast, fully compatible with Conda
**Cons**: Slightly more complex setup

---

## Method 6: pyenv-win

**Best for**: Managing multiple Python versions on Windows

### Step 1: Check if pyenv-win is Installed

```powershell
# Check if pyenv is already installed
pyenv --version

# If pyenv is not found, proceed to install
```

### Step 2: Install pyenv-win

**Option A: Using PowerShell (Recommended)**

```powershell
# Install using PowerShell
Invoke-WebRequest -UseBasicParsing -Uri "https://raw.githubusercontent.com/pyenv-win/pyenv-win/master/pyenv-win/install-pyenv-win.ps1" -OutFile "./install-pyenv-win.ps1"; &"./install-pyenv-win.ps1"

# Close and reopen PowerShell

# Verify installation
pyenv --version
```

**Option B: Using Git**

```powershell
# Clone the repository
git clone https://github.com/pyenv-win/pyenv-win.git "$HOME\.pyenv"

# Add to environment variables
[System.Environment]::SetEnvironmentVariable('PYENV',$HOME + "\.pyenv\pyenv-win\","User")
[System.Environment]::SetEnvironmentVariable('PYENV_HOME',$HOME + "\.pyenv\pyenv-win\","User")
[System.Environment]::SetEnvironmentVariable('path', $env:USERPROFILE + "\.pyenv\pyenv-win\bin;" + $env:USERPROFILE + "\.pyenv\pyenv-win\shims;" + [System.Environment]::GetEnvironmentVariable('path', "User"),"User")

# Close and reopen PowerShell

# Verify
pyenv --version
```

### Step 3: Install Python Versions

```powershell
# List available Python versions
pyenv install --list

# Install Python 3.11
pyenv install 3.11.7

# Install Python 3.10 (for testing multiple versions)
pyenv install 3.10.13

# List installed versions
pyenv versions
```

### Step 4: Set Python Version

```powershell
# Set global Python version
pyenv global 3.11.7

# Or set local version for specific directory
cd C:\path\to\venv_setup\windows\pyenv
pyenv local 3.11.7

# Verify
python --version
```

### Step 5: Create Virtual Environment

```powershell
# Navigate to pyenv directory
cd C:\path\to\venv_setup\windows\pyenv

# Create virtual environment using venv
python -m venv datasci_demo

# Activate
.\datasci_demo\Scripts\Activate.ps1
```

### Step 6: Install Packages

```powershell
# Upgrade pip
pip install --upgrade pip

# Install from requirements.txt
pip install -r ..\..\requirements.txt
```

### Step 7: Deactivate

```powershell
deactivate
```

### Summary for pyenv-win

**Time taken**: ~10-15 minutes (includes downloading Python)
**Disk space**: ~200 MB per Python version + ~500 MB per environment
**Pros**: Multiple Python versions, Windows-specific solution
**Cons**: Requires PowerShell, not as mature as Linux pyenv

---

## Method 7: uv (Modern & Fast)

**Best for**: Speed-critical workflows, modern projects, CI/CD

### Step 1: Install uv

```powershell
# Check if uv is already installed
uv --version

# If not installed, choose one of the installation methods:

# Option 1: Install using PowerShell (recommended)
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

# Option 2: Install using winget
winget install --id=astral-sh.uv -e

# Option 3: Install with pip
pip install uv

# Restart PowerShell to update PATH

# Verify installation
uv --version

# If uv command not found, add to PATH manually:
# The installer usually adds to: C:\Users\<username>\.cargo\bin
```

### Step 2: Create Virtual Environment

```powershell
# Navigate to uv directory
cd C:\path\to\venv_setup\windows\uv

# Create virtual environment (VERY FAST!)
uv venv demo_env

# Or specify Python version
uv venv --python 3.11 demo_env

# Check structure
dir demo_env
```

### Step 3: Activate Environment

```powershell
# Activate in PowerShell
.\demo_env\Scripts\Activate.ps1

# Verify
where python
where pip
```

### Step 4: Install Packages (Lightning Fast!)

```powershell
# Install from requirements.txt using uv
uv pip install -r ..\..\requirements.txt

# Note: This is 10-100x faster than regular pip!

# Or install packages directly
uv pip install numpy pandas matplotlib seaborn
```

### Step 5: Verify Installation

```powershell
# List packages
uv pip list

# Or use regular pip
pip list

# Test imports
python -c "import numpy, pandas, matplotlib; print('Success!')"
```

### Step 6: Deactivate

```powershell
deactivate
```

### Summary for uv

**Time taken**: ~30 seconds - 1 minute (10-100x faster!)
**Disk space**: ~500 MB (with packages)
**Pros**: Extremely fast, modern, pip-compatible
**Cons**: Relatively new, smaller community

---

## Method 8: Windows Terminal + winget (Windows-Specific)

**Best for**: Windows 11 users who want modern package management

This is the recommended approach for modern Windows users.

### Complete Setup

```powershell
# 1. Install Windows Terminal (if not already installed)
winget install Microsoft.WindowsTerminal

# 2. Install Python
winget install Python.Python.3.11

# 3. Restart terminal

# 4. Verify Python
python --version

# 5. Create virtual environment
python -m venv datasci_demo

# 6. Activate
.\datasci_demo\Scripts\Activate.ps1

# 7. Install packages
pip install -r requirements.txt
```

---

## Comparison of Results

After testing all methods, here's a comparison:

| Method | Setup Time | Install Time | Total Time | Disk Space | Difficulty | Windows Native |
|--------|------------|--------------|------------|------------|------------|----------------|
| **venv** | 10 sec | 3-5 min | ~5 min | 500 MB | Easy |  |
| **virtualenv** | 30 sec | 3-5 min | ~5 min | 500 MB | Easy |  |
| **Conda** | 10 min | 10-15 min | ~25 min | 4 GB | Medium |  |
| **Miniconda** | 5 min | 10-15 min | ~20 min | 1.5 GB | Medium |  |
| **Mamba** | 5 min | 1-3 min | ~8 min | 1.5 GB | Medium |  |
| **pyenv-win** | 10 min | 3-5 min | ~15 min | 700 MB | Medium |  |
| **uv** | 30 sec | 30-60 sec | ~1.5 min | 500 MB | Easy |  |
| **winget+Terminal** | 2 min | 3-5 min | ~7 min | 500 MB | Easy |  |

### Recommendations by Scenario:

1. **Quick Demo in Class**: `uv` or `venv`
2. **Data Science Course**: `Mamba` or `Miniconda`
3. **Windows Native Experience**: `winget + Windows Terminal`
4. **GPU/Deep Learning**: `Mamba` or `Conda`
5. **Multiple Python Versions**: `pyenv-win`
6. **Production Deployment**: `venv` or `uv`
7. **Maximum Speed**: `uv`

---

## Troubleshooting

### Issue 1: "python is not recognized"

```powershell
# Add Python to PATH
# During installation: Check "Add Python to PATH"

# Or manually add to PATH:
# System Properties > Environment Variables > Path
# Add: C:\Users\<username>\AppData\Local\Programs\Python\Python311
# Add: C:\Users\<username>\AppData\Local\Programs\Python\Python311\Scripts

# Or in PowerShell (temporary):
$env:Path += ";C:\Users\$env:USERNAME\AppData\Local\Programs\Python\Python311;C:\Users\$env:USERNAME\AppData\Local\Programs\Python\Python311\Scripts"
```

### Issue 2: PowerShell execution policy error

```powershell
# Check current policy
Get-ExecutionPolicy

# Set to RemoteSigned (allows local scripts)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# Or bypass for current session
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process
```

### Issue 3: "conda is not recognized"

```powershell
# Initialize conda for PowerShell
conda init powershell

# Restart PowerShell

# Or manually add to PATH (see Issue 1)
```

### Issue 4: Long path issues (Windows 260 character limit)

```powershell
# Enable long paths in Windows
# Run as Administrator:
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force

# Or use shorter paths
# C:\venv\demo instead of C:\Users\username\Documents\Projects\...
```

### Issue 5: Permission denied errors

```powershell
# Run PowerShell as Administrator
# Right-click PowerShell > Run as Administrator

# Or adjust permissions on directory
icacls C:\path\to\directory /grant Users:F
```

### Issue 6: SSL certificate errors

```powershell
# Update pip
python -m pip install --upgrade pip

# Install certifi
pip install --upgrade certifi

# Or use --trusted-host flag
pip install --trusted-host pypi.org --trusted-host files.pythonhosted.org package_name
```

### Issue 7: Antivirus blocking installation

```powershell
# Temporarily disable antivirus (Windows Defender)
# Windows Security > Virus & threat protection > Manage settings
# Turn off "Real-time protection" temporarily

# After installation, turn it back on
```

---

## Windows-Specific Tips

### 1. Use Windows Terminal

```powershell
# Install Windows Terminal
winget install Microsoft.WindowsTerminal

# Benefits: tabs, better colors, multiple profiles
```

### 2. Use PowerShell 7+ (recommended)

```powershell
# Install PowerShell 7
winget install Microsoft.PowerShell

# Better than Windows PowerShell 5.1
```

### 3. Enable Long Paths

```powershell
# Run as Administrator
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
```

### 4. Use Short Paths for Virtual Environments

```powershell
# Instead of: C:\Users\username\Documents\Projects\ML\venv
# Use: C:\venv or D:\venv

# Shorter paths avoid Windows 260 character limit
```

---

## Testing Checklist

Use this checklist when demonstrating each method:

- [ ] Environment created successfully
- [ ] Environment activated (prompt changed)
- [ ] Correct Python version (`python --version`)
- [ ] Correct pip location (`where pip`)
- [ ] Packages installed from requirements.txt
- [ ] Package import successful (`import numpy, pandas`)
- [ ] Environment can be deactivated
- [ ] Environment can be reactivated
- [ ] Environment can be deleted/removed

---

## Classroom Demonstration Script

### Quick 5-Minute Demo (venv) - PowerShell

```powershell
# 1. Show current Python
python --version

# 2. Create environment
python -m venv quick_demo

# 3. Activate
.\quick_demo\Scripts\Activate.ps1

# 4. Install one package
pip install numpy

# 5. Test
python -c "import numpy; print(numpy.__version__)"

# 6. Deactivate
deactivate

# 7. Clean up
Remove-Item -Recurse -Force quick_demo
```

### Quick 2-Minute Demo (uv - Fastest)

```powershell
# 1. Create environment
uv venv quick_demo

# 2. Activate
.\quick_demo\Scripts\Activate.ps1

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
