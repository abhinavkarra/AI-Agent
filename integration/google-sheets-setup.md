# Google Sheets Integration Setup

## Overview
This guide covers the complete setup process for integrating the AI Agent with Google Sheets to capture customer handoff requests.

## Prerequisites
- Google account with access to Google Sheets
- Tars AI Agent account with integration capabilities
- Basic understanding of spreadsheet management

## Step 1: Create the Google Sheet

### 1.1 Sheet Setup
1. Go to [Google Sheets](https://sheets.google.com)
2. Create a new blank spreadsheet
3. Name it: "Customer Support Requests - [Current Date]"
4. Save it to a dedicated folder (e.g., "AI Agent Data")

### 1.2 Column Structure
Set up the following columns (Row 1 headers):

| Column | Header | Data Type | Description |
|--------|--------|-----------|-------------|
| A | Timestamp | DateTime | Auto-generated submission time |
| B | Customer Name | Text | Full name from form |
| C | Email Address | Email | Customer's email address |
| D | Question/Issue | Text | Detailed description of the issue |
| E | Contact Preference | Text | Email, Phone, or No Preference |
| F | Priority Level | Text | Low, Medium, High, or Urgent |
| G | Session ID | Text | Unique identifier for the chat session |
| H | Status | Text | New, In Progress, Resolved |
| I | Assigned Agent | Text | Human agent assigned to the case |
| J | Resolution Notes | Text | Notes added by human agents |
| K | Follow-up Required | Boolean | Yes/No for follow-up needed |

### 1.3 Format the Headers
1. Select row 1 (headers)
2. Make text **bold**
3. Add background color (light blue or gray)
4. Center-align the text
5. Freeze the first row (View → Freeze → 1 row)

### 1.4 Data Validation (Optional)
Add data validation for specific columns:

**Column E (Contact Preference):**
```
Data → Data validation
Criteria: List of items
Items: Email, Phone, No Preference
```

**Column F (Priority Level):**
```
Data → Data validation
Criteria: List of items
Items: Low, Medium, High, Urgent
```

**Column H (Status):**
```
Data → Data validation
Criteria: List of items
Items: New, In Progress, Resolved
```

## Step 2: Configure Sheet Permissions

### 2.1 Sharing Settings
1. Click "Share" button in the top right
2. Change access to "Anyone with the link can edit"
3. Copy the sheet URL for later use
4. Consider creating a service account for better security (advanced)

### 2.2 Service Account Setup (Recommended for Production)
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a new project or select existing one
3. Enable Google Sheets API
4. Create service account credentials
5. Download the JSON key file
6. Share the sheet with the service account email

## Step 3: Tars Integration Configuration

### 3.1 Access Integration Settings
1. Log into your Tars dashboard
2. Navigate to your AI Agent project
3. Go to Settings → Integrations
4. Select "Google Sheets" from available integrations

### 3.2 Authentication Setup
**Option A: Direct Sheet URL (Simple)**
1. Select "Public Sheet URL" method
2. Paste your Google Sheet URL
3. Test the connection

**Option B: Service Account (Secure)**
1. Select "Service Account" method
2. Upload the JSON key file
3. Enter the sheet ID
4. Test the connection

### 3.3 Field Mapping Configuration
Map the form fields to spreadsheet columns:

```json
{
  "field_mappings": {
    "timestamp": "A",
    "customer_name": "B",
    "email": "C",
    "question": "D",
    "contact_preference": "E",
    "urgency": "F",
    "session_id": "G"
  },
  "default_values": {
    "status": "New",
    "assigned_agent": "",
    "resolution_notes": "",
    "follow_up_required": ""
  }
}
```

### 3.4 Advanced Settings
```json
{
  "append_mode": true,
  "sheet_name": "Sheet1",
  "header_row": 1,
  "timestamp_format": "MM/DD/YYYY HH:MM:SS",
  "duplicate_prevention": false,
  "error_handling": "log_and_continue"
}
```

## Step 4: Testing the Integration

### 4.1 Test Data Submission
Create test submissions with various scenarios:

**Test Case 1: Complete Form**
```
Name: Test Customer 1
Email: test1@example.com
Question: Testing the integration with complete data
Contact Preference: Email
Priority: Medium
```

**Test Case 2: Minimal Required Fields**
```
Name: Test Customer 2
Email: test2@example.com
Question: Testing with only required fields
Contact Preference: (empty)
Priority: (empty)
```

**Test Case 3: Special Characters**
```
Name: José García-Smith
Email: jose.garcia+test@example.com
Question: Testing with special characters and symbols: !@#$%^&*()
Contact Preference: Phone
Priority: High
```

### 4.2 Verification Checklist
- [ ] Data appears in correct columns
- [ ] Timestamp is properly formatted
- [ ] Email validation works
- [ ] Special characters are handled correctly
- [ ] Empty optional fields don't cause errors
- [ ] Session ID is unique for each submission

## Step 5: Monitoring and Maintenance

### 5.1 Create Dashboard View
1. Add a summary section at the top of the sheet:
   - Total requests today
   - Pending requests
   - High priority items
   - Average response time

2. Use formulas for automatic calculations:
```excel
Total Today: =COUNTIF(A:A,">="&TODAY())
Pending: =COUNTIF(H:H,"New")+COUNTIF(H:H,"In Progress")
High Priority: =COUNTIF(F:F,"High")+COUNTIF(F:F,"Urgent")
```

### 5.2 Automated Notifications (Optional)
Set up Google Sheets notifications:
1. Tools → Notification rules
2. Configure alerts for:
   - New high-priority submissions
   - Sheets reaching capacity
   - Daily summary reports

### 5.3 Data Management
**Regular Maintenance Tasks:**
- Archive old data monthly
- Review and update validation rules
- Monitor sheet performance
- Backup data regularly

**Sheet Performance Optimization:**
- Limit to 10,000 rows per sheet
- Use multiple sheets for different time periods
- Consider Google Apps Script for advanced automation

## Step 6: Troubleshooting

### Common Issues and Solutions

**Issue: Data not appearing in sheet**
- Check sheet permissions
- Verify integration credentials
- Test with simple data first
- Check Tars integration logs

**Issue: Incorrect data formatting**
- Review field mapping configuration
- Check timestamp format settings
- Verify data validation rules

**Issue: Duplicate entries**
- Enable duplicate prevention if needed
- Check for multiple form submissions
- Review session ID uniqueness

**Issue: Authentication errors**
- Refresh service account credentials
- Check sheet sharing permissions
- Verify API access is enabled

### Support Resources
- Tars Integration Documentation
- Google Sheets API Documentation
- Community forums and help centers
- Direct support contacts

## Step 7: Security Considerations

### Data Protection
- Use service accounts instead of personal accounts
- Regularly rotate API credentials
- Implement proper access controls
- Monitor for unauthorized access

### Compliance
- Ensure GDPR compliance for EU customers
- Implement data retention policies
- Provide data deletion capabilities
- Maintain audit trails

### Privacy
- Limit access to authorized personnel only
- Use encrypted connections (HTTPS)
- Regular security audits
- Employee training on data handling