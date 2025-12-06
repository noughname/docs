# Knowledge Base

Personal documentation and knowledge base built with Jekyll and hosted on GitHub Pages.

## Features

- 🌙 Dark theme inspired by VS Code
- 🔍 Full-text search functionality
- 📚 Tree-view navigation
- 💻 Syntax highlighting for code blocks
- 📝 GitHub Flavored Markdown support

## Local Development

### Prerequisites

- Ruby 2.7 or higher
- Bundler

### Setup

1. Install dependencies:
   ```bash
   bundle install
   ```

2. Run the development server:
   ```bash
   bundle exec jekyll serve
   ```

3. Open your browser to `http://localhost:4000/docs/`

### Building

To build the site:
```bash
bundle exec jekyll build
```

The site will be generated in the `_site` directory.

## Repository Structure

```
.
├── _config.yml          # Jekyll configuration
├── Gemfile              # Ruby dependencies
├── index.md             # Homepage
├── docs/                # Documentation files
│   ├── getting-started/ # Getting started guides
│   ├── guides/          # How-to guides
│   └── reference/       # Reference documentation
├── _guides/             # Collection for guides (optional)
├── _reference/          # Collection for reference docs (optional)
└── assets/              # Static assets (images, CSS, JS)
```

## Adding Content

### Creating a New Page

Create a new Markdown file in the appropriate directory:

```markdown
---
title: Page Title
layout: default
nav_order: 1
parent: Parent Page (optional)
---

# Page Title

Your content here...
```

### Navigation Order

Use the `nav_order` front matter to control the order of pages in the navigation:

```yaml
---
nav_order: 1
---
```

### Creating Sections

Create parent pages and child pages:

**Parent page:**
```yaml
---
title: Section Title
has_children: true
nav_order: 2
---
```

**Child page:**
```yaml
---
title: Child Page
parent: Section Title
nav_order: 1
---
```

## Deployment

The site is automatically deployed to GitHub Pages via GitHub Actions when changes are pushed to the main branch.

## Theme

This site uses the [Just the Docs](https://just-the-docs.github.io/just-the-docs/) theme, which provides:
- Responsive design
- Built-in search
- Dark mode
- Syntax highlighting
- And much more!

## License

This knowledge base is for personal use.
