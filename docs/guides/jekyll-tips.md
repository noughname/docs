---
title: Jekyll Tips and Tricks
layout: default
parent: Guides
nav_order: 3
---

# Jekyll Tips and Tricks

Essential tips for working with Jekyll and this knowledge base.

{: .fs-6 .fw-300 }

Learn how to leverage Jekyll's powerful features to create better documentation.

---

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Understanding Jekyll

### What is Jekyll?

Jekyll is a static site generator that transforms plain text files into a complete website:

- **Input**: Markdown files + templates + configuration
- **Process**: Jekyll builds the site
- **Output**: Static HTML files ready to deploy

### Benefits

- **Fast**: Static files load instantly
- **Secure**: No database or server-side code to exploit
- **Version controlled**: Everything is in Git
- **Easy deployment**: Works perfectly with GitHub Pages
- **No server needed**: Host anywhere that serves static files

---

## File Structure

### Project Organization

```
docs/
├── _config.yml          # Site configuration
├── index.md             # Homepage
├── docs/                # Documentation pages
│   ├── getting-started/
│   ├── guides/
│   └── reference/
├── _sass/               # Custom styles (optional)
├── assets/              # Images, CSS, JS (optional)
├── Gemfile              # Ruby dependencies
└── .github/
    └── workflows/       # GitHub Actions
```

### Understanding Paths

- **Root path**: `/docs/` (defined by `baseurl` in `_config.yml`)
- **Page URLs**: Generated from file paths
- **Permalink**: Can be customized in front matter

Example:
```
File: docs/guides/my-guide.md
URL:  /docs/guides/my-guide.html
```

---

## Front Matter Deep Dive

### Required Fields

Every page needs at minimum:

```yaml
---
title: Page Title
layout: default
---
```

### All Available Fields

```yaml
---
title: Complete Example
layout: default
nav_order: 10
parent: Parent Page
grand_parent: Grandparent Page
has_children: true
permalink: /custom-url/
nav_exclude: false
search_exclude: false
description: Page description for SEO
---
```

### Field Descriptions

| Field | Purpose | Example |
|-------|---------|---------|
| `title` | Page title in navigation and header | `"Installation Guide"` |
| `layout` | Template to use | `default`, `home` |
| `nav_order` | Position in navigation (ascending) | `1`, `2`, `3` |
| `parent` | Parent page title (exact match) | `"Getting Started"` |
| `grand_parent` | Grandparent (for 3-level hierarchy) | `"Documentation"` |
| `has_children` | Mark as parent page | `true`, `false` |
| `permalink` | Custom URL path | `/docs/custom/` |
| `nav_exclude` | Hide from navigation | `true`, `false` |
| `search_exclude` | Exclude from search | `true`, `false` |

---

## Working with Links

### Safe Internal Links

Always use Jekyll's `link` tag for internal links:

```markdown
{% raw %}[Link text]({% link docs/guides/my-guide.md %}){% endraw %}
```

**Benefits:**
- Build fails if link is broken (catches errors early)
- Automatically handles path changes
- Works with any permalink configuration

### Asset Links

For images and files:

```markdown
{% raw %}![Image]({{ site.baseurl }}/assets/images/screenshot.png){% endraw %}
```

Or use relative paths from the page:

```markdown
![Image](../../assets/images/screenshot.png)
```

### External Links

External links work normally:

```markdown
[GitHub](https://github.com)
```

---

## Custom Variables

### Site Variables

Access configuration from `_config.yml`:

```liquid
{% raw %}{{ site.title }}
{{ site.url }}
{{ site.baseurl }}
{{ site.description }}{% endraw %}
```

### Page Variables

Access current page information:

```liquid
{% raw %}{{ page.title }}
{{ page.url }}
{{ page.path }}
{{ page.date }}{% endraw %}
```

### Custom Variables

Add custom variables in front matter:

```yaml
---
title: My Page
custom_field: custom value
author: John Doe
version: 1.2.3
---
```

