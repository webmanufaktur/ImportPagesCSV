# ImportPagesCSV: Import CSV file to pages

This is a ProcessWire module that enables you to import CSV files to create pages
or modify existing pages. The module requires ProcessWire 3.0.123 or newer. 
This is an admin/development tool that is recommended only for use by the 
superuser or developer. 

Version 1.14 adds support for per-row parents, safer duplicate handling, and stricter
validation during imports.

The following Fieldtypes are supported for importing, as well as most types 
derived from them: 

- Checkbox
- Datetime
- Email
- File
- Float
- Image
- Integer
- Options
- Page
- PageTitle
- Text
- Textarea
- URL

## To install:

1. Place the file ImportPagesCSV.module in a /site/modules/ImportPagesCSV/ directory. 
2. In ProcessWire admin, click to 'Modules' and 'Check for new modules'. 
3. Click 'install next to the 'Import Pages CSV' module (under heading 'Import'). 

Once installed, the module can be found on your admin Setup menu under the title "Import 
Pages CSV". 

## Importing file/image fields

CSV column should contain full URL (or diskpath and filename) to the file you want to import. 
For fields that support multiple files, place each filename or URL on its own line, OR separate 
them by | (pipe) OR tab.

## Importing page reference fields

For single-value page fields the CSV imported value can be the page id, path, title, or name.
For multi-value page fields, the value can be the same but multiple-values should be separated by 
either a newline in the column, or a pipe "|" character. Please make sure that your Page reference 
field has one or more pages selected for the "Parent" setting on the Details tab. If you want the 
import to be able to create paes, there must also be a single Template selected on the "Template" 
setting. 

## Recent enhancements (v1.14)

### Custom identifier field selection
- Choose any CSV column as the identifier used to find existing pages, rather than being limited to titles.
- Match rows against legacy IDs, custom fields, or other unique values to avoid creating duplicates.
- Step 2 now requires choosing an identifier so the importer can either modify a matched page or create a new one when no match is found.

**How to use**
1. In Step 2, pick `Identifier Column` and choose the CSV column that contains the values you want to match.
2. The module sanitizes the values and searches for existing pages whose titles match the selected column.
3. When a page is found it is modified (based on the duplicate handling option); otherwise a new page is created.

### Dynamic parent page selection
- Each CSV row can supply its own parent page via a `parent_id` column.
- Rows without a `parent_id` fallback to the default parent chosen in Step 1.
- Parent IDs are validated for existence and edit permissions before importing.

**How to use**
1. In Step 2, pick `Parent ID Column` and choose the CSV column that contains parent page IDs.
2. Supply integer page IDs in the CSV; rows with valid IDs use those parents, others use the default.

### Improved title field workflow
- Identifier values are validated before lookups run, so empty identifiers stop the row with a helpful error.
- Page names are generated from title values after all CSV data is assigned, keeping the title-to-name flow consistent.
- Existing pages retain their names; only new pages must resolve to a unique name.

### PHP 8.2+ compatibility and hardening
- Dropped the deprecated `auto_detect_line_endings` usage and added null-coalescing operators where needed.
- Eliminated `strlen(null)` warnings and ensured all string operations account for nullable CSV data.
- Retained backward compatibility while keeping imports quiet on modern PHP versions.


---
Copyright 2011-2021 by Ryan Cramer for ProcessWire
