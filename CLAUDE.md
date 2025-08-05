# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Quarto-based academic course website for "Methods in Linguistics" (MethLing251) at LMU Munich. The course consists of 12 sessions covering research methodology in linguistics, from basic organization to advanced data analysis techniques.

## Common Development Commands

### Building and Rendering
```bash
# Render the entire website
quarto render

# Preview the website locally
quarto preview

# Render specific session
quarto render 01_organisation-introduction/01_organisation-introduction.qmd
```

### Publishing
```bash
# Publish to GitHub Pages (automatic via GitHub Actions)
git push origin main
```

## Architecture and Structure

### Project Type
- **Framework**: Quarto website with RevealJS slides integration
- **Output**: Static HTML website published to GitHub Pages
- **Primary Language**: Markdown (Quarto-flavored)

### Directory Structure
```
/
├── _quarto.yml              # Main Quarto configuration
├── index.qmd                # Homepage with schedule and course info
├── references.bib           # Course-wide bibliography
├── includes/                # Reusable content components
│   ├── _schedule.qmd        # Course schedule table
│   └── _course-description.qmd
├── [01-12]_*/              # Session directories
│   ├── [session].qmd       # Main content (webpage or slides)
│   ├── slides.qmd          # RevealJS slides (if separate)
│   ├── att/                # Session-specific images/assets
│   ├── custom.css          # Session-specific styling
│   └── _extensions/        # Quarto extensions
└── docs/                   # Generated output (GitHub Pages)
```

### Session Organization Patterns
1. **Webpage Format**: Sessions 01-06 use standard Quarto HTML format
2. **Embedded Slides**: Session 07 embeds RevealJS slides in webpage wrapper
3. **Pure Slides**: Sessions 08-12 use RevealJS presentation format

### Content Workflow
Content is converted from Obsidian materials following specific patterns:
- Images: Obsidian `![[att/image.png]]` → Quarto `![alt](att/image.png){width="600"}`
- Links: Obsidian `[[Link|Text]]` → Quarto `[Text](path.qmd)`
- Citations: Use square brackets `[@AuthorYearKeyword]`

## Key Configuration Files

### _quarto.yml
- **Project type**: `website`
- **Navigation**: Dropdown menu for sessions, GitHub link
- **Rendering**: Selective rendering with explicit file lists
- **Theme**: Cosmo with custom CSS

### Bibliography System
- **Global file**: `references.bib` in project root
- **Session reference**: `../references.bib` in session YAML
- **Citation format**: Chicago author-date with `[@AuthorYearKeyword]`
- **BibTeX keys**: Pattern `AuthorYearKeyword` (e.g., `Litosseliti2025Methods`)

## YAML Frontmatter Templates

### Standard Webpage
```yaml
---
title: "[Session Title]"
subtitle: "Methods in linguistics"
author: "Quirin Würschinger, LMU Munich"
date: [YYYY-MM-DD]
date-format: "long"
bibliography: ../references.bib
format:
  html:
    code-overflow: wrap
    css: custom.css
---
```

### RevealJS Slides
```yaml
---
title: "[Session Title]"
subtitle: "Methods in linguistics"
author: "Quirin Würschinger, LMU Munich"
date: [YYYY-MM-DD]
date-format: "long"
format:
  clean-revealjs:
    slide-number: true
    slide-level: 0
css: custom.css
bibliography: ../references.bib
---
```

## Content Standards

### Writing Conventions
- **Headings**: Sentence case (e.g., "Course requirements")
- **Language**: British English spellings
- **Lists**: Must be preceded/followed by empty lines in Quarto
- **Bullet spacing**: Single space after dash (`- item`)
- **Linguistic forms**: Use asterisks (*word*)

### Image Handling
- **Location**: Session `att/` directories
- **Syntax**: `![alt](att/image.png){width="600"}`
- **Source**: Originally from Obsidian vault (`~/obs/proj/MethLing251`)

### Code Wrapping Solution
For comprehensive code wrapping across inline code, source blocks, and output:
1. **YAML**: Add `code-overflow: wrap` under `format: html:`
2. **CSS**: Include custom.css with rules for `p code`, `div.cell-output pre code`, and `pre code`

## Extension System

### Required Extensions per Session
Sessions 07-12 include Quarto extensions in `_extensions/`:
- `clean-revealjs`: Clean theme for presentations
- `excalidraw`: Drawing integration (Session 12)
- `pointer`: Presentation pointer (Session 12)

### Setup for New Sessions
```bash
mkdir XX_session-name
cp -r 07_corpus-linguistics-2/_extensions XX_session-name/
mkdir XX_session-name/att
cp 07_corpus-linguistics-2/custom.css XX_session-name/
```

## Collaboration System

### Private Files
- **Location**: `inf/inf.md` - central collaboration knowledge base
- **Git**: `inf/` directory excluded from version control
- **Purpose**: Track project progress, decisions, troubleshooting notes

### Content Integration Checklist
1. Read source Obsidian file for complete content
2. Create session directory and QMD file with proper YAML
3. Search for and copy all referenced images
4. Convert Obsidian syntax to Quarto markdown
5. Add/verify bibliography entries from global bib file
6. Update `_quarto.yml` render list and navigation
7. Update `includes/_schedule.qmd` with session link
8. Test render for warnings and fix path issues

## GitHub Pages Deployment

- **Workflow**: `.github/workflows/publish.yml` (GitHub Actions)
- **Trigger**: Push to main branch
- **Output**: Automatic deployment to `https://wuqui.github.io/MethLing251/`
- **Branch**: `gh-pages` (automatically managed)

## Troubleshooting Common Issues

### Render Warnings
- **"Unable to resolve link target"**: Remove schedule includes from session directories
- **Missing images**: Search entire Obsidian vault with `find /Users/quirin/obs -name "filename.png"`
- **Path issues**: Ensure relative paths work from session directories

### Bibliography Problems
- Source entries from global `~/promo/bib/references.bib`
- Never modify BibTeX keys when copying entries
- Use `nocite: '@*'` in index to display all references

### Extension Issues
- Copy complete `_extensions/` directory for new sessions
- Verify custom.css exists for styling consistency
- Check RevealJS format compatibility with extensions