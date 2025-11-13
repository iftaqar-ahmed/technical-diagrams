# Export Diagrams to PDF

Export specific .drawio files in the exports/ folder to PDF format with standardized settings.

## Instructions

1. **List all .drawio files** in `exports/` folder (exclude `_ARCHIVE_*/` and `_OLD/`)
2. **Ask user which file(s)** to export:
   - "Which diagram would you like to export to PDF?"
   - Show numbered list of available .drawio files
   - User can specify: filename, number, or "all"
3. **Export selected file(s) to PDF** using Draw.io CLI with these settings:
   - `--all-pages` (export all diagram tabs)
   - `--crop` (fit PDF to diagram size)
   - `--border 20` (20px margin around content)
4. **Show export summary** with file sizes
5. **Ask user** if they want to commit and push to GitHub
6. **If yes**, commit with message: "Re-export diagrams to PDF with latest changes"

## Export Command Template

```bash
"C:/Program Files/draw.io/draw.io.exe" --export --format pdf --all-pages --crop --border 20 --output "FILE.pdf" "FILE.drawio"
```

## Examples

**Example 1 - Single file by name:**
```
User: /export-diagrams
Assistant: "Which diagram would you like to export?"
User: "LNE_Actual_Model_Sync_Diagrams_Final"
Assistant: [Exports that file]
```

**Example 2 - By number:**
```
Assistant: "Available diagrams:
1. LNE_Actual_Model_Sync_Diagrams_Final.drawio
2. LNE_Plan_Model_Sync_Diagrams_Final.drawio"
User: "1"
Assistant: [Exports diagram #1]
```

**Example 3 - Multiple files:**
```
User: "1 and 2" or "all"
Assistant: [Exports all specified files]
```

## Notes

- Accept filename with or without .drawio extension
- Display progress for each file being exported
- Show any errors encountered during export
- File paths are relative to exports/ folder
