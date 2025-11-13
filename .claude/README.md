# Claude Code Automation - Technical Diagrams

This folder contains Claude Code configuration files and slash commands for automating diagram workflows.

## Available Slash Commands

### `/export-diagrams`
**Purpose:** Export all .drawio files to PDF with standardized settings

**What it does:**
1. Finds all .drawio files in `exports/` folder
2. Exports each to PDF using: `--all-pages --crop --border 20`
3. Shows export summary with file sizes
4. Asks if you want to commit and push changes

**When to use:**
- After making changes to .drawio files
- To regenerate all PDFs with consistent settings
- When adding new diagram files

**Example:**
```
User: "I made some changes to the diagrams"
Assistant: /export-diagrams
```

---

### `/commit-diagrams`
**Purpose:** Quick commit and push workflow for diagram changes

**What it does:**
1. Checks git status for changed files
2. Shows summary of changes
3. Stages all changes in exports/
4. Commits with descriptive message
5. Pushes to GitHub

**When to use:**
- After exporting diagrams to PDF
- To quickly commit diagram updates
- When you want a standardized commit message

**Example:**
```
User: "Push these changes to GitHub"
Assistant: /commit-diagrams
```

---

## Workflow Example

**Typical workflow when updating diagrams:**

1. User edits .drawio files manually in Draw.io
2. User: "Re-export again as I made some changes to them"
3. Type: `/export-diagrams`
4. Claude exports all diagrams to PDF
5. Claude asks: "Would you like to commit and push?"
6. User: "Yes"
7. Claude commits and pushes to GitHub

**Alternative (manual steps):**
1. User: "Re-export the diagrams"
2. Type: `/export-diagrams`
3. User: "Commit them"
4. Type: `/commit-diagrams`

---

## Export Settings Reference

**Standard PDF export settings:**
```bash
"C:/Program Files/draw.io/draw.io.exe" \
  --export \
  --format pdf \
  --all-pages \
  --crop \
  --border 20 \
  --output "FILENAME.pdf" \
  "FILENAME.drawio"
```

**What each setting does:**
- `--all-pages`: Export all diagram tabs (Matrix View, TreeView, etc.)
- `--crop`: Fit PDF page to diagram size (removes excess whitespace)
- `--border 20`: Add 20px margin around content (prevents cutoff)

---

## Manual Export (Draw.io UI)

If exporting manually from Draw.io application:

1. **File → Export as → PDF**
2. **Settings:**
   - PDF: **All Pages** ✓
   - Page View: **Crop** ✓
   - Border Width: **20**
   - Zoom: **100%**
   - Grid: Unchecked
   - Shadows: Unchecked
   - Transparent Background: Unchecked
3. Click **Export**

---

## Files in This Folder

```
.claude/
├── README.md                    # This file
└── commands/
    ├── export-diagrams.md       # /export-diagrams slash command
    └── commit-diagrams.md       # /commit-diagrams slash command
```

---

## Adding New Slash Commands

To create a new slash command:

1. Create `commands/your-command-name.md`
2. Write instructions for Claude Code to follow
3. Use in chat by typing `/your-command-name`

**Template:**
```markdown
# Command Title

Brief description of what this command does.

## Instructions

1. Step 1
2. Step 2
3. Step 3

## Notes

- Important detail 1
- Important detail 2
```

---

## Tips

**For faster workflows:**
- Combine commands: "Export diagrams and commit"
- Use shorthand: "Export and push"
- Reference by action: "Re-export the PDFs"

**File naming conventions:**
- Use `*Final.drawio` for production diagrams
- Archive old versions to `_ARCHIVE_YYYY-MM-DD/`
- Use descriptive names: `LNE_Actual_Model_Sync_Diagrams_Final.drawio`

---

Last Updated: 2025-01-12
