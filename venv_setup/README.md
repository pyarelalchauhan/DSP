# Python Virtual Environment Setup Guide for Data Science

A comprehensive guide to understanding and setting up Python virtual environments for Data Science projects across **Linux**, **macOS**, and **Windows**.

---

## 🚀 Quick Start

**Choose your operating system and jump to the detailed guide:**

- **[Linux Setup Guide →](./linux/README.md)** - Ubuntu, Debian, Fedora, CentOS, Arch
- **[macOS Setup Guide →](./mac/README.md)** - Intel & Apple Silicon (M1/M2/M3)
- **[Windows Setup Guide →](./windows/README.md)** - Windows 10, Windows 11

---

## 📁 Repository Structure

```
venv_setup/
├── README.md                  # This file - overview and concepts
├── linux/                     # Linux-specific setup
│   ├── README.md             # Detailed Linux instructions
│   ├── requirements.txt       # Demo pip packages
│   ├── environment.yml        # Demo conda environment
│   ├── pyproject.toml        # Demo modern Python project
│   └── [venv, conda, mamba, etc.]  # Testing directories
├── mac/                       # macOS-specific setup
│   ├── README.md             # Detailed macOS instructions
│   ├── requirements.txt       # Demo pip packages
│   ├── environment.yml        # Demo conda environment
│   ├── pyproject.toml        # Demo modern Python project
│   └── [venv, conda, mamba, etc.]  # Testing directories
└── windows/                   # Windows-specific setup
    ├── README.md             # Detailed Windows instructions
    ├── requirements.txt       # Demo pip packages
    ├── environment.yml        # Demo conda environment
    ├── pyproject.toml        # Demo modern Python project
    └── [venv, conda, mamba, etc.]  # Testing directories
```

---

## 🎯 For Students: How to Use This Repository

### Step 1: Clone the Repository

```bash
# Clone this repository
git clone <repository-url>
cd venv_setup

# Navigate to your operating system directory
cd linux     # For Linux
# or
cd mac       # For macOS
# or
cd windows   # For Windows
```

### Step 2: Read the OS-Specific README

Each operating system has its own detailed README with:
- Prerequisites and system checks
- Step-by-step instructions for 7+ methods
- Troubleshooting tips
- Classroom demonstration scripts

### Step 3: Follow Along in Class

Your instructor will demonstrate each method. You can:
1. Follow along with the commands
2. Test in the corresponding subdirectories
3. Use the demo files (requirements.txt, environment.yml)

---

## Table of Contents

