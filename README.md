# Google Docs Mail Merge Script

## Overview
This Google Apps Script automates the process of creating personalized documents from a template using data from Google Sheets. The script can handle multiple data sources and supports dynamic merging of data into both document body and footer sections.

## Features
- 🔄 Automated document generation from templates
- 📊 Support for multiple data sources
- 📝 Dynamic field mapping
- 👤 Personalized document naming
- 📄 Footer section support
- 🎯 Custom menu integration

## Prerequisites
- Google Workspace account
- Access to Google Drive, Docs, and Sheets
- Necessary permissions to create and modify documents

## Setup

### 1. Configuration Spreadsheet
Create a sheet named "Config" in your bound spreadsheet with the following columns:
- FIRST_NAME
- LAST_NAME
- ADDRESS
- EMAIL
- DOCS_FILE_ID (Google Doc template ID)
- SHEETS_FILE_ID (Data source spreadsheet ID)

### 2. Template Document
1. Create a template Google Doc
2. Use placeholders in the format `{{FIELD_NAME}}`:
   - `{{FIRST_NAME}}`
   - `{{LAST_NAME}}`
   - `{{ADDRESS}}`
   - `{{EMAIL}}`

### 3. Data Source Spreadsheet
Create a spreadsheet with matching column headers:
- FIRST_NAME
- LAST_NAME
- ADDRESS
- EMAIL

## Usage

### Method 1: Using Custom Menu
1. Open the bound spreadsheet
2. Click on the "🗂 Merge and Dupe" menu
3. Select "📂 Merge and Dupe Sheets"

### Method 2: Running Script Manually
1. Open Script Editor
2. Run the `main()` function

## Functions

### `getConfigData()`
Retrieves configuration data from the bound spreadsheet.

### `getColumnMapping(headers)`
Maps column headers to their respective indices.

### `getSheetsData(sheetsFileId)`
Fetches data from specified Google Sheets file.

### `copyTemplate(docFileId, firstName, lastName)`
Creates a copy of the template document.

### `mergeTemplate(mergeData)`
Populates template with data and returns the new document ID.

### `main()`
Main execution function that orchestrates the merge process.

## Error Handling
- The script includes logging for missing file IDs
- Handles missing sheets and invalid data gracefully
- Provides feedback through Logger

## Limitations
- Template must be a Google Doc
- Data source must be a Google Sheet
- Placeholders must match exact column names
- Footer sections are optional

## Contributing
Feel free to submit issues and enhancement requests!


# Input/Output Examples

## Configuration Sheet ("Config") Example

### Input
Your configuration sheet should look like this:

| FIRST_NAME | LAST_NAME | ADDRESS | EMAIL | DOCS_FILE_ID | SHEETS_FILE_ID |
|------------|-----------|---------|-------|--------------|----------------|
| [empty] | [empty] | [empty] | [empty] | 1abc...xyz | 2def...uvw |
| [empty] | [empty] | [empty] | [empty] | 3ghi...rst | 4jkl...mno |

Note: The FIRST_NAME, LAST_NAME, ADDRESS, and EMAIL columns in the Config sheet should be empty as they will be populated from the data source sheets.

## Data Source Spreadsheet Example

### Input
Your data source spreadsheet (referenced by SHEETS_FILE_ID) should look like this:

| FIRST_NAME | LAST_NAME | ADDRESS | EMAIL |
|------------|-----------|---------|-------|
| John | Doe | 123 Main St, City, State 12345 | john.doe@email.com |
| Jane | Smith | 456 Oak Ave, Town, State 67890 | jane.smith@email.com |
| Bob | Johnson | 789 Pine Rd, Village, State 13579 | bob.johnson@email.com |

## Template Document Example

### Input
Your template document (referenced by DOCS_FILE_ID) might contain:

```
Dear {{FIRST_NAME}} {{LAST_NAME}},

We're writing to confirm your address:
{{ADDRESS}}

We will send further correspondence to: {{EMAIL}}

Best regards,
[Your Company Name]
```

### Output
The script will generate individual documents that look like:

```
Dear John Doe,

We're writing to confirm your address:
123 Main St, City, State 12345

We will send further correspondence to: john.doe@email.com

Best regards,
[Your Company Name]
```

## Logger Output Example

When the script runs successfully, you'll see logs like this:
```
Merged letter for John Doe: https://docs.google.com/document/d/[generated-id-1]/edit
Merged letter for Jane Smith: https://docs.google.com/document/d/[generated-id-2]/edit
Merged letter for Bob Johnson: https://docs.google.com/document/d/[generated-id-3]/edit
```

If there are errors, you might see:
```
DOCS_FILE_ID or SHEETS_FILE_ID is missing for row 2
Error accessing SHEETS_FILE_ID 2def...uvw: File not found
```

## File Structure Example

After running the script, your Google Drive will contain:
```
📁 [Original Location]
├── 📄 Template Document
├── 📊 Configuration Spreadsheet
├── 📊 Data Source Spreadsheet
└── 📁 Generated Documents
    ├── 📄 John Doe
    ├── 📄 Jane Smith
    └── 📄 Bob Johnson
```

## Important Notes

1. **File IDs**: Both DOCS_FILE_ID and SHEETS_FILE_ID are found in the URL of their respective Google files:
   - Example Google Doc URL: `https://docs.google.com/document/d/1abc...xyz/edit`
   - The ID would be: `1abc...xyz`

2. **Placeholder Format**: 
   - Must be in ALL CAPS
   - Must be enclosed in double curly braces
   - Must match column headers exactly
   - Example: `{{FIRST_NAME}}`, not `{{First_Name}}` or `{{firstName}}`

3. **Column Headers**:
   - Must match exactly between Config sheet and data source sheets
   - Are case-sensitive
   - Should not contain spaces (use underscores instead)
```

