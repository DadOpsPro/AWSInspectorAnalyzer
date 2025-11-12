# AWS Inspector Vulnerability Analyzer

A Streamlit-based tool for analyzing AWS Inspector findings across multiple repositories. Designed for DevSecOps teams managing container vulnerabilities at scale.

## Features

- **Multi-Repository Analysis**: Identify CVEs affecting multiple repositories simultaneously
- **Excel Integration**: Import/export with one sheet per repository for easy organization
- **Smart Filtering**: Automatically shows only new findings based on Master Excel file
- **Persistent Storage**: Master Excel maintains review history across sessions
- **Quick Annotations**: Mark exploitability status and add research notes
- **Bulk Updates**: Apply annotations to all affected repositories at once
- **Common CVE Detection**: Highlights vulnerabilities from shared base images
- **Business-Friendly Format**: Professional Excel reports for management

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

### Requirements

- Python 3.8+
- streamlit >= 1.28.0
- pandas >= 2.0.0
- openpyxl >= 3.0.0

### Usage

1. **Upload AWS Inspector JSON files** - Export findings from AWS Inspector
2. **Optional: Upload Master Excel** - Continue from previous review session
3. **Review CVEs by repository** - Add annotations and research notes
4. **Export results** - Save as `Security_Assessment_Report_[date].xlsx`

## Workflow

### First Time Use
1. Upload AWS Inspector JSON exports
2. Annotate each CVE with exploitability status and notes
3. Export to Excel (each repository gets its own sheet)
4. Save as your Master Excel file

### Subsequent Use
1. Upload new AWS Inspector JSON exports
2. Upload previous Master Excel file
3. **Only new findings are displayed** for review (previously reviewed items are hidden)
4. Export updated Excel file with all findings

## Master Excel File Format

### Structure
Your Master Excel file should have **one sheet per repository/image**:

```
master_vulnerabilities.xlsx
├── api-service (Sheet 1)
├── web-frontend (Sheet 2)
├── auth-service (Sheet 3)
└── payment-processor (Sheet 4)
```

### Required Columns

Each sheet must have these columns (in order):

1. **CVE** - CVE identifier (e.g., CVE-2024-1234)
2. **Severity** - CRITICAL, HIGH, MEDIUM, LOW
3. **Repository** - Repository/image name
4. **Image Tag** - Container tag (e.g., latest, v1.2.3)
5. **Package** - Combined package:version (e.g., `pam:1.3.1`, `openssl:1.1.1k`)
6. **Fixed In** - Version where fixed (e.g., 1.1.1w, N/A)
7. **AWS Account** - AWS account ID
8. **Exploitable** - Yes, No, Unknown, or N/A - False Positive
9. **Notes** - Research notes (empty = new finding, has text = reviewed)
10. **First Seen** - Timestamp (e.g., 2024-11-08T14:23:45)

### Key Points

- **Package format**: Combined as `package:version` (e.g., `pam:1.3.1`)
- **Notes column**: Empty notes = NEW finding (displayed), Has notes = REVIEWED (hidden)
- **First Seen**: Records when the vulnerability was first identified
- **One sheet per repository**: Makes navigation and organization easy

## Features in Detail

### Smart Filtering
When you upload a Master Excel file, the tool:
- Loads all previous annotations from all sheets
- Identifies which CVEs have been reviewed (have notes)
- **Displays only NEW findings** (those without notes)
- Shows count: "Loaded X total findings. There are Y new findings for review."

This way you never waste time re-reviewing the same vulnerabilities.

### Common CVE Detection
Automatically identifies CVEs that appear in multiple repositories, which often indicate:
- Shared base image vulnerabilities
- Common dependency issues
- Organization-wide remediation opportunities

For these CVEs, you can use "Apply to All" to annotate all affected repositories at once.

### Annotation System
Track exploitability and research for each CVE/repository combination:
- **Exploitable**: Yes / No / Unknown / N/A - False Positive
- **Notes**: Research findings, remediation plans, testing results
- **Apply to All**: Quick annotation for CVEs in shared base images
- **Timestamp**: Automatically records when annotation was made

### Export Options

**Excel Export (Recommended):**
- One sheet per repository (automatic organization)
- Filename: `Security_Assessment_Report_YYYY-MM-DD.xlsx`
- Auto-sized columns for readability
- Maintains format for next import
- Professional appearance for management

**CSV Export (Alternative):**
- Single file with all data
- Compatible with other tools
- Less organized than Excel

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
        "vulnerablePackages": [
          {
            "name": "openssl",
            "version": "1.1.1k",
            "fixedInVersion": "1.1.1w"
          }
        ]
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
- Focus on new vulnerabilities each sprint

### Security Teams
- Triage vulnerabilities by exploitability
- Document research and false positives
- Generate professional reports for management
- Track remediation progress over time

### Development Teams
- Understand which CVEs affect their repositories
- Track remediation status
- Plan security updates
- Avoid duplicate research on same CVEs

## Architecture

```
┌─────────────────────┐
│  AWS Inspector      │
│  JSON Exports       │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐      ┌──────────────────────┐
│  Streamlit App      │◄─────┤  Master Excel        │
│  - Parse findings   │      │  (Multi-sheet)       │
│  - Smart filtering  │      │  - One per repo      │
│  - Display UI       │      │  - All annotations   │
│  - Manage state     │      └──────────────────────┘
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Excel Export       │
│  - All findings     │
│  - All annotations  │
│  - One sheet/repo   │
│  - Team sharing     │
└─────────────────────┘
```

## Data Privacy

- All processing happens locally
- No data sent to external services
- Session state stores annotations temporarily
- Export Excel for persistent storage
- No database or external infrastructure needed

## Example Workflow

**Monday Morning:**
1. Get latest AWS Inspector scan results (JSON)
2. Upload JSON files to tool
3. Upload Friday's Master Excel file
4. Tool shows: "25 new findings for review"
5. Review the 25 new CVEs
6. Export: `Security_Assessment_Report_2024-11-11.xlsx`

**Result:** Complete history maintained, no duplicate work, organized by repository!

## Team Collaboration

While designed for single-reviewer workflow, the tool supports team collaboration:

1. **First reviewer** completes annotations and exports Excel
2. **Share** the Excel file with team (email, OneDrive, SharePoint)
3. **Team members** can review annotations in familiar Excel format
4. **Import back** into tool for updates or additional scanning

## Tips

### Maintaining Your Master Excel
- Always export after a review session
- Use descriptive filenames with dates
- Keep backups of Master Excel files
- One Master Excel file per environment (dev, staging, prod)

### Efficient Reviews
- Upload Master Excel at start of each session
- Focus only on new findings displayed
- Use "Apply to all repos" for common CVEs
- Add detailed notes for future reference

### Working with Excel
- Each repository has its own tab for easy navigation
- Use Excel's native sorting/filtering on each sheet
- Can add Excel comments alongside Notes column
- Share via email or OneDrive/SharePoint

