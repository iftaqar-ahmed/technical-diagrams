# Commit and Push Diagram Changes

Quick workflow to commit and push diagram and PDF changes to GitHub.

## Instructions

1. **Check git status** to see what files changed
2. **Show summary** of changed files (drawio and pdf files)
3. **Stage all changes** in exports/ folder
4. **Commit** with descriptive message based on changes:
   - If only PDFs changed: "Re-export diagrams to PDF"
   - If both .drawio and .pdf changed: "Update diagrams and re-export PDFs"
   - If new files added: "Add [diagram names] with PDF exports"
5. **Push to GitHub**

## Commit Message Template

```
Re-export diagrams to PDF with latest changes

Updated:
- [List of PDF files with sizes]

Export settings: --all-pages --crop --border 20

🤖 Generated with Claude Code (https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

## Notes

- Only commit files in exports/ folder
- Show file sizes in commit message
- Always push immediately after commit