Use in content:

```liquid
{% raw %}Author: {{ page.author }}
Version: {{ page.version }}{% endraw %}
```

---

## Includes and Reusable Content

### Create an Include

Create a file in `_includes/` directory:

```markdown
<!-- _includes/note.md -->
{: .note }
{{ include.content }}
```

### Use the Include

```liquid
{% raw %}{% include note.md content="This is a reusable note!" %}{% endraw %}
```

### Common Use Cases

- Warning messages
- Code snippets
- Contact information
- Footer content
- Navigation elements

---

## Collections

### What are Collections?

Collections group related content beyond regular pages:

```yaml
# _config.yml
collections:
  guides:
    output: true
    permalink: /guides/:name/
  tutorials:
    output: true
    permalink: /tutorials/:name/
```

### Using Collections

Create files in `_guides/` or `_tutorials/`:

```yaml
---
title: My Tutorial
collection: tutorials
---

Content here...
```

### When to Use Collections

- API documentation (one page per endpoint)
- Team members (one page per person)
- Products or services
- Blog posts or news items

---

## Liquid Templating

### Conditionals

```liquid
{% raw %}{% if page.author %}
  Author: {{ page.author }}
{% endif %}

{% if page.version >= 2.0 %}
  This is a new version!
{% endif %}{% endraw %}
```

### Loops

```liquid
{% raw %}{% for guide in site.guides %}
  - [{{ guide.title }}]({{ guide.url }})
{% endfor %}{% endraw %}
```

### Filters

```liquid
{% raw %}{{ page.title | upcase }}
{{ page.content | number_of_words }}
{{ page.date | date: "%Y-%m-%d" }}
{{ page.url | relative_url }}{% endraw %}
```

Common filters:
- `upcase`, `downcase`, `capitalize`
- `strip`, `strip_html`
- `number_of_words`, `truncate`
- `date`, `date_to_string`
- `sort`, `reverse`, `uniq`

---

## Search Configuration

### Search Settings

Configure search in `_config.yml`:

```yaml
search_enabled: true
search:
  heading_level: 2        # Index h1-h2 headings
  previews: 3             # Show 3 preview matches
  preview_words_before: 5 # Context before match
  preview_words_after: 10 # Context after match
  tokenizer_separator: /[\s/]+/
  rel_url: true
  button: false           # Don't show search button
```

### Exclude from Search

Exclude specific pages:

```yaml
---
title: Private Notes
search_exclude: true
---
```

### Improve Search Results

Tips for better search:
1. Use descriptive headings
2. Include relevant keywords naturally
3. Add synonyms for important terms
4. Use clear, concise language

---

## Performance Tips

### 1. Minimize Plugins

Only include plugins you actually need:

```ruby
# Gemfile
group :jekyll_plugins do
  gem "jekyll-seo-tag"
  gem "jekyll-sitemap"
  # Comment out unused plugins
end
```

### 2. Optimize Images

- Use appropriate formats (WebP, JPEG, PNG)
- Compress images before adding
- Use reasonable dimensions
- Consider lazy loading for many images

### 3. Reduce Build Time

- Keep number of pages reasonable (< 1000)
- Minimize complex Liquid logic
- Use pagination for large collections
- Cache dependencies in CI/CD

### 4. Efficient Front Matter

Only include fields you actually use:

```yaml
# Minimal
---
title: My Page
parent: Guides
---
```

---

## Testing Locally

### Install Dependencies

```bash
bundle install
```

### Run Development Server

```bash
bundle exec jekyll serve
```

Options:
- `--livereload`: Auto-refresh browser on changes
- `--drafts`: Include draft posts
- `--incremental`: Faster rebuilds (experimental)
- `--port 4000`: Change port number

### Preview URL

Open http://localhost:4000/docs/ in your browser.

{: .note }
The trailing slash is important! The baseurl is `/docs`.

