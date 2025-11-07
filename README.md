# Technical Diagrams

A public repository for technical diagrams, workflows, and visual documentation. This repository serves as a centralized location for shareable diagrams and architecture visualizations.

## Repository Purpose

- **Public Access**: All diagrams are publicly accessible for easy sharing and collaboration
- **Draw.io Compatible**: All diagrams use `.drawio` format for easy editing
- **Organized Structure**: Categorized folders for different diagram types
- **Version Controlled**: Track changes and evolution of diagrams over time

## Structure

```
technical-diagrams/
├── workflows/        # Process flows, dataflows, and workflow diagrams
├── architecture/     # System architecture, component diagrams
├── processes/        # Business processes, decision trees
└── README.md
```

## Available Diagrams

### Workflows

- **[claude_code_dataflow.drawio](workflows/claude_code_dataflow.drawio)** - Claude Code request processing dataflow
  - Shows 3-stage BR creation workflow
  - Tool selection decision tree
  - Task management flow

## How to Use

### Viewing Diagrams

**Option 1: Draw.io Web (Recommended)**
1. Go to https://app.diagrams.net/
2. File → Open from → GitHub
3. Select: `iftaqar-ahmed/technical-diagrams`
4. Choose your diagram

**Option 2: Direct Download**
1. Click on any `.drawio` file in this repository
2. Download and open with:
   - Draw.io Desktop app
   - Draw.io VS Code extension
   - Online at https://app.diagrams.net/

**Option 3: VS Code Extension**
1. Install "Draw.io Integration" extension
2. Clone this repository
3. Open `.drawio` files directly in VS Code

### Editing Diagrams

1. **Fork this repository** (or clone if you have write access)
2. Open diagram in Draw.io (web, desktop, or VS Code)
3. Make your changes
4. Save and commit
5. Create pull request (if forked)

### Adding New Diagrams

1. Create your diagram in Draw.io
2. Save as `.drawio` format
3. Place in appropriate folder:
   - `workflows/` - Process and data flows
   - `architecture/` - System designs
   - `processes/` - Business processes
4. Update this README with diagram description
5. Commit and push

## Tools

### Draw.io (diagrams.net)
- **Web**: https://app.diagrams.net/
- **Desktop**: https://github.com/jgraph/drawio-desktop/releases
- **VS Code**: Search "Draw.io Integration" in extensions

### Features
- Free and open source
- No account required
- GitHub integration
- Export to PNG, SVG, PDF
- Collaborative editing

## Diagram Standards

### File Naming
- Use lowercase with underscores: `my_diagram_name.drawio`
- Be descriptive: `user_authentication_flow.drawio` (not `auth.drawio`)
- Include version if needed: `api_v2_architecture.drawio`

### Best Practices
- Use consistent colors for similar components
- Include a legend if using color coding
- Add title and date to diagram
- Use clear, readable fonts (12pt minimum)
- Export PNG/SVG for README previews

## Contributing

This is a personal repository, but contributions are welcome:

1. Fork the repository
2. Create your diagram
3. Follow naming conventions
4. Update README
5. Submit pull request

## License

MIT License - Feel free to use these diagrams for reference, learning, or adaptation.

## Related Repositories

- **myOSlibrary** (Private) - OneStream business rules and development resources

## Contact

For questions or suggestions, open an issue in this repository.

---

**Last Updated**: 2025-11-07
**Maintainer**: Iftaqar Ahmed