1. [What are Virtual Environments?](#what-are-virtual-environments)
2. [Why Do We Need Virtual Environments?](#why-do-we-need-virtual-environments)
3. [Overview of Virtual Environment Tools](#overview-of-virtual-environment-tools)
4. [Platform-Specific Guides](#platform-specific-guides)
5. [Setup Guides (Conceptual)](#setup-guides-conceptual)
   - [Conda](#1-conda)
   - [Miniconda](#2-miniconda)
   - [Mamba](#3-mamba)
   - [Python venv](#4-python-venv)
   - [virtualenv](#5-virtualenv)
   - [pyenv](#6-pyenv)
   - [uv](#7-uv-modern-python-package-manager)
6. [Comparison Table](#comparison-table)
7. [Advanced Configuration](#advanced-configuration)
8. [Additional Resources](#additional-resources)

---

## What are Virtual Environments?

A **virtual environment** is an isolated Python environment that allows you to install packages and dependencies specific to a project without affecting your system-wide Python installation or other projects.

### Key Concepts:

- **Isolation**: Each virtual environment has its own Python binary and independent set of installed packages
- **Dependency Management**: Different projects can use different versions of the same package
- **Reproducibility**: Environments can be recreated from requirement files, ensuring consistency across different machines
- **Clean System**: Keeps your system Python installation clean and prevents dependency conflicts

### How They Work:

Virtual environments create a directory structure containing:
- A copy of or symlink to the Python interpreter
- A `site-packages` directory for installing packages
- Scripts to activate/deactivate the environment
- Environment-specific binaries and libraries

---

## Why Do We Need Virtual Environments?

### 1. **Dependency Isolation**
Different projects may require different versions of the same library. For example:
- Project A needs `numpy==1.24.0`
- Project B needs `numpy==1.26.0`

Without virtual environments, these projects would conflict.

### 2. **Reproducibility**
Virtual environments ensure that your code runs consistently across:
- Different team members' machines
- Development, testing, and production environments
- Different time periods (avoiding "it worked on my machine" problems)

### 3. **Clean Development**
- Prevents pollution of system-wide Python installation
- Easier to identify project-specific dependencies
- Safer experimentation with new packages

### 4. **Version Management**
- Test code against multiple Python versions
- Maintain legacy projects with older dependencies
- Prepare for Python version upgrades

### 5. **Data Science Specific Needs**
- Scientific libraries (NumPy, SciPy, Pandas) have complex dependencies
- Deep learning frameworks (TensorFlow, PyTorch) often require specific versions
- Jupyter notebooks benefit from isolated kernel environments
- CUDA and GPU libraries require careful version matching

---

## Overview of Virtual Environment Tools

| Tool | Best For | Package Manager | Python Versions | Data Science Focus |
|------|----------|----------------|-----------------|-------------------|
| **Conda** | Data Science, ML/DL projects | conda | Multiple | High |
| **Miniconda** | Lightweight Conda alternative | conda | Multiple | High |
| **Mamba** | Fast Conda alternative | conda/mamba | Multiple | High |
| **venv** | Simple Python projects | pip | Single (system) | Low |
| **virtualenv** | Enhanced venv | pip | Multiple | Low |
| **pyenv** | Python version management | pip | Multiple | Medium |
| **uv** | Modern, ultra-fast package management | pip-compatible | Multiple | Growing |

---

## Platform-Specific Guides

For detailed, step-by-step instructions tailored to your operating system, please refer to:

### 📘 [Linux Setup Guide](./linux/README.md)

**Covers:**
- Ubuntu, Debian, Fedora, CentOS, Arch Linux
- 7 methods: venv, virtualenv, Conda, Miniconda, Mamba, pyenv, uv
- Prerequisites checks and installation instructions
- Custom cache directory configuration for limited home space
- Troubleshooting for common Linux issues

**Best for**: Server environments, development workstations, cloud instances

---

### 📗 [macOS Setup Guide](./mac/README.md)

**Covers:**
- Intel Macs (x86_64) and Apple Silicon (M1/M2/M3)
- 8 methods: venv, virtualenv, Conda, Miniconda, Mamba, pyenv, uv, Homebrew+pyenv
- Homebrew integration
- Architecture-specific considerations
- Troubleshooting for macOS-specific issues

**Best for**: MacBook users, iOS developers doing ML, design + data science workflows

---

### 📙 [Windows Setup Guide](./windows/README.md)

**Covers:**
- Windows 10, Windows 11 (64-bit)
- 8 methods: venv, virtualenv, Conda, Miniconda, Mamba, pyenv-win, uv, winget+Terminal
- PowerShell and Command Prompt instructions
- Execution policy configuration
- Windows Terminal setup
- Troubleshooting for Windows-specific issues (long paths, permissions, etc.)

**Best for**: Windows desktop users, enterprise environments, beginners

---

## Setup Guides (Conceptual)

The following sections provide conceptual overviews of each tool. For OS-specific installation and usage, **please refer to the platform-specific guides above**.

### 1. Conda

**Conda** is a package manager and environment manager that comes with Anaconda distribution. It's particularly popular in Data Science because it handles both Python packages and non-Python dependencies (like CUDA, MKL libraries).

#### Official Documentation:
- **Conda Docs**: https://docs.conda.io/
- **Getting Started**: https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html
- **Anaconda Distribution**: https://www.anaconda.com/products/distribution

#### Installation:

1. **Download Anaconda:**
   ```bash
   # For Linux
   wget https://repo.anaconda.com/archive/Anaconda3-latest-Linux-x86_64.sh
   bash Anaconda3-latest-Linux-x86_64.sh
   ```

2. **Follow the installer prompts and initialize conda:**
   ```bash
   conda init bash  # or zsh, fish, etc.
   ```

#### Basic Usage:

```bash
# Create a new environment
conda create -n myenv python=3.11

# Create environment with specific packages
conda create -n datasci python=3.11 numpy pandas scikit-learn jupyter

# Activate environment
conda activate myenv

# Install packages
conda install matplotlib seaborn

# List all environments
conda env list

# Export environment
conda env export > environment.yml

# Create environment from file
conda env create -f environment.yml

# Deactivate environment
conda deactivate

# Remove environment
conda env remove -n myenv
```

#### Pros:
- Handles both Python and non-Python dependencies
- Excellent for Data Science (pre-built scientific packages)
- Cross-platform package management
- Built-in support for different Python versions

#### Cons:
- Large installation size (~3GB for Anaconda)
- Can be slow for dependency resolution
- Uses its own package repository (conda-forge)

---

### 2. Miniconda

**Miniconda** is a minimal installer for Conda. It includes only Conda, Python, and a small number of essential packages. You install only what you need.

#### Official Documentation:
- **Miniconda Docs**: https://docs.conda.io/projects/miniconda/
- **Installation Guide**: https://docs.conda.io/projects/miniconda/en/latest/miniconda-install.html

#### Installation:

```bash
# For Linux
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh

# Initialize conda
conda init bash
```

#### Basic Usage:

Same as Conda (Miniconda uses the same `conda` command). The only difference is the initial installation size.

```bash
# Create environment with Data Science stack
conda create -n ds_project python=3.11
conda activate ds_project
conda install numpy pandas matplotlib seaborn scikit-learn jupyter
```

#### Pros:
- Lightweight (~400MB vs 3GB for Anaconda)
- All Conda functionality
- Install only what you need
- Faster installation

#### Cons:
- Need to manually install common packages
- Same slow dependency resolution as Conda

#### When to Use:
- Limited disk space
- Custom Data Science stack
- Server environments
- Want control over installed packages

**Read More**: https://statistics.berkeley.edu/computing/conda

---

### 3. Mamba

**Mamba** is a reimplementation of Conda in C++, providing much faster dependency resolution and package installation. It's fully compatible with Conda environments and packages.

#### Official Documentation:
- **Mamba Docs**: https://mamba.readthedocs.io/
- **GitHub**: https://github.com/mamba-org/mamba
- **Miniforge (includes Mamba)**: https://github.com/conda-forge/miniforge

#### Installation:

**Option 1: Install Mamba in existing Conda/Miniconda:**
```bash
conda install mamba -n base -c conda-forge
```

**Option 2: Install Miniforge (recommended):**
```bash
# For Linux
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh
bash Miniforge3-Linux-x86_64.sh
```

#### Basic Usage:

Replace `conda` with `mamba` in most commands:

```bash
# Create environment (much faster!)
mamba create -n myenv python=3.11

# Install packages
mamba install numpy pandas scikit-learn tensorflow pytorch

# Create Data Science environment quickly
mamba create -n ml_project python=3.11 numpy pandas matplotlib seaborn \
    scikit-learn jupyter jupyterlab plotly scipy

# Activate/deactivate (still use conda)
conda activate myenv
conda deactivate

# Update packages (faster)
mamba update --all

# Search for packages
mamba search tensorflow
```

#### Pros:
- **10-100x faster** than Conda for dependency resolution
- Fully compatible with Conda packages and environments
- Parallel downloads
- Better dependency conflict resolution
- Uses conda-forge by default

#### Cons:
- Slightly more complex installation
- Smaller community than Conda (but growing)

#### When to Use:
- Large Data Science projects with many dependencies
- Working with deep learning frameworks
- Tired of waiting for Conda to resolve dependencies
- Need frequent environment updates

**Read More**: https://dasroot.net/posts/2024/12/anaconda-vs-miniconda-vs-mamba/

---

### 4. Python venv

**venv** is Python's built-in module for creating virtual environments. Available in Python 3.3+, it's the standard lightweight solution for simple projects.

#### Official Documentation:
- **venv Docs**: https://docs.python.org/3/library/venv.html
- **Tutorial**: https://docs.python.org/3/tutorial/venv.html

#### Installation:

No installation needed! It's built into Python 3.3+.

```bash
# Verify Python version
python3 --version
```

#### Basic Usage:

```bash
# Create virtual environment
python3 -m venv myenv

# Activate environment
source myenv/bin/activate  # Linux/Mac
# myenv\Scripts\activate  # Windows

# Install packages
pip install numpy pandas matplotlib

# Save dependencies
pip freeze > requirements.txt

# Install from requirements
pip install -r requirements.txt

# Deactivate
deactivate

# Remove environment (just delete the folder)
rm -rf myenv
```

#### Data Science Example:

```bash
# Create environment for ML project
python3 -m venv ml_env
source ml_env/bin/activate

# Install Data Science stack
pip install numpy pandas matplotlib seaborn scikit-learn jupyter

# Install deep learning
pip install torch torchvision tensorflow

# Save environment
pip freeze > requirements.txt
```

#### Pros:
- Built into Python (no extra installation)
- Lightweight and fast
- Simple and straightforward
- Good for pure Python projects

#### Cons:
- Only manages Python packages (no system dependencies)
- Cannot switch Python versions within environment
- Slower package installation than newer tools
- No built-in support for non-Python dependencies (CUDA, etc.)

#### When to Use:
- Simple Python projects
- Quick prototyping
- Projects without complex system dependencies
- Learning Python/Data Science basics

---

### 5. virtualenv

**virtualenv** is a more feature-rich alternative to venv. It supports older Python versions and provides additional functionality.

#### Official Documentation:
- **virtualenv Docs**: https://virtualenv.pypa.io/
- **User Guide**: https://virtualenv.pypa.io/en/latest/user_guide.html

#### Installation:

```bash
pip install virtualenv
```

#### Basic Usage:

```bash
# Create environment
virtualenv myenv

# Create with specific Python version
virtualenv -p python3.11 myenv

# Activate
source myenv/bin/activate

# Install packages
pip install numpy pandas scikit-learn

# Deactivate
deactivate
```

#### Advanced Features:

```bash
# Create environment with system site packages access
virtualenv --system-site-packages myenv

# Create without pip
virtualenv --no-pip myenv

# Use different Python interpreter
virtualenv -p /usr/bin/python3.9 myenv
```

#### Pros:
- More features than venv
- Works with Python 2.7+ and Python 3.3+
- Faster environment creation
- More configuration options

#### Cons:
- Requires separate installation
- Overkill for simple projects
- Still pip-based (slower than modern alternatives)

#### When to Use:
- Need Python 2.7 support
- Require advanced virtualenv features
- Working with legacy codebases

---

### 6. pyenv

**pyenv** is a Python version management tool. It lets you easily switch between multiple versions of Python and works well with virtual environments.

#### Official Documentation:
- **pyenv GitHub**: https://github.com/pyenv/pyenv
- **pyenv-virtualenv**: https://github.com/pyenv/pyenv-virtualenv

#### Installation:

```bash
# Install pyenv (Linux)
curl https://pyenv.run | bash

# Add to shell configuration (~/.bashrc or ~/.zshrc)
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc

# Reload shell
source ~/.bashrc

# Install pyenv-virtualenv plugin
git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc
source ~/.bashrc
```

#### Basic Usage:

```bash
# List available Python versions
pyenv install --list

# Install Python versions
pyenv install 3.11.0
pyenv install 3.10.0
pyenv install 3.9.0

# List installed versions
pyenv versions

# Set global Python version
pyenv global 3.11.0

# Set local (project-specific) version
pyenv local 3.10.0

# Create virtual environment
pyenv virtualenv 3.11.0 my-project-env

# Activate environment
pyenv activate my-project-env

# Deactivate
pyenv deactivate

# Delete environment
pyenv virtualenv-delete my-project-env
```

#### Data Science Workflow:

```bash
# Install Python 3.11
pyenv install 3.11.0

# Create Data Science environment
pyenv virtualenv 3.11.0 datasci-env

# Activate and install packages
pyenv activate datasci-env
pip install numpy pandas matplotlib seaborn scikit-learn jupyter

# Set as local environment for project
cd /path/to/project
pyenv local datasci-env
# Now this environment activates automatically when you cd into this directory
```

#### Pros:
- Manage multiple Python versions easily
- Per-project Python version configuration
- Automatic environment activation with pyenv-virtualenv
- Lightweight

#### Cons:
- More complex setup
- Only manages Python versions (still uses pip for packages)
- Not specifically designed for Data Science

#### When to Use:
- Need multiple Python versions
- Testing code across Python versions
- Per-project Python version requirements
- Combine with pip or conda for package management

---

### 7. uv (Modern Python Package Manager)

**uv** is a blazingly fast Python package installer and resolver, written in Rust. It's designed as a drop-in replacement for pip, pip-tools, and virtualenv, with 10-100x faster performance.

#### Official Documentation:
- **uv Docs**: https://docs.astral.sh/uv/
- **GitHub**: https://github.com/astral-sh/uv
- **Getting Started**: https://docs.astral.sh/uv/getting-started/

#### Installation:

```bash
# Install uv (Linux/Mac)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Or with pip
pip install uv

# Or with pipx
pipx install uv
```

#### Basic Usage:

```bash
# Create virtual environment
uv venv myenv

# Create with specific Python version
uv venv --python 3.11 myenv

# Activate environment
source myenv/bin/activate

# Install packages (extremely fast!)
uv pip install numpy pandas matplotlib

# Install from requirements.txt
uv pip install -r requirements.txt

# Install multiple packages
uv pip install numpy pandas scikit-learn tensorflow torch

# Sync dependencies (like pip-tools)
uv pip sync requirements.txt

# Compile requirements
uv pip compile pyproject.toml -o requirements.txt

# Deactivate
deactivate
```

#### Advanced Features:

```bash
# Create environment with specific Python
uv venv --python 3.11.0 ml-env

# Install with constraints
uv pip install numpy --constraint constraints.txt

# Install from pyproject.toml
uv pip install -e .

# Generate locked requirements
uv pip compile requirements.in -o requirements.txt

# Upgrade all packages
uv pip install --upgrade -r requirements.txt
```

#### Data Science Workflow:

```bash
# Create Data Science environment
uv venv datasci
source datasci/bin/activate

# Install Data Science stack (very fast!)
uv pip install numpy pandas matplotlib seaborn scipy scikit-learn jupyter jupyterlab

# Install deep learning frameworks
uv pip install torch torchvision tensorflow

# Save exact versions
uv pip freeze > requirements.txt

# Reproducible installation
uv pip install -r requirements.txt
```

#### Pros:
- **10-100x faster** than pip
- Drop-in replacement for pip
- Fast dependency resolution
- Written in Rust (memory safe, concurrent)
- Modern dependency management
- Growing ecosystem

#### Cons:
- Relatively new (still maturing)
- Smaller community than pip/conda
- Some edge cases may not be supported yet
- Not yet widely adopted in Data Science

#### When to Use:
- Speed is critical (large projects, CI/CD)
- Modern Python projects
- Tired of slow pip installations
- Want reproducible builds
- Clean dependency management

**Read More**:
- https://medium.com/@rickboelen98/python-packaging-uvs-rise-against-pip-2c614960efc5
- https://docs.astral.sh/uv/

---

## Comparison Table

| Feature | Conda/Mamba | venv | virtualenv | pyenv | uv |
|---------|-------------|------|------------|-------|-----|
| **Installation Size** | Large (Conda) / Medium (Mamba) | None | Small | Small | Small |
| **Speed** | Slow (Conda) / Fast (Mamba) | Medium | Medium | Medium | Very Fast |
| **Python Versions** | Multiple | Single | Multiple | Multiple | Multiple |
| **Non-Python Deps** | Yes | No | No | No | No |
| **Data Science Focus** | High | Low | Low | Medium | Medium |
| **Package Resolution** | Advanced | Basic | Basic | Basic | Advanced |
| **Learning Curve** | Medium | Easy | Easy | Medium | Easy |
| **Binary Packages** | Yes | Via pip | Via pip | Via pip | Via pip |
| **GPU/CUDA Support** | Excellent | Manual | Manual | Manual | Manual |

### Recommendations by Use Case:

1. **Data Science / Machine Learning Projects**:
   - **Best**: Mamba (speed + functionality)
   - **Alternative**: Miniconda (if Mamba unavailable)

2. **Deep Learning with GPU**:
   - **Best**: Mamba or Conda (handles CUDA dependencies)
   - **Alternative**: venv/uv with manual CUDA setup

3. **Simple Python Projects**:
   - **Best**: venv (built-in, simple)
   - **Alternative**: uv (if speed matters)

4. **Multiple Python Versions**:
   - **Best**: pyenv (designed for this)
   - **Alternative**: Conda/Mamba

5. **Speed-Critical Workflows**:
   - **Best**: uv (package installation) + Mamba (environment creation)

6. **Production/Deployment**:
   - **Best**: venv or uv (lightweight, reproducible)
   - **Alternative**: Conda if non-Python dependencies required

7. **Teaching/Learning**:
   - **Best**: venv (simple, built-in)
   - **Alternative**: Miniconda (for Data Science courses)

---

## Advanced Configuration

### Setting Up Cache Directory in Project Folder

When working on shared systems or when your home directory has limited space, you may need to configure package managers to store cache and packages in a different location (e.g., your project folder or a mounted drive).

#### 1. Conda/Mamba Cache Configuration

Conda stores packages in `~/.conda/pkgs` by default. To change this:

**Method 1: Using `.condarc` file**

Create or edit `~/.condarc`:

```yaml
pkgs_dirs:
  - /NFSDISK2/pyare/TAship/DSP315/.conda/pkgs
  - /NFSDISK2/pyare/TAship/DSP315/.conda/cache

envs_dirs:
  - /NFSDISK2/pyare/TAship/DSP315/.conda/envs
  - ~/.conda/envs
```

**Method 2: Using environment variables**

Add to your `~/.bashrc` or `~/.zshrc`:

```bash
export CONDA_PKGS_DIRS="/NFSDISK2/pyare/TAship/DSP315/.conda/pkgs"
export CONDA_ENVS_DIRS="/NFSDISK2/pyare/TAship/DSP315/.conda/envs"
```

**Method 3: Per-project configuration**

Create a local `.condarc` in your project directory:

```bash
cd /NFSDISK2/pyare/TAship/DSP315
cat > .condarc << EOF
pkgs_dirs:
  - ./.conda/pkgs

envs_dirs:
  - ./.conda/envs
EOF

# Use with --file flag
conda config --file ./.condarc --add pkgs_dirs ./.conda/pkgs
```

**Verify configuration:**

```bash
conda config --show pkgs_dirs
conda config --show envs_dirs
```

#### 2. pip Cache Configuration

pip caches downloaded packages in `~/.cache/pip` by default.

**Method 1: Environment variable**

```bash
# Add to ~/.bashrc
export PIP_CACHE_DIR="/NFSDISK2/pyare/TAship/DSP315/.pip_cache"
```

**Method 2: pip configuration file**

Create or edit `~/.config/pip/pip.conf`:

```ini
[global]
cache-dir = /NFSDISK2/pyare/TAship/DSP315/.pip_cache
```

**Method 3: Command-line flag**

```bash
pip install --cache-dir=/NFSDISK2/pyare/TAship/DSP315/.pip_cache numpy pandas
```

**Verify cache location:**

```bash
pip cache dir
```

#### 3. uv Cache Configuration

uv uses `~/.cache/uv` by default.

**Set cache directory:**

```bash
# Environment variable
export UV_CACHE_DIR="/NFSDISK2/pyare/TAship/DSP315/.uv_cache"

# Or use command-line flag
uv pip install --cache-dir /NFSDISK2/pyare/TAship/DSP315/.uv_cache numpy
```

**Add to `~/.bashrc`:**

```bash
export UV_CACHE_DIR="/NFSDISK2/pyare/TAship/DSP315/.uv_cache"
```

#### 4. Complete Setup Script for Limited Home Space

Create a setup script `setup_custom_cache.sh`:

```bash
#!/bin/bash

# Project directory
PROJECT_DIR="/NFSDISK2/pyare/TAship/DSP315"

# Create cache directories
mkdir -p ${PROJECT_DIR}/.conda/pkgs
mkdir -p ${PROJECT_DIR}/.conda/envs
mkdir -p ${PROJECT_DIR}/.pip_cache
mkdir -p ${PROJECT_DIR}/.uv_cache

# Configure Conda
cat > ~/.condarc << EOF
pkgs_dirs:
  - ${PROJECT_DIR}/.conda/pkgs

envs_dirs:
  - ${PROJECT_DIR}/.conda/envs
  - ~/.conda/envs
EOF

# Configure pip
mkdir -p ~/.config/pip
cat > ~/.config/pip/pip.conf << EOF
[global]
cache-dir = ${PROJECT_DIR}/.pip_cache
EOF

# Export environment variables (add these to ~/.bashrc for persistence)
export CONDA_PKGS_DIRS="${PROJECT_DIR}/.conda/pkgs"
export CONDA_ENVS_DIRS="${PROJECT_DIR}/.conda/envs"
export PIP_CACHE_DIR="${PROJECT_DIR}/.pip_cache"
export UV_CACHE_DIR="${PROJECT_DIR}/.uv_cache"

echo "Cache directories configured in ${PROJECT_DIR}"
echo "Add the following lines to your ~/.bashrc for persistence:"
echo ""
echo "export CONDA_PKGS_DIRS=\"${PROJECT_DIR}/.conda/pkgs\""
echo "export CONDA_ENVS_DIRS=\"${PROJECT_DIR}/.conda/envs\""
echo "export PIP_CACHE_DIR=\"${PROJECT_DIR}/.pip_cache\""
echo "export UV_CACHE_DIR=\"${PROJECT_DIR}/.uv_cache\""
```

**Make it executable and run:**

```bash
chmod +x setup_custom_cache.sh
./setup_custom_cache.sh
```

#### 5. Add to `.bashrc` for Permanent Configuration

Add to `~/.bashrc`:

```bash
# Custom cache directories for DSP315 project
export PROJECT_DIR="/NFSDISK2/pyare/TAship/DSP315"
export CONDA_PKGS_DIRS="${PROJECT_DIR}/.conda/pkgs"
export CONDA_ENVS_DIRS="${PROJECT_DIR}/.conda/envs"
export PIP_CACHE_DIR="${PROJECT_DIR}/.pip_cache"
export UV_CACHE_DIR="${PROJECT_DIR}/.uv_cache"
```

Then reload:

```bash
source ~/.bashrc
```

#### 6. Verify Configuration

```bash
# Check Conda
conda config --show pkgs_dirs
conda config --show envs_dirs

# Check pip
pip cache dir

# Check environment variables
echo $CONDA_PKGS_DIRS
echo $PIP_CACHE_DIR
echo $UV_CACHE_DIR
```

#### 7. Clean Up Old Caches (Optional)

Before switching, you may want to clean old caches:

```bash
# Clean Conda cache
conda clean --all

# Clean pip cache
pip cache purge

# Clean uv cache
uv cache clean
```

---

## Additional Resources

### Official Documentation

- **Conda**: https://docs.conda.io/
- **Miniconda**: https://docs.conda.io/projects/miniconda/
- **Mamba**: https://mamba.readthedocs.io/
- **Python venv**: https://docs.python.org/3/library/venv.html
- **virtualenv**: https://virtualenv.pypa.io/
- **pyenv**: https://github.com/pyenv/pyenv
- **uv**: https://docs.astral.sh/uv/

### Comparisons and Guides

- **Anaconda vs Miniconda vs Mamba**: https://dasroot.net/posts/2024/12/anaconda-vs-miniconda-vs-mamba/
- **Berkeley Conda Guide**: https://statistics.berkeley.edu/computing/conda
- **uv vs pip**: https://medium.com/@rickboelen98/python-packaging-uvs-rise-against-pip-2c614960efc5

### Package Repositories

- **PyPI (Python Package Index)**: https://pypi.org/
- **Conda-forge**: https://conda-forge.org/
- **Anaconda Repository**: https://repo.anaconda.com/

### Best Practices

- **Python Packaging Guide**: https://packaging.python.org/
- **Conda Best Practices**: https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html
- **Virtual Environments Tutorial**: https://realpython.com/python-virtual-environments-a-primer/

### Community Resources

- **r/Python**: https://reddit.com/r/Python
- **r/datascience**: https://reddit.com/r/datascience
- **Stack Overflow - Python**: https://stackoverflow.com/questions/tagged/python

---

## Quick Reference Commands

### Conda/Mamba

```bash
# Create environment
conda create -n myenv python=3.11
mamba create -n myenv python=3.11

# Activate/Deactivate
conda activate myenv
conda deactivate

# Install packages
conda install numpy pandas
mamba install scikit-learn tensorflow

# Export/Import
conda env export > environment.yml
conda env create -f environment.yml
```

### venv

```bash
# Create
python3 -m venv myenv

# Activate
source myenv/bin/activate  # Linux/Mac

# Install
pip install numpy pandas

# Export
pip freeze > requirements.txt

# Install from file
pip install -r requirements.txt
```

### uv

```bash
# Create
uv venv myenv

# Activate
source myenv/bin/activate

# Install
uv pip install numpy pandas

# From requirements
uv pip install -r requirements.txt
```

### pyenv

```bash
# Install Python
pyenv install 3.11.0

# Create virtualenv
pyenv virtualenv 3.11.0 myenv

# Activate
pyenv activate myenv

# Set local
pyenv local myenv
```

---

## Troubleshooting

### Common Issues

1. **"command not found: conda"**
   - Solution: Run `conda init bash` and restart terminal

2. **"permission denied" when installing**
   - Solution: Don't use `sudo` with virtual environments

3. **Environment not activating**
   - Solution: Check shell configuration, ensure scripts are sourced

4. **Slow conda dependency resolution**
   - Solution: Switch to Mamba

5. **Disk space issues**
   - Solution: Use custom cache directories (see Advanced Configuration)

6. **Package conflicts**
   - Solution: Create fresh environment with specific versions

---

**Last Updated**: October 2025
**Maintained for**: DSP315 - Data Science Course
