# Vulnerability Analyzer - Master CSV Feature

## Overview
The analyzer now supports loading a "Master CSV" file to continue working on previously reviewed vulnerabilities. This allows you to:

1. **Maintain a persistent database** of all CVE reviews across multiple scanning sessions
2. **Focus on new findings** - only unreviewed CVEs are displayed for annotation
3. **Export complete records** - both new and previously reviewed findings are included in exports

## How It Works

### Workflow

#### First Time Use (No Master CSV)
1. Upload AWS Inspector JSON files
2. Review and annotate CVEs
3. Export to CSV - **save this as your Master CSV**

#### Subsequent Use (With Master CSV)
1. Upload AWS Inspector JSON files
2. **Upload your Master CSV** (from previous session)
3. System loads previous annotations automatically
4. **Only NEW findings are displayed** for review
5. Export updated CSV - this becomes your new Master CSV

## Features

### Smart Filtering
- **New Findings Only**: Only CVEs without notes are displayed for review
- **Status Message**: Shows `"Loaded X total findings from Y files. There are Z new findings for review."`
- **Filtering Info**: Displays how many previously reviewed findings are hidden

### Complete Exports
- Export tab includes **ALL findings** (new + previously reviewed)
- Master CSV can be uploaded to any future session
- All previous annotations are preserved

### Example Status Messages

```
✅ Loaded 150 total findings from 3 files. There are 25 new findings for review.
ℹ️ Displaying only new findings. 125 previously reviewed findings are hidden but will be included in exports.
```

Or if everything has been reviewed:

```
✅ Loaded 150 total findings from 3 files. There are 0 new findings for review.
⚠️ No new findings! All items have been previously reviewed. Showing all findings for reference.
```

## Master CSV Format

The Master CSV uses the same format as the export CSV:

| Column | Description |
|--------|-------------|
| CVE | CVE identifier |
| Severity | CRITICAL, HIGH, MEDIUM, LOW |
| Repository | Repository name |
| Image Tag | Container image tag |
| Package | Vulnerable package name |
| Version | Current package version |
| Fixed In | Version where vulnerability is fixed |
| AWS Account | AWS account ID |
| Common | "Yes" if affects multiple repos |
| Exploitable | Review status: Yes/No/Unknown/N/A - False Positive |
| Notes | Your research and findings |
| Last Updated | Timestamp of last review |

## Tips

### Maintaining Your Master CSV
1. **Always export after a review session** - this creates your updated Master CSV
2. **Use descriptive filenames** - e.g., `master_vulnerabilities_2024-11-08.csv`
3. **Keep backups** - Master CSV contains your entire review history

### Efficient Reviews
1. Upload Master CSV at the start of each session
2. Focus only on new findings displayed
3. Use "Apply to all repos" for common CVEs
4. Export when done to update Master CSV

### Team Collaboration
1. First reviewer exports Master CSV
2. Team imports same AWS Inspector JSON + Master CSV
3. Each team member sees same data
4. Final reviewer exports complete Master CSV for records

## Technical Details

### What Makes a Finding "New"?
A finding is considered new if its `Notes` field is empty or contains only whitespace. Once you add notes to a CVE/Repository combination, it's marked as reviewed.

### Data Persistence
- Annotations are stored in session state during review
- Master CSV provides persistence between sessions
- Export always includes ALL findings for complete records

## Important Notes

- ⚠️ **Always use the latest export as your Master CSV** - it contains the most recent annotations
- 💾 **Export tab shows ALL findings** - even those filtered from display
- 🔄 **Re-uploading Master CSV is safe** - it will reload all previous annotations
- 📊 **Multiple runs same day**: Latest export contains all cumulative reviews