---

## Troubleshooting

### Build Failures

#### Error: Page Not Found

```
Liquid Exception: Could not find document 'docs/page.md'
```

**Solution:**
- Check file path is correct
- Ensure file exists
- Verify spelling and capitalization

#### Error: Invalid Front Matter

```
Error: YAML Exception reading file
```

**Solution:**
- Check front matter syntax
- Ensure closing `---`
- Verify YAML formatting

#### Error: Plugin Not Found

```
Error: Could not find 'jekyll-plugin-name'
```

**Solution:**
```bash
bundle install
```

### Local vs Production Differences

**Different baseurl:**
- Local: http://localhost:4000/docs/
- GitHub Pages: https://username.github.io/docs/

**Solution:**
Always use `{% raw %}{{ site.baseurl }}{% endraw %}` for paths.

---

## GitHub Pages Specific

### Supported Plugins

GitHub Pages only supports certain plugins:
- jekyll-seo-tag ✅
- jekyll-sitemap ✅
- jekyll-feed ✅
- jekyll-github-metadata ✅
- And a few others

See [GitHub Pages dependencies](https://pages.github.com/versions/) for the full list.

### Custom Domain

To use a custom domain:

1. Add a `CNAME` file to repository root:
   ```
   docs.example.com
   ```

2. Configure DNS:
   ```
   docs.example.com.  CNAME  username.github.io.
   ```

3. Enable HTTPS in repository settings

---

## Advanced Configuration

### Exclude Files from Build

```yaml
# _config.yml
exclude:
  - README.md
  - Gemfile
  - Gemfile.lock
  - node_modules
  - vendor
  - .vscode
  - .idea
```

### Custom 404 Page

Create `404.md`:

```markdown
---
layout: default
title: Page Not Found
permalink: /404.html
---

# Page Not Found

The page you're looking for doesn't exist.
```

### Redirects

For changed URLs, add a redirect page:

```markdown
---
layout: default
title: Old Page
redirect_to: /docs/new-page/
---

<meta http-equiv="refresh" content="0; url={{ page.redirect_to }}">
```

---

## Best Practices

### 1. Consistent Naming

Use lowercase with hyphens:
- ✅ `my-guide.md`
- ❌ `MyGuide.md`
- ❌ `my_guide.md`

### 2. Logical Organization

Group related pages:
```
docs/
├── getting-started/
│   ├── index.md
│   ├── installation.md
│   └── quick-start.md
└── guides/
    ├── index.md
    └── advanced.md
```

### 3. Clear Navigation Order

Use nav_order strategically:
- Homepage: `nav_order: 1`
- Getting Started: `nav_order: 2`
- Guides: `nav_order: 3`
- Reference: `nav_order: 4`

### 4. Meaningful Permalinks

Customize permalinks for cleaner URLs:

```yaml
---
permalink: /start/
---
```

### 5. Test Before Pushing

Always test locally:
```bash
bundle exec jekyll serve
```

---

## Quick Reference

### Common Commands

```bash
# Install dependencies
bundle install

# Serve locally
bundle exec jekyll serve

# Serve with live reload
bundle exec jekyll serve --livereload

# Build only
bundle exec jekyll build

# Clean build artifacts
bundle exec jekyll clean
```

### Common Files

```
_config.yml       # Main configuration
Gemfile           # Dependencies
index.md          # Homepage
404.md            # Error page
_includes/        # Reusable components
_layouts/         # Page templates
_sass/            # Custom styles
assets/           # Static files
```

---

## Additional Resources

- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [Liquid Documentation](https://shopify.github.io/liquid/)
- [Just the Docs Theme](https://just-the-docs.github.io/just-the-docs/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)

---

## Related Topics

- [Adding Content]({% link docs/getting-started/adding-content.md %})
- [Working with Markdown]({% link docs/guides/markdown-guide.md %})
- [Features Overview]({% link docs/getting-started/features.md %})
