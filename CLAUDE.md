# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static HTML/CSS website for a Canadian immigration law firm (Lawpoint Immigration) with a **typography-first design** inspired by legal documents styled in MS Word. No frameworks, build tools, or package managers — vanilla HTML5, CSS3, and minimal inline JavaScript.

## Running Locally

Open any `.html` file directly in a browser, or serve via HTTP:
```
python -m http.server 8000
```
No build step, no dependencies to install.

## Architecture

### CSS Design System (4 modular files)

All styling flows from CSS custom properties defined in a single source of truth:

- **`css/variables.css`** — Design tokens: colors, typography, spacing (8px base), layout widths, transitions. Every visual value is a custom property.
- **`css/typography.css`** — Legal document text styles mapped to CSS classes, plus base reset and semantic element styling.
- **`css/layout.css`** — Column/full-width modes, serif/sans font toggle, responsive breakpoints, utility classes.
- **`css/components.css`** — Fixed navigation, buttons (primary/secondary/ghost), toggle switches, mobile hamburger menu.

### Typography Classes (core abstraction)

The site's key design decision is a set of text classes that mirror legal document formatting from the reference Word document (`source/Memorandum.docx`):

| Class | Purpose |
|-------|---------|
| `.text-normal` | Standard body text |
| `.text-section` | Bold, uppercase, underlined section headers |
| `.text-subsection` | Indented subheaders |
| `.text-numbered` | Numbered paragraphs (legal style, indented) |
| `.text-offset` | Indented unnumbered paragraphs |
| `.text-reference` | Right-aligned citations |
| `.text-list` | Bullet lists with em-dash (`—`) markers |
| `.text-law` | Law citations (italic, dark blue `#17365D`, deep indent) |
| `.text-quote` | Quotes (italic, maroon `#632423`, deep indent) |

### Two-Mode Toggle System

Both toggles use `classList` manipulation on `<body>`:
- **Font toggle**: `.font-serif` ↔ `.font-sans` — switches `--font-family-body` between Times New Roman and Inter. Mobile forces sans-serif for readability.
- **Width toggle**: `.layout-column` (52rem max) ↔ `.layout-full` — controls content width.

### Indentation Hierarchy

Mirrors MS Word indentation for legal documents:
- `--indent-standard: 2.5rem` (40px) — numbered, offset, list paragraphs
- `--indent-deep: 5rem` (80px) — law citations, quotes

### Responsive Breakpoints

- **Mobile** (≤640px): Forces sans-serif, reduces spacing/indentation
- **Tablet** (641–1024px): Adjusts heading sizes and indentation
- **Desktop** (1025px+): Full experience with all design variables

### Page Structure

All pages share: fixed `<nav class="main-menu">` → `<main class="main-content layout-column">` → `<footer>`. Body has `padding-top: var(--menu-height)` to offset the fixed header.

### HTML Pages

- `index.html` — Home (hero, about, services, contact)
- `about.html` — Team page
- `consultation.html` — Booking page (form backend not implemented)
- `content-template.html` — Template for legal content/services pages
- `notification.html` — 404/success/error messages
- `typography-preview.html`, `components-preview.html` — Development preview pages (not production)

## Naming Conventions

- CSS classes: kebab-case (`.btn-primary`, `.text-section`, `.menu-logo`)
- CSS variables: kebab-case with category prefix (`--color-*`, `--font-*`, `--spacing-*`)
- Files: kebab-case

## Reference Materials

- `source/Memorandum.docx` / `.pdf` — Visual reference for typography styles
- `source/task.txt` — Original project requirements
- `docx_extracted/` — Extracted Word XML for style reference
