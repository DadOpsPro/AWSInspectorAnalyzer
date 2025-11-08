# Streamlit Configuration Setup

## Hiding the Deploy Button

To remove the "Deploy" button from your Streamlit app, create a `.streamlit/config.toml` file in your project root:

```
your-project/
├── .streamlit/
│   └── config.toml
└── VulnerabilityAnalyzer.py
```

The `config.toml` file contains:

```toml
[client]
# Hide the Deploy button in the toolbar
toolbarMode = "minimal"

[server]
# Additional server configs
enableCORS = false
enableXsrfProtection = true

[browser]
# Disable usage stats collection
gatherUsageStats = false
```

## Alternative: Command Line

You can also pass the config via command line:

```bash
streamlit run VulnerabilityAnalyzer.py --client.toolbarMode=minimal
```

## Other Useful Config Options

### Hide Hamburger Menu Completely
```toml
[ui]
hideTopBar = true
```

### Custom Theme
```toml
[theme]
primaryColor = "#F63366"
backgroundColor = "#FFFFFF"
secondaryBackgroundColor = "#F0F2F6"
textColor = "#262730"
font = "sans serif"
```

### Performance Settings
```toml
[server]
maxUploadSize = 200  # Max file upload size in MB
enableStaticServing = true
```

## GitHub Repository Structure

```
vulnerability-analyzer/
├── .streamlit/
│   └── config.toml
├── VulnerabilityAnalyzer.py
├── README.md
├── requirements.txt
└── .gitignore
```

### requirements.txt
```
streamlit>=1.28.0
pandas>=2.0.0
```

### .gitignore
```
*.pyc
__pycache__/
.streamlit/secrets.toml
*.csv
*.json
.DS_Store
```
