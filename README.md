# AWS Inspector Vulnerability Analyzer

A Streamlit-based tool for analyzing AWS Inspector findings across multiple repositories. Designed for DevSecOps teams managing container vulnerabilities at scale.

## Features

- **Multi-Repository Analysis**: Identify CVEs affecting multiple repositories simultaneously
- **Collaborative Review Workflow**: Two-stage review process with annotations and notes
- **Persistent Storage**: Master CSV maintains review history across sessions
- **Smart Filtering**: Focus on new findings while hiding previously reviewed items
- **Export/Import**: Share reviews with team members via CSV
- **Common CVE Detection**: Highlights vulnerabilities from shared base images

## Quick Start

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/vulnerability-analyzer.git
cd vulnerability-analyzer

# Install dependencies
pip install -r requirements.txt

# Run the application
streamlit run VulnerabilityAnalyzer.py
```

### Usage

1. **Upload AWS Inspector JSON files** - Export findings from AWS Inspector
2. **Optional: Upload Master CSV** - Continue from previous review session
3. **Review CVEs by repository** - Add annotations and research notes
4. **Export results** - Save as Master CSV for next session

## Workflow

### First Time Use
1. Upload AWS Inspector JSON exports
2. Annotate each CVE with exploitability status and notes
3. Export to CSV (save as `master_vulnerabilities.csv`)

### Subsequent Use
1. Upload new AWS Inspector JSON exports
2. Upload previous Master CSV
3. **Only new findings are displayed** for review
4. Export updated Master CSV

## Features in Detail

### Common CVE Detection
Automatically identifies CVEs that appear in multiple repositories, which often indicate:
- Shared base image vulnerabilities
- Common dependency issues
- Organization-wide remediation opportunities

### Annotation System
Track exploitability and research for each CVE/repository combination:
- **Exploitable**: Yes / No / Unknown / N/A - False Positive
- **Notes**: Research findings, remediation plans, testing results
- **Apply to All**: Quick annotation for CVEs in shared base images

### Export/Import
- Export includes all findings (new + reviewed) with annotations
- Import previous reviews to continue where you left off
- CSV format compatible with GitLab, JIRA, and other tools

## Configuration

### Remove Deploy Button

Create `.streamlit/config.toml`:

```toml
[client]
toolbarMode = "minimal"

[browser]
gatherUsageStats = false
```

### Custom Theme

Add to `.streamlit/config.toml`:

```toml
[theme]
primaryColor = "#F63366"
backgroundColor = "#FFFFFF"
secondaryBackgroundColor = "#F0F2F6"
```

## AWS Inspector JSON Format

The tool parses standard AWS Inspector JSON exports:

```json
{
  "findings": [
    {
      "packageVulnerabilityDetails": {
        "vulnerabilityId": "CVE-2024-XXXXX",
        "vulnerablePackages": [...]
      },
      "resources": [
        {
          "details": {
            "awsEcrContainerImage": {
              "repositoryName": "my-app",
              "imageTags": ["latest"]
            }
          }
        }
      ],
      "severity": "HIGH"
    }
  ]
}
```

## Use Cases

### DevSecOps Teams
- Centralized vulnerability tracking across microservices
- Coordinate remediation efforts for shared dependencies
- Maintain audit trail of security reviews

### Security Teams
- Triage vulnerabilities by exploitability
- Document research and false positives
- Generate reports for compliance

### Development Teams
- Understand which CVEs affect their repositories
- Track remediation status
- Plan security updates

## Architecture

```
┌─────────────────────┐
│  AWS Inspector      │
│  JSON Exports       │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐      ┌──────────────────┐
│  Streamlit App      │◄─────┤  Master CSV      │
│  - Parse findings   │      │  (Annotations)   │
│  - Display UI       │      └──────────────────┘
│  - Manage state     │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Export CSV         │
│  - All findings     │
│  - All annotations  │
│  - Team sharing     │
└─────────────────────┘
```

## Data Privacy

- All processing happens locally
- No data sent to external services
- Session state stores annotations temporarily
- Export CSV for persistent storage

## Contributing

Contributions welcome! Please feel free to submit a Pull Request.

## License

MIT License - See LICENSE file for details

## Support

For issues and questions:
- Create an issue on GitHub
- Check existing issues for solutions
- Include sample data (redacted) when reporting bugs

## Roadmap

- [ ] JIRA integration for automatic ticket creation
- [ ] GitLab issue creation for remediation tracking
- [ ] Dashboard with metrics and trends
- [ ] CVSS score filtering
- [ ] False positive management system
- [ ] Multi-user collaboration features
