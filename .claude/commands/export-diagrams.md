# Export All Diagrams to PDF

Export all .drawio files in the exports/ folder to PDF format with standardized settings.

## Instructions

1. **Find all .drawio files** in `exports/` folder
2. **Export each file to PDF** using Draw.io CLI with these settings:
   - `--all-pages` (export all diagram tabs)
   - `--crop` (fit PDF to diagram size)
   - `--border 20` (20px margin around content)
3. **Show export summary** with file sizes
4. **Ask user** if they want to commit and push to GitHub
5. **If yes**, commit with message: "Re-export diagrams to PDF with latest changes"

## Export Command Template

```bash
"C:/Program Files/draw.io/draw.io.exe" --export --format pdf --all-pages --crop --border 20 --output "FILE.pdf" "FILE.drawio"
```

## Notes

- Only export files matching pattern: `*Final.drawio` or `*.drawio` (ask user which pattern)
- Skip files in `_ARCHIVE_*/` and `_OLD/` folders
- Display progress for each file being exported
- Show any errors encountered during export
