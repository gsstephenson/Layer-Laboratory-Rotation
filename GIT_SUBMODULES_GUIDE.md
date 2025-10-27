# Git Submodules Setup and Management Guide

This document explains how the Git submodule structure was set up for this repository and how to manage submodules in the future.

## Repository Structure

This repository (`Layer-Laboratory-Rotation`) contains submodules that are independent Git repositories:

- **Main Repository**: `https://github.com/gsstephenson/Layer-Laboratory-Rotation`
- **Submodule**: `Alpha_genome_quickstart_notebook` → `https://github.com/gsstephenson/alphagenome-quickstart`

## Why Use Submodules?

Submodules allow you to:
- Keep separate repositories with independent version control
- Work on projects that should have their own commit history
- Share code between multiple parent repositories
- Version control each component independently

---

## Initial Setup Process (What Was Done)

### Step 1: Initialize Main Repository
```bash
cd /mnt/work_1/gest9386/CU_Boulder/rotations/LAYER
git init
git branch -M main
```

### Step 2: Create Security .gitignore Files
Created `.gitignore` in main repository to exclude sensitive files:
```
# Python
__pycache__/
*.py[cod]
*.so
.Python
env/
venv/
.env

# Jupyter
.ipynb_checkpoints
*.ipynb_checkpoints/

# IDE
.vscode/
.idea/
```

Created `.gitignore` in submodule directory to protect API keys:
```
.env
__pycache__/
*.pyc
```

### Step 3: Initialize Submodule Repository
```bash
cd Alpha_genome_quickstart_notebook
git init
git branch -M main
```

### Step 4: Commit Submodule Files
```bash
git add .
git commit -m "Initial commit: AlphaGenome quickstart notebook with documentation"
```

### Step 5: Authenticate GitHub CLI
```bash
gh auth login
# Follow prompts: GitHub.com → SSH → Upload SSH key → Browser authentication
```

### Step 6: Create Remote Repositories
```bash
# Create and push submodule repository
cd Alpha_genome_quickstart_notebook
gh repo create alphagenome-quickstart --public \
  --description "AlphaGenome Quick Start: Comprehensive tutorial notebook" \
  --source=. --remote=origin --push

# Create main repository (without pushing yet)
cd ..
gh repo create Layer-Laboratory-Rotation --public \
  --description "Layer Lab Rotation Project at CU Boulder" \
  --source=. --remote=origin
```

### Step 7: Configure Proper Submodule Relationship
```bash
# Temporarily rename the submodule directory
mv Alpha_genome_quickstart_notebook Alpha_genome_quickstart_notebook_backup

# Add as proper Git submodule
git submodule add git@github.com:gsstephenson/alphagenome-quickstart.git Alpha_genome_quickstart_notebook

# Remove backup
rm -rf Alpha_genome_quickstart_notebook_backup
```

This creates a `.gitmodules` file:
```
[submodule "Alpha_genome_quickstart_notebook"]
    path = Alpha_genome_quickstart_notebook
    url = git@github.com:gsstephenson/alphagenome-quickstart.git
```

### Step 8: Commit and Push Main Repository
```bash
git add .
git commit -m "Initial commit: Layer Lab rotation project with AlphaGenome quickstart submodule"
git push -u origin main
```

---

## Daily Workflow: Making Changes

### Updating the Submodule Content

When you want to make changes inside the submodule:

```bash
# Navigate to the submodule directory
cd Alpha_genome_quickstart_notebook

# Make your changes to files...

# Commit changes to the submodule
git add .
git commit -m "Description of changes"
git push origin main

# Go back to main repository
cd ..

# Update the main repository to point to the new submodule commit
git add Alpha_genome_quickstart_notebook
git commit -m "Update submodule reference to latest version"
git push origin main
```

**Important**: The main repository stores a reference to a specific commit in the submodule. When you update the submodule, you must commit that new reference in the main repository.

### Updating Files in Main Repository

For files in the main repository (not in submodules):

```bash
# Make changes to files in the main directory...

# Commit normally
git add <files>
git commit -m "Your commit message"
git push origin main
```

---

## Cloning This Repository

### For Others Cloning Your Repository

**Option 1: Clone with submodules (recommended)**
```bash
git clone --recursive git@github.com:gsstephenson/Layer-Laboratory-Rotation.git
```

