---
title: Adding Content
layout: default
parent: Getting Started
nav_order: 1
---

# Adding Content

Learn how to add new pages and organize content in this knowledge base.

## Creating a New Page

To create a new page, simply add a Markdown file in the appropriate directory:

```markdown
---
title: My New Page
layout: default
nav_order: 1
---

# My New Page

Your content here...
```

## Front Matter

Every page should include front matter at the top:

| Field | Description | Required |
|-------|-------------|----------|
| `title` | Page title displayed in navigation | Yes |
| `layout` | Layout to use (usually `default`) | Yes |
| `nav_order` | Order in navigation (lower numbers first) | No |
| `parent` | Parent page title for hierarchical nav | No |
| `has_children` | Set to `true` if page has child pages | No |

## Navigation Hierarchy

### Parent Pages

Create a parent page that will contain child pages:

```yaml
---
title: Section Name
has_children: true
nav_order: 2
---
```

### Child Pages

Link a page to its parent:

```yaml
---
title: Child Page Name
parent: Section Name
nav_order: 1
---
```

### Grandchild Pages

You can nest pages up to 3 levels:

```yaml
---
title: Grandchild Page
parent: Child Page Name
grand_parent: Section Name
nav_order: 1
---
```

## Code Blocks

Use fenced code blocks with syntax highlighting:

````markdown
```python
def hello_world():
    print("Hello, World!")
```
````

Supported languages include: Python, JavaScript, Ruby, Go, Java, C++, Bash, YAML, JSON, and many more.

## Markdown Features

### GitHub Flavored Markdown

This knowledge base supports GitHub Flavored Markdown (GFM), including:

- [x] Task lists
- Tables
- Strikethrough with `~~text~~`
- Automatic URL linking
- Emoji :smile:

### Tables

```markdown
| Header 1 | Header 2 |
|----------|----------|
| Cell 1   | Cell 2   |
```

### Callouts

Use callouts to highlight important information:

{: .note }
This is a note callout.

{: .warning }
This is a warning callout.

{: .highlight }
This is a highlighted callout.

## Tips

- Keep filenames lowercase with hyphens (e.g., `my-page.md`)
- Use descriptive titles
- Organize related pages under parent sections
- Use `nav_order` to control the sequence
- Include code examples where appropriate
