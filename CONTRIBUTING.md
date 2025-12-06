# Contributing to the Knowledge Base

Thank you for your interest in contributing to this knowledge base! This document provides guidelines for adding and organizing content.

## Getting Started

### Local Development Setup

1. **Prerequisites**:
   - Ruby 2.7 or higher
   - Bundler gem

2. **Clone and Setup**:
   ```bash
   git clone https://github.com/noughname/docs.git
   cd docs
   bundle install
   ```

3. **Run Locally**:
   ```bash
   bundle exec jekyll serve
   ```
   Then open http://localhost:4000/docs/ in your browser.

## Adding Content

### Creating a New Page

1. Create a new Markdown file in the appropriate directory:
   - `docs/getting-started/` - For introductory content
   - `docs/guides/` - For step-by-step tutorials
   - `docs/reference/` - For reference documentation

2. Add front matter at the top of the file:
   ```markdown
   ---
   title: Your Page Title
   layout: default
   parent: Parent Page Name (if applicable)
   nav_order: 1
   ---
   
   # Your Page Title
   
   Your content here...
   ```

### Front Matter Fields

| Field | Description | Required | Example |
|-------|-------------|----------|---------|
| `title` | Page title | Yes | `title: Getting Started` |
| `layout` | Page layout | Yes | `layout: default` |
| `nav_order` | Order in navigation | No | `nav_order: 1` |
| `parent` | Parent page title | No | `parent: Guides` |
| `has_children` | Has child pages | No | `has_children: true` |
| `grand_parent` | Grandparent page | No | `grand_parent: Guides` |

### Navigation Structure

#### Creating a Section (Parent Page)

```markdown
---
title: My Section
layout: default
nav_order: 2
has_children: true
---

# My Section

Section overview...
```

#### Creating a Child Page

```markdown
---
title: My Page
layout: default
parent: My Section
nav_order: 1
---

# My Page

Page content...
```

## Content Guidelines

### Writing Style

- Use clear, concise language
- Write in second person ("you") for guides
- Use present tense
- Keep sentences and paragraphs short

### Code Blocks

Use fenced code blocks with language identifiers:

````markdown
```python
def hello_world():
    print("Hello, World!")
```
````

### Headings

- Use `#` for page title (h1)
- Use `##` for main sections (h2)
- Use `###` for subsections (h3)
- Don't skip heading levels

### Links

#### Internal Links
```markdown
[Link Text]({% link path/to/file.md %})
```

#### External Links
```markdown
[Link Text](https://example.com)
```

### Images

1. Place images in `assets/images/`
2. Reference them in Markdown:
   ```markdown
   ![Alt text](/docs/assets/images/filename.png)
   ```

### Tables

```markdown
| Header 1 | Header 2 |
|----------|----------|
| Cell 1   | Cell 2   |
```

### Callouts

Use callouts to highlight important information:

```markdown
{: .note }
This is a note.

{: .warning }
This is a warning.

{: .highlight }
This is highlighted information.
```

## Best Practices

### Organization

- Group related content together
- Use descriptive file names (lowercase with hyphens)
- Keep the directory structure simple
- Limit nesting to 3 levels

### File Naming

- Use lowercase letters
- Separate words with hyphens
- Use `.md` extension
- Make names descriptive

Examples:
- ✅ `getting-started.md`
- ✅ `python-basics.md`
- ❌ `GettingStarted.md`
- ❌ `page1.md`

### Content Quality

- Verify all code examples work
- Test all links
- Spell check your content
- Keep content up to date

## Markdown Features

### GitHub Flavored Markdown

This site supports GitHub Flavored Markdown, including:

- ✅ Task lists
  ```markdown
  - [x] Completed task
  - [ ] Pending task
  ```

- ✅ Tables
- ✅ Strikethrough: `~~text~~`
- ✅ Automatic URL linking
- ✅ Emoji: `:smile:` → 😊

### Syntax Highlighting

Supported languages include:
- Python
- JavaScript
- Ruby
- Go
- Java
- C/C++
- Bash/Shell
- YAML
- JSON
- Markdown
- And many more...

## Testing Your Changes

### Before Committing

1. **Build the site locally**:
   ```bash
   bundle exec jekyll build
   ```

2. **Check for errors** in the output

3. **Preview your changes**:
   ```bash
   bundle exec jekyll serve
   ```

4. **Test**:
   - Navigation works correctly
   - Links are not broken
   - Images display properly
   - Code examples are formatted correctly
   - Search finds your content

## Deployment

### Automatic Deployment

When changes are pushed to the `main` branch:
1. GitHub Actions automatically triggers
2. Jekyll builds the site
3. The site is deployed to GitHub Pages
4. Changes are live at https://noughname.github.io/docs/

### Manual Testing

To test the deployment locally:
```bash
bundle exec jekyll build --baseurl "/docs"
```

## Getting Help

If you encounter issues:

1. Check the [Jekyll documentation](https://jekyllrb.com/docs/)
2. Review [Just the Docs theme documentation](https://just-the-docs.github.io/just-the-docs/)
3. Look at existing pages for examples
4. Check the build logs in GitHub Actions

## Questions?

For questions or suggestions, please open an issue on GitHub.

Thank you for contributing!
