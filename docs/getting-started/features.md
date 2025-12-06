---
title: Features Overview
layout: default
parent: Getting Started
nav_order: 3
---

# Features Overview

Discover all the powerful features that make this knowledge base easy to use and maintain.

{: .fs-6 .fw-300 }

---

## 🔍 Full-Text Search

### Instant Search Results

The built-in search engine indexes all content and provides instant results as you type:

- Searches through **all page titles, headings, and content**
- **Highlights matching terms** in preview snippets
- **Keyboard accessible** with `Ctrl/Cmd + K` or `/`
- **No page refresh** required - results appear instantly

### Smart Search Features

- **Fuzzy matching**: Find content even with typos
- **Context previews**: See surrounding text for each match
- **Result ranking**: Most relevant results appear first
- **Multiple keywords**: Combine terms to narrow results

{: .note }
The search index is automatically updated when new content is added.

---

## 🌙 Dark Theme

### VS Code-Inspired Design

The dark theme is carefully designed for long reading sessions:

- **Low eye strain**: Optimized color contrast
- **Syntax highlighting**: Color schemes that work well in dark mode
- **Consistent styling**: All elements designed for dark background
- **Code-friendly**: Syntax colors inspired by popular dark themes

### Benefits

- Reduces eye fatigue during extended reading
- Makes code blocks more readable
- Provides professional, modern appearance
- Consistent with popular development tools

---

## 📚 Hierarchical Navigation

### Tree-View Sidebar

The navigation sidebar provides a clear overview of all content:

- **Collapsible sections**: Expand/collapse parent items
- **Visual hierarchy**: Indentation shows relationships
- **Active page highlighting**: Always know where you are
- **Ordered navigation**: Control order with `nav_order` front matter

### Navigation Levels

Supports up to three levels of hierarchy:

```
Parent Page
├── Child Page 1
│   ├── Grandchild Page 1.1
│   └── Grandchild Page 1.2
└── Child Page 2
    └── Grandchild Page 2.1
```

---

## 💻 Syntax Highlighting

### Multi-Language Support

Beautiful syntax highlighting for dozens of programming languages:

**Python Example:**
```python
def fibonacci(n):
    """Generate Fibonacci sequence."""
    a, b = 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b

# Usage
for num in fibonacci(10):
    print(num)
```

**JavaScript Example:**
```javascript
// Async/await example
async function fetchData(url) {
  try {
    const response = await fetch(url);
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error:', error);
    throw error;
  }
}
```

**Bash Example:**
```bash
#!/bin/bash
# Script to backup files
SOURCE="/path/to/source"
DEST="/path/to/backup"
DATE=$(date +%Y-%m-%d)

tar -czf "$DEST/backup-$DATE.tar.gz" "$SOURCE"
echo "Backup completed: backup-$DATE.tar.gz"
```

### Supported Languages

- Python, JavaScript, TypeScript, Ruby, Go
- Java, C++, C#, PHP, Swift, Kotlin
- Bash, PowerShell, Shell
- YAML, JSON, XML, TOML
- SQL, HTML, CSS, SCSS
- Markdown, Dockerfile, Makefile
- And many more!

---

## 📝 GitHub Flavored Markdown

### Full GFM Support

This knowledge base supports all GitHub Flavored Markdown features:

#### Task Lists

Track progress with interactive checkboxes:

- [x] Set up repository
- [x] Configure GitHub Pages
- [x] Add sample documentation
- [ ] Add more guides
- [ ] Add API documentation

#### Tables

Create formatted tables easily:

| Feature | Status | Priority |
|---------|--------|----------|
| Search | ✅ Complete | High |
| Dark Theme | ✅ Complete | High |
| Mobile Support | ✅ Complete | Medium |
| PDF Export | ⏳ Planned | Low |

#### Strikethrough

~~Old information~~ Updated information

#### Automatic URL Linking

Links are automatically created: https://github.com/noughname/docs

#### Emoji Support

Add personality with emoji: 🚀 💡 ⚡ 🎉 📚

---

## 🔗 Smart Linking

### Internal Cross-References

Easily link between pages with Jekyll's linking system:

```markdown
[Link text]({% raw %}{% link docs/guides/sample-guide.md %}{% endraw %})
```

Benefits:
- **Build-time validation**: Broken links cause build errors
- **Automatic path resolution**: No manual URL management
- **Refactoring safe**: Update file paths without breaking links

### Anchor Links

Link to specific sections within pages:

```markdown
[Jump to section](#features-overview)
```

All headings automatically get anchor links for easy sharing.

---

## 📱 Responsive Design

### Mobile-Friendly

The site automatically adapts to different screen sizes:

- **Mobile navigation**: Hamburger menu on small screens
- **Touch-friendly**: Large tap targets for mobile
- **Readable text**: Optimized font sizes for all devices
- **Fluid layout**: Content reflows naturally

### Desktop Enhancements

On larger screens, enjoy additional features:

- **Sticky navigation**: Sidebar stays visible while scrolling
- **Table of contents**: Right sidebar with page outline
- **Wider content area**: More text per line for comfortable reading

---

## ⚡ Performance

### Fast Loading

The site is optimized for speed:

- **Static generation**: Pre-built HTML for instant loading
- **Minimal JavaScript**: Only essential features require JS
- **Optimized assets**: Compressed CSS and images
- **CDN delivery**: Served via GitHub's fast infrastructure

### Efficient Search

- **Client-side search**: No server requests needed
- **Indexed content**: Search index built at compile time
- **Instant results**: No waiting for API responses

---

## 🔄 Version Control Integration

### Git-Based Workflow

All documentation is stored in Git:

- **Change tracking**: Full history of all modifications
- **Collaboration**: Multiple contributors can work together
- **Review process**: Pull requests for quality control
- **Rollback capability**: Easily revert problematic changes

### Automatic Deployment

GitHub Actions automatically deploys changes:

1. Push to `main` branch
2. GitHub Actions builds the site
3. Site deploys to GitHub Pages
4. Changes are live in minutes

---

## 🎨 Customizable

### Theme Customization

Customize the appearance with configuration:

```yaml
# _config.yml
color_scheme: dark
search_enabled: true
heading_anchors: true
back_to_top: true
```

### Flexible Structure

Organize content however you prefer:

- Create custom collections
- Define your own navigation structure
- Add custom pages and layouts
- Extend with plugins

---

## 📋 Additional Features

### Callout Boxes

Highlight important information:

{: .note }
This is a note callout - perfect for additional information.

{: .warning }
This is a warning callout - use for cautions and important notices.

{: .highlight }
This is a highlight callout - emphasize key points.

### Code Block Features

Enhanced code blocks with:
- Syntax highlighting
- Language labels
- Copy-to-clipboard button
- Line wrapping
- Preserved formatting

### Navigation Enhancements

- Breadcrumb navigation
- Previous/Next page links
- Back to top button
- Active page indicator
- Collapsible sections

### SEO Optimization

Built-in SEO features:
- Meta tags generation
- Sitemap creation
- Semantic HTML
- Open Graph tags
- Structured data

---

## Next Steps

Ready to start using these features?

- [Quick Start Guide]({% link docs/getting-started/quick-start.md %})
- [Add Your Own Content]({% link docs/getting-started/adding-content.md %})
- [Browse Example Guides]({% link docs/guides/index.md %})