**Option 2: Clone then initialize submodules**
```bash
git clone git@github.com:gsstephenson/Layer-Laboratory-Rotation.git
cd Layer-Laboratory-Rotation
git submodule init
git submodule update
```

### For You on a New Machine

Same as above, but you'll also need to set up your `.env` file in the submodule:
```bash
cd Alpha_genome_quickstart_notebook
# Create .env file with your API key
echo "ALPHA_GENOME_KEY=your_key_here" > .env
```

---

## Adding New Submodules

If you want to add another submodule to this repository:

### Method 1: Adding an Existing Repository as a Submodule

```bash
# Navigate to main repository
cd /mnt/work_1/gest9386/CU_Boulder/rotations/LAYER

# Add the submodule
git submodule add <repository-url> <directory-name>

# Example:
git submodule add git@github.com:username/another-project.git another_project

# Commit the changes
git add .gitmodules another_project
git commit -m "Add another_project as submodule"
git push origin main
```

### Method 2: Converting an Existing Directory to a Submodule

If you already have a directory that you want to turn into a submodule:

```bash
# Step 1: Initialize Git in the directory
cd existing_directory
git init
git branch -M main

# Step 2: Commit the files
git add .
git commit -m "Initial commit"

# Step 3: Create remote repository (using GitHub CLI)
gh repo create repository-name --public --source=. --remote=origin --push

# Step 4: Go back to main repository
cd ..

# Step 5: Remove the directory from tracking (keeps files)
git rm -r --cached existing_directory

# Step 6: Rename temporarily
mv existing_directory existing_directory_backup

# Step 7: Add as proper submodule
git submodule add git@github.com:username/repository-name.git existing_directory

# Step 8: Remove backup
rm -rf existing_directory_backup

# Step 9: Commit the changes
git add .gitmodules existing_directory
git commit -m "Convert existing_directory to submodule"
git push origin main
```

---

## Useful Submodule Commands

### Check Submodule Status
```bash
git submodule status
```

### Update All Submodules to Latest Commit
```bash
git submodule update --remote
```

### Pull Latest Changes in Main Repo and All Submodules
```bash
git pull --recurse-submodules
```

### Execute a Git Command in All Submodules
```bash
git submodule foreach 'git status'
git submodule foreach 'git pull origin main'
```

### Remove a Submodule
```bash
# Step 1: Deinitialize the submodule
git submodule deinit -f path/to/submodule

# Step 2: Remove from .git/modules
rm -rf .git/modules/path/to/submodule

# Step 3: Remove from working tree
git rm -f path/to/submodule

# Step 4: Commit the changes
git commit -m "Remove submodule_name submodule"
```

---

## Troubleshooting

### Submodule Shows Modified but No Changes
```bash
# Usually happens with line ending or permission differences
cd submodule_directory
git diff
# If it's just whitespace/permissions:
git submodule update --init
```

### Detached HEAD in Submodule
Submodules checkout specific commits by default (detached HEAD state):
```bash
cd submodule_directory
git checkout main  # or your branch name
git pull origin main
cd ..
git add submodule_directory
git commit -m "Update submodule to latest"
```

### Submodule Directory is Empty After Clone
```bash
git submodule init
git submodule update
```

### Authentication Issues
If you get authentication errors:
```bash
# Re-authenticate GitHub CLI
gh auth login

# Or check your SSH keys
ssh -T git@github.com
```

---

## Best Practices

1. **Always commit submodule changes first**, then commit the reference in the main repository
2. **Use descriptive commit messages** that mention submodule updates
3. **Protect sensitive files** with `.gitignore` (like `.env` files with API keys)
4. **Document your submodules** in the main repository's README
5. **Keep submodules focused** - each submodule should represent a logical, independent component
6. **Regular updates** - periodically update submodule references to stay current
7. **Communicate with team** - when updating submodules, notify collaborators so they can update their local copies

---

## Security Notes

- The `.env` file in `Alpha_genome_quickstart_notebook` contains your API key
- This file is excluded via `.gitignore` and will **never be committed**
- Always verify `.gitignore` is working before pushing:
  ```bash
  git status --ignored
  ```
- Never commit API keys, passwords, or credentials to Git repositories

---

## References

- [Git Submodules Official Documentation](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
- [GitHub CLI Documentation](https://cli.github.com/manual/)
- [Pro Git Book - Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules)

---

**Last Updated**: October 26, 2025  
**Repository Owner**: gsstephenson  
**CU Boulder - Layer Laboratory Rotation**
