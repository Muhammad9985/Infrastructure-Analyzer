# GitHub Setup & Deployment Guide

Complete instructions for deploying the Infrastructure Analyzer to GitHub, managing versions, and collaborating.

---

## Table of Contents

1. [Initialize Git Repository](#1-initialize-git-repository)
2. [Create GitHub Repository](#2-create-github-repository)
3. [Push to GitHub](#3-push-to-github)
4. [Branch Strategy](#4-branch-strategy)
5. [Release Management](#5-release-management)
6. [Continuous Integration](#6-continuous-integration)
7. [GitHub Actions Workflows](#7-github-actions-workflows)
8. [Issue Tracking](#8-issue-tracking)
9. [Wiki Documentation](#9-wiki-documentation)

---

## 1. Initialize Git Repository

```bash
cd infrastructure_analyzer/
git init
git add .
git commit -m "Initial commit: Infrastructure Analyzer v1.0.0"
```

The `.gitignore` file already excludes:

- `__pycache__/`, `*.pyc`, `*.pyo`
- `venv/`, `.venv/`
- `dist/`, `build/`
- `*.egg-info/`
- `*.log`, `*.tmp`

---

## 2. Create GitHub Repository

### Via Browser

1. Go to https://github.com/new
2. Repository name: `infrastructure-analyzer`
3. Description: "Geospatial infrastructure analysis tool for urban planning and emergency management"
4. Visibility: Private or Public
5. Do NOT initialize with README, .gitignore, or license (already have them)
6. Click **Create repository**

### Via GitHub CLI

```bash
gh repo create infrastructure-analyzer --private --description "Geospatial infrastructure analysis tool" --source=. --push
```

---

## 3. Push to GitHub

```bash
# Add the remote
git remote add origin https://github.com/your-org/infrastructure-analyzer.git

# Push main branch
git branch -M main
git push -u origin main
```

---

## 4. Branch Strategy

```
main          — Production-ready code, protected branch
├── dev       — Integration branch for feature development
├── feature/* — Individual features (e.g., feature/shapefile-import)
├── fix/*     — Bug fixes (e.g., fix/osm-timeout)
└── release/* — Release preparation branches
```

### Workflow

```bash
# Create a feature branch
git checkout -b feature/shapefile-import dev

# Work, commit, push
git add .
git commit -m "Add Shapefile import support"
git push -u origin feature/shapefile-import

# Create pull request to dev
gh pr create --base dev --head feature/shapefile-import --title "Shapefile Import" --body "Adds .shp import support"

# After review, merge to dev, then create release PR to main
```

### Protected Branches

In GitHub **Settings > Branches**, protect `main`:

- Require pull request reviews (1 reviewer minimum)
- Require status checks (if CI is configured)
- Require up-to-date branches

---

## 5. Release Management

### Semantic Versioning

Given a version number `MAJOR.MINOR.PATCH`:

- **MAJOR** — Incompatible API changes
- **MINOR** — Backward-compatible new features
- **PATCH** — Backward-compatible bug fixes

Current version: **1.0.0**

### Creating a Release

```bash
# From main branch, tag the release
git tag -a v1.0.0 -m "Release v1.0.0 — Initial stable release"
git push origin v1.0.0
```

### GitHub Release

```bash
gh release create v1.0.0 \
    --title "v1.0.0 — Initial Release" \
    --notes "First stable release of Infrastructure Analyzer." \
    --target main
```

Or create via GitHub web interface: **Releases > Create a new release**

### Release Checklist

- [ ] All tests pass: `python tests/test_core.py`
- [ ] Version number updated in `src/gui.py` (`APP_VERSION`)
- [ ] CHANGELOG updated
- [ ] Tag created and pushed
- [ ] Release notes written
- [ ] ZIP archive of source attached to release

---

## 6. Continuous Integration

### Local Testing

Before any push:

```bash
# Run the full test suite
python tests/test_core.py

# Verify all modules compile
python -m py_compile src/gui.py
python -m py_compile src/models.py
python -m py_compile src/map_renderer.py
python -m py_compile src/analysis_engine.py
python -m py_compile src/data_handler.py
python -m py_compile src/report_generator.py

# Verify dependencies
pip install -r requirements.txt
```

---

## 7. GitHub Actions Workflows

Create `.github/workflows/test.yml`:

```yaml
name: Tests

on:
  push:
    branches: [main, dev]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ["3.10", "3.11", "3.12"]

    steps:
      - uses: actions/checkout@v4
      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
      - name: Run tests
        run: python tests/test_core.py
      - name: Compile check
        run: |
          python -m py_compile src/gui.py
          python -m py_compile src/models.py
          python -m py_compile src/map_renderer.py
```

---

## 8. Issue Tracking

### Bug Report Template

Use GitHub Issues with this template:

```markdown
**Describe the bug**
Clear description of the issue.

**To Reproduce**
Steps to reproduce the behavior:
1. Go to Location tab
2. Search for '...'
3. Click '...'
4. See error

**Expected behavior**
What should happen instead.

**Screenshots**
If applicable.

**Environment**
- OS: [e.g. Windows 11]
- Python version: [e.g. 3.13]
- App version: [e.g. 1.0.0]

**Console output**
Paste any error messages from the Console tab.
```

### Feature Request Template

```markdown
**Feature description**
What should the feature do?

**Use case**
Why is this needed? What problem does it solve?

**Proposed implementation**
How should it work? Include mockups if relevant.

**Alternatives considered**
Any other approaches?
```

---

## 9. Wiki Documentation

Enable GitHub Wiki for the repository and create these pages:

### Home
Project overview, quick start, and architecture diagram.

### User Guide
Step-by-step instructions for each feature:
- Searching locations and fetching OSM data
- Running vulnerability analysis
- Exporting reports
- Importing custom datasets

### Developer Guide
- Project structure overview
- Adding new infrastructure categories
- Extending the analysis engine
- Custom tile layers
- Building from source

### API Reference
If the code is refactored into a library, document:
- `InfrastructureNode` — Node model with coordinates and attributes
- `AnalysisEngine` — Vulnerability, cascade, service area methods
- `MapRenderer` — Tile layers, markers, overlays
- `ReportGenerator` — PDF and text report generation

### Deployment
- Installing Python and dependencies
- Virtual environment setup
- Running the application
- Troubleshooting common issues (SSL errors, OSM API limits, Qt platform plugin errors)

---

## Quick Reference

### Essential Git Commands

```bash
git status                    # Check working tree state
git log --oneline -10         # View recent commits
git diff                      # Show unstaged changes
git add <file>                # Stage a file
git commit -m "message"       # Commit staged changes
git push origin main          # Push to remote
git pull origin main          # Pull latest from remote
git checkout -b <branch>      # Create and switch to branch
```

### Essential GitHub CLI Commands

```bash
gh repo create <name>         # Create repository
gh pr create                  # Create pull request
gh pr view                    # View pull request
gh issue create               # Create issue
gh release create <tag>       # Create release
gh run list                   # List workflow runs
```

---

*Developed by Iran Govt*
