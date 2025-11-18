# Test Data for AWS Inspector Vulnerability Analyzer

## 📁 Files Included

### JSON Files (Simulated AWS Inspector Exports)
1. **api-service_findings.json** - 4 vulnerabilities
2. **web-frontend_findings.json** - 4 vulnerabilities  
3. **auth-service_findings.json** - 3 vulnerabilities

### Excel File (Simulated Master File)
**Sample_Master_Vulnerabilities.xlsx** - Previous scan results with 3 sheets

---

## 🎯 Test Scenario

This test data demonstrates both new features:

### Feature 1: Resolved CVE Tracking ✅

When you upload the master Excel + the 3 JSON files, you should see:

**🎉 3 vulnerabilities resolved!**
- CVE-2024-2222 in api-service (curl vulnerability)
- CVE-2024-1111 in auth-service (python-requests)
- CVE-2024-9876 in auth-service (tzdata)

These CVEs exist in the master file but are **not present** in the new JSON files, indicating they were successfully remediated!

### Feature 2: Dual Export Options ✅

**Option 1: Export Updated Master**
- Will include all 12 CVEs from master
- Will mark 3 as "Resolved"
- Will add 2 new CVEs
- Total: 14 CVEs in export

**Option 2: Export Latest Findings Only**
- Will include only 11 CVEs from JSON files
- Won't include the 3 resolved CVEs
- Clean current-state view

---

## 📊 Expected Results

### Common CVEs (Should Detect)
The tool should identify these CVEs appearing in multiple repositories:

1. **CVE-2024-1234** (OpenSSL) - In BOTH api-service AND web-frontend
   - Critical severity
   - Shared base image issue

2. **CVE-2024-5678** (glibc) - In BOTH api-service AND web-frontend
   - High severity
   - Shared base image issue

### New Findings (Not in Master)
These CVEs appear in JSON but not in master:

1. **CVE-2024-9999** (Python SQLAlchemy) - In api-service
   - High severity
   - NEW vulnerability requiring assessment

2. **CVE-2024-8888** (PyJWT) - In auth-service
   - High severity
   - NEW vulnerability requiring assessment

### Resolved CVEs (In Master, Not in JSON)
These CVEs were in the master file but are no longer present:

1. **CVE-2024-2222** (curl) - Was in api-service
   - Successfully patched! 🎉

2. **CVE-2024-1111** (python-requests) - Was in auth-service
   - Successfully patched! 🎉

3. **CVE-2024-9876** (tzdata) - Was in auth-service
   - Successfully patched! 🎉

### Active CVEs (In Both Master and JSON)
These CVEs persist across scans:

1. CVE-2024-1234 (OpenSSL) - api-service, web-frontend
2. CVE-2024-5678 (glibc) - api-service, web-frontend
3. CVE-2024-3333 (Linux Kernel) - api-service
4. CVE-2024-7777 (Node.js) - web-frontend
5. CVE-2024-4444 (Nginx) - web-frontend
6. CVE-2024-6666 (PostgreSQL) - auth-service
7. CVE-2024-5555 (Redis) - auth-service

---

## 🧪 How to Test

### Step 1: Upload Files
```
1. Start Streamlit: streamlit run VulnerabilityAnalyzer.py
2. Upload all 3 JSON files at once
3. Upload Sample_Master_Vulnerabilities.xlsx
```

### Step 2: Observe Resolved CVE Notification
You should see:
```
🎉 3 vulnerabilities are no longer present in the latest scan
(will be marked as 'Resolved' in export)

[View Resolved CVEs ▼]
• CVE-2024-2222 in api-service
• CVE-2024-1111 in auth-service
• CVE-2024-9876 in auth-service
```

### Step 3: Check New Findings
You should see:
```
✅ Loaded 11 total findings from 3 files. 
   There are 2 new findings for review.

ℹ️ Displaying only new findings. 
   9 previously reviewed findings are hidden but will be included in exports.
```

### Step 4: Review Common CVEs Tab
Should show:
```
Common CVEs
CVEs affecting multiple repositories

🔴 CVE-2024-1234 [CRITICAL] - 2 repos
   • api-service
   • web-frontend

🟠 CVE-2024-5678 [HIGH] - 2 repos
   • api-service
   • web-frontend
```

### Step 5: Test Export Options

**Export Option 1 (Updated Master):**
```
Metrics displayed:
┌──────┬────────┬──────────┬─────┐
│Total │ Active │ Resolved │ New │
│  14  │   11   │    -3    │ +2  │
└──────┴────────┴──────────┴─────┘
```

Download and verify:
- 14 total CVEs (11 active + 3 resolved)
- 3 CVEs marked with Status = "Resolved"
- 2 CVEs marked with Status = "New"
- 9 CVEs marked with Status = "Active"

