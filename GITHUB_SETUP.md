# GitHub Repository Setup Guide

## Directory Structure

Your GitHub repository should be organized like this:

```
vulnerability-analyzer/
├── .streamlit/
│   └── config.toml          # Hides Deploy button, customizes UI
├── .gitignore               # Prevents committing sensitive data
├── VulnerabilityAnalyzer.py # Main application
├── README.md                # Project documentation
├── requirements.txt         # Python dependencies
├── MASTER_CSV_FEATURE.md   # Master CSV documentation
└── LICENSE                  # Your chosen license (optional)
```

## Step-by-Step Setup

### 1. Create GitHub Repository
```bash
# On GitHub, create a new repository named "vulnerability-analyzer"
# Then locally:

git init
git add .
git commit -m "Initial commit: AWS Inspector Vulnerability Analyzer"
git branch -M main
git remote add origin https://github.com/yourusername/vulnerability-analyzer.git
git push -u origin main
```

### 2. File Checklist

All files are provided in your outputs folder:

- ✅ `VulnerabilityAnalyzer.py` - Main application
- ✅ `.streamlit/config.toml` - Config (hides Deploy button)
- ✅ `requirements.txt` - Dependencies
- ✅ `.gitignore` - Excludes sensitive files
- ✅ `README.md` - Project documentation
- ✅ `MASTER_CSV_FEATURE.md` - Feature documentation

### 3. Test Locally Before Pushing

```bash
# Create the directory structure
mkdir vulnerability-analyzer
cd vulnerability-analyzer

# Copy files from outputs folder
cp /path/to/outputs/VulnerabilityAnalyzer.py .
cp /path/to/outputs/requirements.txt .
cp /path/to/outputs/.gitignore .
cp /path/to/outputs/README.md .
cp /path/to/outputs/MASTER_CSV_FEATURE.md .

# Create .streamlit directory
mkdir .streamlit
cp /path/to/outputs/.streamlit/config.toml .streamlit/

# Test it works
pip install -r requirements.txt
streamlit run VulnerabilityAnalyzer.py
```

### 4. Verify Deploy Button is Hidden

When you run `streamlit run VulnerabilityAnalyzer.py`, you should see a minimal toolbar without the "Deploy" button. The hamburger menu will still be present but cleaner.

## Quick Commands Reference

### Running the Application
```bash
streamlit run VulnerabilityAnalyzer.py
```

### Running with Custom Port
```bash
streamlit run VulnerabilityAnalyzer.py --server.port 8502
```

### Running with Additional Config
```bash
streamlit run VulnerabilityAnalyzer.py --client.toolbarMode=minimal --server.maxUploadSize=200
```

## Repository Settings Recommendations

### GitHub Topics (for discoverability)
- `aws-inspector`
- `vulnerability-management`
- `devsecops`
- `security-tools`
- `streamlit`
- `container-security`
- `cve-tracking`

### Branch Protection (for production use)
- Require pull request reviews
- Require status checks to pass
- Require branches to be up to date

### GitHub Actions (optional - for CI/CD)
Create `.github/workflows/test.yml`:
```yaml
name: Test

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.9'
      - run: pip install -r requirements.txt
      - run: python -m py_compile VulnerabilityAnalyzer.py
```

## Adding a License

Consider adding a LICENSE file. For open source, MIT is popular:

```bash
# Add MIT License
curl -o LICENSE https://raw.githubusercontent.com/licenses/license-templates/master/templates/mit.txt
# Edit LICENSE to add your name and year
```

## README Badges (optional)

Add to top of README.md:
```markdown
![Python](https://img.shields.io/badge/python-3.9+-blue.svg)
![Streamlit](https://img.shields.io/badge/streamlit-1.28+-red.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
```

## Next Steps

1. ✅ Push initial code to GitHub
2. 📝 Add screenshots to README (take screenshots of your app in action)
3. 🏷️ Create v1.0.0 release tag
4. 📢 Share with your team
5. 🔄 Set up CI/CD (optional)

## Troubleshooting

### Deploy Button Still Showing?
- Verify `.streamlit/config.toml` exists in the correct location
- Check file contains `toolbarMode = "minimal"`
- Restart Streamlit after config changes
- Clear browser cache

### Config Not Loading?
```bash
# Check config is in right place
ls -la .streamlit/config.toml

# Verify syntax
cat .streamlit/config.toml
```

### Large Files Warning?
If your test CSV/JSON files are >100MB, Git will warn you:
```bash
# Use git-lfs for large files (optional)
git lfs install
git lfs track "*.csv"
git lfs track "*.json"
```

Or better: **Don't commit data files** (they're in .gitignore)
