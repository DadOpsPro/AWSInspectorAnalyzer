# Your Master Excel File Format

## Required Columns (In Order)

Based on your format where Package and Version are combined (e.g., `pam:1.3.1`):

1. **CVE** - CVE identifier (e.g., CVE-2024-1234)
2. **Severity** - CRITICAL, HIGH, MEDIUM, LOW
3. **Repository** - Repository/image name
4. **Image Tag** - Container image tag (e.g., latest, v1.2.3)
5. **Package** - Combined package:version (e.g., `pam:1.3.1`, `openssl:1.1.1k`)
6. **Fixed In** - Version where fixed (e.g., 1.1.1w, N/A)
7. **AWS Account** - AWS account ID
8. **Exploitable** - Yes, No, Unknown, or N/A - False Positive
9. **Notes** - Your research notes (empty = new finding, has text = reviewed)
10. **First Seen** - Timestamp (e.g., 2024-11-08T14:23:45)

## Example Sheet: my-api-service

| CVE | Severity | Repository | Image Tag | Package | Fixed In | AWS Account | Exploitable | Notes | First Seen |
|-----|----------|------------|-----------|---------|----------|-------------|-------------|-------|------------|
| CVE-2024-1234 | HIGH | my-api-service | latest | pam:1.3.1 | 1.5.0 | 123456789012 | Yes | Exploitable via auth bypass | 2024-11-08T10:00:00 |
| CVE-2024-5678 | MEDIUM | my-api-service | latest | openssl:1.1.1k | 1.1.1w | 123456789012 | No | Not exposed publicly | 2024-11-08T10:05:00 |
| CVE-2024-9999 | CRITICAL | my-api-service | v2.1.0 | nginx:1.18.0 | 1.20.2 | 123456789012 | Unknown |  |  |

## Key Points

✅ **Package format**: `package:version` (combined in one column with colon separator)
✅ **No "Common" column**: Tool calculates this automatically
✅ **"First Seen" not "Last Updated"**: Your naming convention
✅ **Notes determines review status**: 
   - Empty Notes = NEW finding (will display)
   - Has Notes = REVIEWED (hidden but included in export)

## What the Tool Does

### On Import:
- Reads all sheets from your Excel file
- Accepts "First Seen" column name
- Handles combined Package:Version format
- Loads reviewed items (where Notes has text)
- Shows only new items (where Notes is empty)

### On Export:
- Creates Excel with same format
- Combines Package:Version as `package:version`
- Uses "First Seen" column name (not "Last Updated")
- No "Common" column in export
- One sheet per repository

## File Structure

```
master_vulnerabilities.xlsx
├── api-service (Sheet 1)
├── web-frontend (Sheet 2)
├── auth-service (Sheet 3)
└── payment-processor (Sheet 4)
```

Each sheet has the 10 columns listed above.

## Workflow

**Upload existing Master Excel:**
1. Tool reads all sheets
2. Loads annotations from Notes column
3. Shows only CVEs with empty Notes (new findings)

**After annotation:**
1. Export to Excel
2. Maintains your format (Package:Version combined, "First Seen" column)
3. Use exported file as your next Master Excel

The tool is already configured for your format! Just upload your existing Master Excel file and it will work.