**Export Option 2 (Latest Only):**
- Should have 11 CVEs (only from JSON files)
- No resolved CVEs included
- 2 marked as "New"
- 9 marked as "Active"

---

## 📋 Data Summary

### By Repository

**api-service:**
- 4 CVEs in JSON (1 new: CVE-2024-9999)
- 4 CVEs in master (1 resolved: CVE-2024-2222)
- 3 active, 1 new, 1 resolved

**web-frontend:**
- 4 CVEs in JSON (all previously known)
- 4 CVEs in master (all still active)
- 4 active, 0 new, 0 resolved

**auth-service:**
- 3 CVEs in JSON (1 new: CVE-2024-8888)
- 4 CVEs in master (2 resolved: CVE-2024-1111, CVE-2024-9876)
- 3 active, 1 new, 2 resolved

### By Severity

**CRITICAL (4 CVEs):**
- CVE-2024-1234 (OpenSSL) - Common across 2 repos
- CVE-2024-6666 (PostgreSQL) - auth-service

**HIGH (7 CVEs):**
- CVE-2024-5678 (glibc) - Common across 2 repos
- CVE-2024-9999 (SQLAlchemy) - api-service [NEW]
- CVE-2024-7777 (Node.js) - web-frontend
- CVE-2024-8888 (PyJWT) - auth-service [NEW]
- CVE-2024-5555 (Redis) - auth-service
- CVE-2024-2222 (curl) - [RESOLVED]
- CVE-2024-1111 (requests) - [RESOLVED]

**MEDIUM (2 CVEs):**
- CVE-2024-3333 (Kernel) - api-service
- CVE-2024-4444 (Nginx) - web-frontend

**LOW (1 CVE):**
- CVE-2024-9876 (tzdata) - [RESOLVED]

---

## 🎓 Teaching Points

This test data demonstrates:

1. **Lifecycle Tracking**: See how CVEs move from Active → Resolved
2. **Common Vulnerabilities**: Identify shared base image issues
3. **New Threat Detection**: Spot newly discovered vulnerabilities
4. **Annotation Preservation**: All notes from master are preserved
5. **Multi-Repository View**: Understand impact across services
6. **Export Flexibility**: Choose the right export for your needs

---

## 🔧 Customizing Test Data

Want to modify the test data?

### Add More CVEs to JSON:
Edit the JSON files and add more finding objects following the same structure.

### Add More Repositories:
Create a new JSON file like `payment-service_findings.json` following the same format.

### Add More Historical Data:
Edit `Sample_Master_Vulnerabilities.xlsx` to add more sheets or rows.

### Simulate Different Scenarios:

**Scenario A: All CVEs Resolved**
- Remove all findings from JSON files
- Upload master + empty JSONs
- See all CVEs marked as resolved!

**Scenario B: No Changes**
- Make JSON CVEs match master exactly
- Upload both
- See "0 new findings, 0 resolved"

**Scenario C: Major Outbreak**
- Add 20+ new CVEs to JSON files
- Upload master + JSONs
- See high count of new findings

---

## 💡 Pro Tips for Testing

1. **Test Common CVE Detection**: Look for the 🔗 COMMON badge on CVE-2024-1234 and CVE-2024-5678

2. **Test Bulk Annotation**: Try using "Apply to all repos" for the common CVEs

3. **Test Filtering**: Notice how previously reviewed findings are hidden from the By Repository tab

4. **Test Both Exports**: Download both Option 1 and Option 2, compare in Excel

5. **Test Annotations**: Add notes to new CVEs, export, then re-upload to verify persistence

---

## ✅ Success Criteria

After testing with this data, you should have verified:

- [x] Resolved CVE notification appears
- [x] Count shows "3 vulnerabilities resolved"
- [x] Expandable list shows correct 3 CVEs
- [x] New findings count shows 2
- [x] Common CVEs tab shows 2 shared vulnerabilities
- [x] Option 1 export includes all 14 CVEs (11 active + 3 resolved)
- [x] Option 2 export includes only 11 CVEs (current scan)
- [x] Status column correctly shows New/Active/Resolved
- [x] Previous annotations are preserved
- [x] Metrics dashboard shows correct counts

---

## 🐛 Troubleshooting

**Issue: No resolved CVEs showing**
- Verify you uploaded the master Excel BEFORE the JSONs
- Check that repository names match exactly

**Issue: Wrong CVE counts**
- Clear browser cache and refresh
- Restart Streamlit app

**Issue: Can't see annotations**
- Check the "Notes" column in master Excel
- Ensure notes are not empty

---

## 📞 Support

This test data is designed to demonstrate all features. If something doesn't work as expected, please open a GitHub issue with:
- Screenshots of the unexpected behavior
- Browser console errors (F12)
- Steps to reproduce

Happy testing! 🚀
