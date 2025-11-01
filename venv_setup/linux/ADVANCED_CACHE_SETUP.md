# Advanced Cache Configuration - Linux

## Storing Cache in Project Directory Instead of Home

This guide shows how to configure all virtual environment tools to store their cache and packages in your project directory instead of your home directory. This is useful when:

- Home directory has limited space (common on shared servers)
- Project is on a larger disk partition
- You want to keep all project data together
- You need to clean up easily by removing one directory

---

## Table of Contents

1. [Why Change Cache Location?](#why-change-cache-location)
2. [Disk Space Analysis](#disk-space-analysis)
3. [Conda/Mamba Cache Configuration](#condamamba-cache-configuration)
4. [Pip Cache Configuration](#pip-cache-configuration)
5. [uv Cache Configuration](#uv-cache-configuration)
6. [pyenv Cache Configuration](#pyenv-cache-configuration)
7. [Complete Setup Script](#complete-setup-script)
8. [Verification](#verification)
9. [Cleanup](#cleanup)

---

## Why Change Cache Location?

### Default Cache Locations

By default, package managers store cache in your home directory:

```bash
# Conda/Mamba
~/.conda/pkgs/          # Downloaded packages (~5-10 GB)
~/.conda/envs/          # Environments

# Pip
~/.cache/pip/           # Downloaded packages (~1-5 GB)

# uv
~/.cache/uv/            # Downloaded packages

# pyenv
~/.pyenv/versions/      # Python versions (~200 MB each)
```

### Problems with Default Locations

1. **Limited Home Directory Space**: Many servers have small home directory quotas
2. **Difficult to Clean**: Cache scattered across different locations
3. **Not Portable**: Can't move project without reconfiguring
4. **Hidden from User**: Hard to track disk usage

### Benefits of Project-Local Cache

1. **Better Disk Usage**: Use larger project partitions
2. **Easy Cleanup**: Delete one folder to clean all cache
3. **Portable**: Move project folder with all dependencies
4. **Visible**: Clear what's using disk space

---

## Disk Space Analysis

### Check Current Disk Usage

```bash
# Check home directory usage
du -sh ~
df -h ~

# Check project directory usage
du -sh /NFSDISK2/pyare/TAship/DSP315/
df -h /NFSDISK2/pyare/TAship/DSP315/

# Check conda cache size (if installed)
du -sh ~/.conda/pkgs/ 2>/dev/null || echo "No conda cache"

# Check pip cache size
du -sh ~/.cache/pip/ 2>/dev/null || echo "No pip cache"

# Total cache usage
du -sh ~/.conda ~/.cache/pip ~/.cache/uv 2>/dev/null
```

### Identify Disk Partitions

```bash
# Show all disk partitions and available space
df -h

# Find which partition has more space
df -h ~ /NFSDISK2/pyare/TAship/DSP315/
```

---

## Conda/Mamba Cache Configuration

### Method 1: Using `.condarc` File (Recommended)

Create a conda configuration file in your project directory:

```bash
# Navigate to project root
cd /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux

# Create cache directories
mkdir -p .cache/conda/pkgs
mkdir -p .cache/conda/envs

# Create local .condarc file
cat > .condarc << 'EOF'
# Conda/Mamba configuration for project-local cache

# Package cache directory
pkgs_dirs:
  - /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/conda/pkgs

# Environment directory
envs_dirs:
  - /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/conda/envs
  - ~/.conda/envs  # Fallback to home if needed

# Channel priority
channel_priority: strict

# Show channel URLs
show_channel_urls: true
EOF

# Use this config with conda/mamba commands
conda config --file .condarc --show pkgs_dirs
mamba config --file .condarc --show pkgs_dirs
```

### Method 2: Global Configuration (Affects All Projects)

```bash
# Edit user's .condarc
cat > ~/.condarc << 'EOF'
pkgs_dirs:
  - /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/conda/pkgs
  - ~/.conda/pkgs  # Fallback

envs_dirs:
  - /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/conda/envs
  - ~/.conda/envs  # Fallback
EOF

# Verify
conda config --show pkgs_dirs
conda config --show envs_dirs
```

### Method 3: Environment Variables (Session-Specific)

```bash
# Set for current session
export CONDA_PKGS_DIRS="/NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/conda/pkgs"
export CONDA_ENVS_DIRS="/NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/conda/envs"

# Add to project-specific shell script
cat > conda_env_vars.sh << 'EOF'
#!/bin/bash
# Project-specific conda cache configuration
export CONDA_PKGS_DIRS="/NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/conda/pkgs"
export CONDA_ENVS_DIRS="/NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/conda/envs"
EOF

# Source before using conda/mamba
source conda_env_vars.sh
```

### Using Project-Local Cache

```bash
# Method 1: Use --file flag with local .condarc
conda env create -f environment.yml --file .condarc

# Or set environment variable before creating
export CONDARC="/NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.condarc"
mamba env create -f environment.yml
```

---

## Pip Cache Configuration

### Method 1: Environment Variable

```bash
# Create cache directory
mkdir -p /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/pip

# Set for current session
export PIP_CACHE_DIR="/NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/pip"

# Test
pip cache dir

# Use pip as normal
pip install numpy pandas
```

### Method 2: pip Configuration File

```bash
# Create pip config directory
mkdir -p /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.config/pip

# Create pip.conf
cat > /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.config/pip/pip.conf << 'EOF'
[global]
cache-dir = /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/pip

[install]
# Use cache
no-cache-dir = false
EOF

# Set environment variable to use this config
export PIP_CONFIG_FILE="/NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.config/pip/pip.conf"

# Verify
pip config list
```

### Method 3: Command-Line Flag

```bash
# Use --cache-dir flag with every pip command
pip install --cache-dir=/NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/pip numpy pandas

# Install from requirements
pip install --cache-dir=/NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/pip -r requirements.txt
```

---

## uv Cache Configuration

### Environment Variable Method

```bash
# Create cache directory
mkdir -p /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/uv

# Set cache directory
export UV_CACHE_DIR="/NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/uv"

# Verify
echo $UV_CACHE_DIR

# Use uv as normal
uv pip install numpy pandas
```

### Command-Line Flag Method

```bash
# Use --cache-dir flag
uv pip install --cache-dir /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.cache/uv numpy pandas
```

---

## pyenv Cache Configuration

### Change PYENV_ROOT Location

```bash
# Create pyenv directory in project
mkdir -p /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.pyenv

# Set PYENV_ROOT
export PYENV_ROOT="/NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"

# Install Python versions (will be stored in project .pyenv)
pyenv install 3.11.7

# Create virtualenv
pyenv virtualenv 3.11.7 project_env
```

**Note**: Changing `PYENV_ROOT` affects all pyenv operations. Consider using system pyenv with venv instead for project-specific environments.

---

## Complete Setup Script

Create a setup script that configures all cache directories:

```bash
#!/bin/bash
# complete_cache_setup.sh
# Configure all package manager caches to use project directory

PROJECT_DIR="/NFSDISK2/pyare/TAship/DSP315/venv_setup/linux"

echo "Setting up project-local cache directories..."

# Create all cache directories
mkdir -p ${PROJECT_DIR}/.cache/conda/pkgs
mkdir -p ${PROJECT_DIR}/.cache/conda/envs
mkdir -p ${PROJECT_DIR}/.cache/pip
mkdir -p ${PROJECT_DIR}/.cache/uv

echo "Created cache directories:"
ls -la ${PROJECT_DIR}/.cache/

# Create conda configuration
cat > ${PROJECT_DIR}/.condarc << EOF
pkgs_dirs:
  - ${PROJECT_DIR}/.cache/conda/pkgs

envs_dirs:
  - ${PROJECT_DIR}/.cache/conda/envs
  - ~/.conda/envs
EOF

echo "Created .condarc"

# Create pip configuration
mkdir -p ${PROJECT_DIR}/.config/pip
cat > ${PROJECT_DIR}/.config/pip/pip.conf << EOF
[global]
cache-dir = ${PROJECT_DIR}/.cache/pip
EOF

echo "Created pip.conf"

# Create environment variables script
cat > ${PROJECT_DIR}/set_cache_env.sh << 'EOF'
#!/bin/bash
# Source this script to set cache environment variables

PROJECT_DIR="/NFSDISK2/pyare/TAship/DSP315/venv_setup/linux"

# Conda/Mamba
export CONDARC="${PROJECT_DIR}/.condarc"
export CONDA_PKGS_DIRS="${PROJECT_DIR}/.cache/conda/pkgs"
export CONDA_ENVS_DIRS="${PROJECT_DIR}/.cache/conda/envs"

# Pip
export PIP_CACHE_DIR="${PROJECT_DIR}/.cache/pip"
export PIP_CONFIG_FILE="${PROJECT_DIR}/.config/pip/pip.conf"

# uv
export UV_CACHE_DIR="${PROJECT_DIR}/.cache/uv"

echo "Cache environment variables set:"
echo "  CONDA_PKGS_DIRS: $CONDA_PKGS_DIRS"
echo "  PIP_CACHE_DIR: $PIP_CACHE_DIR"
echo "  UV_CACHE_DIR: $UV_CACHE_DIR"
EOF

chmod +x ${PROJECT_DIR}/set_cache_env.sh

echo ""
echo "Setup complete! To use project-local cache:"
echo ""
echo "  1. Source the environment variables:"
echo "     source ${PROJECT_DIR}/set_cache_env.sh"
echo ""
echo "  2. Or add to your ~/.bashrc or ~/.zshrc:"
echo "     echo 'source ${PROJECT_DIR}/set_cache_env.sh' >> ~/.zshrc"
echo ""
echo "  3. Then use conda/mamba/pip/uv as normal"
echo ""
```

### Using the Setup Script

```bash
# Navigate to project directory
cd /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux

# Create the setup script
cat > complete_cache_setup.sh << 'EOF'
[paste the script above]
EOF

# Make executable
chmod +x complete_cache_setup.sh

# Run setup
./complete_cache_setup.sh

# Source environment variables
source set_cache_env.sh

# Now all package managers will use project cache
mamba env create -f environment.yml
```

---

## Verification

### Verify Cache Locations

```bash
# Source cache environment
source /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/set_cache_env.sh

# Check conda
conda config --show pkgs_dirs
conda config --show envs_dirs

# Check pip
pip cache dir
echo $PIP_CACHE_DIR

# Check uv
echo $UV_CACHE_DIR

# List cache contents
ls -lh .cache/conda/pkgs/
ls -lh .cache/pip/
ls -lh .cache/uv/
```

### Test Installation

```bash
# Create test environment to verify cache is used
mamba create -n cache_test python=3.11 numpy

# Activate environment
conda activate cache_test

# Install with pip (should use project cache)
pip install pandas

# Check cache directories have content
du -sh .cache/conda/pkgs/
du -sh .cache/pip/

# Clean up test
conda deactivate
conda env remove -n cache_test
```

---

## Cleanup

### Clean All Caches

```bash
# Navigate to project
cd /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux

# Check current cache size
du -sh .cache/

# Clean conda cache
conda clean --all --yes

# Clean pip cache
pip cache purge

# Or manually remove cache directories
rm -rf .cache/conda/pkgs/*
rm -rf .cache/pip/*
rm -rf .cache/uv/*

# Verify cleanup
du -sh .cache/
```

### Remove Specific Environment

```bash
# Remove specific conda environment
conda env remove -n demo_env

# Remove cache for that environment
# (conda clean will remove unused packages)
conda clean --packages --yes
```

---

## Best Practices

### 1. Use .gitignore

Add cache directories to `.gitignore`:

```bash
cat >> .gitignore << 'EOF'
# Virtual environment caches
.cache/
.pyenv/
.config/pip/
.condarc
set_cache_env.sh
EOF
```

### 2. Document in Project README

Add to your project README:

```markdown
## Setup

This project uses local cache directories to avoid filling home directory.

1. Run the cache setup script:
   ```bash
   ./complete_cache_setup.sh
   ```

2. Source environment variables:
   ```bash
   source set_cache_env.sh
   ```

3. Create environment:
   ```bash
   mamba env create -f environment.yml
   ```
```

### 3. Periodic Cleanup

```bash
# Add to weekly cleanup script
#!/bin/bash
# weekly_cleanup.sh

cd /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux

# Show current usage
echo "Current cache usage:"
du -sh .cache/*

# Clean old cached packages
conda clean --all --yes
pip cache purge

# Show new usage
echo "After cleanup:"
du -sh .cache/*
```

### 4. Backup Configuration

```bash
# Backup your cache configuration
tar -czf cache_config_backup.tar.gz .condarc .config/ set_cache_env.sh

# Restore if needed
tar -xzf cache_config_backup.tar.gz
```

---

## Troubleshooting

### Issue 1: Cache still going to home directory

```bash
# Verify environment variables are set
env | grep -E 'CONDA|PIP|UV'

# Make sure you sourced the script
source set_cache_env.sh

# Check conda config
conda config --show pkgs_dirs
```

### Issue 2: Permission denied

```bash
# Check directory permissions
ls -la .cache/

# Fix permissions
chmod -R u+rwX .cache/
```

### Issue 3: Disk space still full

```bash
# Check if old cache still exists
du -sh ~/.conda/pkgs/
du -sh ~/.cache/pip/

# Clean old caches
conda clean --all --yes
rm -rf ~/.cache/pip/*

# Move old environments if needed
mv ~/.conda/envs/* .cache/conda/envs/
```

### Issue 4: Environment variables not persisting

```bash
# Add to shell RC file permanently
echo 'source /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux/set_cache_env.sh' >> ~/.zshrc

# Reload
source ~/.zshrc
```

---

## Example Workflow

### Complete Example: Setting Up Project with Local Cache

```bash
# 1. Navigate to project
cd /NFSDISK2/pyare/TAship/DSP315/venv_setup/linux

# 2. Run cache setup (one time)
./complete_cache_setup.sh

# 3. Source environment variables (each session)
source set_cache_env.sh

# 4. Create environment using local cache
mamba env create -f environment.yml

# 5. Activate environment
conda activate demo_env

# 6. Install additional packages (uses local cache)
pip install scikit-learn

# 7. Verify cache location
pip cache dir
conda config --show pkgs_dirs

# 8. Check disk usage
du -sh .cache/

# 9. When done, deactivate
conda deactivate

# 10. Periodic cleanup
conda clean --all --yes
pip cache purge
```

---

## Additional Resources

- **Conda Configuration**: https://conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html
- **Pip Configuration**: https://pip.pypa.io/en/stable/topics/configuration/
- **uv Documentation**: https://docs.astral.sh/uv/
- **Disk Space Management**: `man du`, `man df`

---

**Last Updated**: October 31, 2025
**For Course**: DSP315 - Data Science
**Environment**: Linux (Ubuntu, Debian, Fedora, etc.)
