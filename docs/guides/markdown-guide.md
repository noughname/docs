---
title: Working with Markdown
layout: default
parent: Guides
nav_order: 2
---

# Working with Markdown

Master Markdown to create beautiful documentation with ease.

{: .fs-6 .fw-300 }

This comprehensive guide covers everything you need to know about writing documentation in Markdown for this knowledge base.

---

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Basic Syntax

### Headings

Create headings using hash symbols:

```markdown
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6
```

{: .note }
Headings automatically generate anchor links for easy navigation and sharing.

### Emphasis

Make text stand out:

```markdown
*Italic text* or _italic text_
**Bold text** or __bold text__
***Bold and italic*** or ___bold and italic___
~~Strikethrough text~~
```

Result:
- *Italic text*
- **Bold text**
- ***Bold and italic***
- ~~Strikethrough text~~

### Paragraphs and Line Breaks

Separate paragraphs with a blank line:

```markdown
This is the first paragraph.

This is the second paragraph.
```

For a line break without starting a new paragraph, end a line with two spaces:

```markdown
First line  
Second line
```

---

## Lists

### Unordered Lists

Create bullet lists with `-`, `*`, or `+`:

```markdown
- Item 1
- Item 2
  - Nested item 2.1
  - Nested item 2.2
- Item 3
```

Result:
- Item 1
- Item 2
  - Nested item 2.1
  - Nested item 2.2
- Item 3

### Ordered Lists

Create numbered lists:

```markdown
1. First item
2. Second item
   1. Nested item 2.1
   2. Nested item 2.2
3. Third item
```

Result:
1. First item
2. Second item
   1. Nested item 2.1
   2. Nested item 2.2
3. Third item

### Task Lists

Create interactive checkboxes:

```markdown
- [x] Completed task
- [ ] Incomplete task
- [ ] Another task
```

Result:
- [x] Completed task
- [ ] Incomplete task
- [ ] Another task

---

## Links

### Basic Links

```markdown
[Link text](https://example.com)
[Link with title](https://example.com "Link title")
```

### Internal Links

Link to other pages in the knowledge base:

```markdown
[Getting Started]({% raw %}{% link docs/getting-started/index.md %}{% endraw %})
```

{: .highlight }
Using Jekyll's `link` tag validates links at build time and prevents broken links.

### Reference Links

Keep your markdown clean with reference-style links:

```markdown
[Link text][reference]

[reference]: https://example.com
```

### Anchor Links

Link to sections within a page:

```markdown
[Jump to Basic Syntax](#basic-syntax)
```

---

## Images

### Basic Image Syntax

```markdown
![Alt text](path/to/image.png)
![Alt text with title](path/to/image.png "Image title")
```

### Image with Link

Make images clickable:

```markdown
[![Alt text](path/to/image.png)](https://example.com)
```

---

## Code

### Inline Code

Use backticks for inline code:

```markdown
Use the `print()` function to output text.
```

Result: Use the `print()` function to output text.

### Code Blocks

Create code blocks with triple backticks and specify the language for syntax highlighting:

````markdown
```python
def hello_world():
    print("Hello, World!")
```
````

Supported languages include:
- `python`, `javascript`, `typescript`, `java`, `c`, `cpp`, `csharp`
- `ruby`, `go`, `rust`, `php`, `swift`, `kotlin`
- `bash`, `shell`, `powershell`
- `yaml`, `json`, `xml`, `toml`, `ini`
- `html`, `css`, `scss`, `markdown`
- `sql`, `dockerfile`, `makefile`
- And many more!

### Code Block with Filename

Add a filename comment at the top:

````markdown
```python
# filename: hello.py
def hello_world():
    print("Hello, World!")
```
````

---

## Tables

Create tables using pipes and hyphens:

```markdown
| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Row 1    | Data     | More data |
| Row 2    | Data     | More data |
```

Result:

| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Row 1    | Data     | More data |
| Row 2    | Data     | More data |

### Table Alignment

Control column alignment:

```markdown
| Left | Center | Right |
|:-----|:------:|------:|
| Text | Text   | Text  |
```

Result:

| Left | Center | Right |
|:-----|:------:|------:|
| Text | Text   | Text  |

---

## Blockquotes

Create blockquotes with `>`:

```markdown
> This is a blockquote.
> It can span multiple lines.
>
> And multiple paragraphs.
```

Result:

> This is a blockquote.
> It can span multiple lines.
>
> And multiple paragraphs.

### Nested Blockquotes

```markdown
> First level
>> Second level
>>> Third level
```

Result:

> First level
>> Second level
>>> Third level

---

## Horizontal Rules

Create horizontal lines:

```markdown
---
***
___
```

Result:

---

## Callouts (Just the Docs)

Use special callouts to highlight information:

### Note Callout

```markdown
{: .note }
This is a note with important information.
```

{: .note }
This is a note with important information.

### Warning Callout

```markdown
{: .warning }
This is a warning - be careful!
```

{: .warning }
This is a warning - be careful!

### Highlight Callout

```markdown
{: .highlight }
This is highlighted text to draw attention.
```

{: .highlight }
This is highlighted text to draw attention.

---

## Front Matter

Every page must include YAML front matter at the top:

```yaml
---
title: Page Title
layout: default
nav_order: 1
parent: Parent Page (if applicable)
has_children: true/false
---
```

### Common Front Matter Fields

| Field | Description | Required |
|-------|-------------|----------|
| `title` | Page title | Yes |
| `layout` | Layout template (usually `default`) | Yes |
| `nav_order` | Navigation order (lower first) | No |
| `parent` | Parent page title | No |
| `grand_parent` | Grandparent page title (for 3rd level) | No |
| `has_children` | Set to `true` if page has children | No |
| `permalink` | Custom URL path | No |
| `nav_exclude` | Exclude from navigation | No |

---

## Advanced Formatting

### Definition Lists

```markdown
Term 1
: Definition 1

Term 2
: Definition 2a
: Definition 2b
```

### Footnotes

```markdown
Here's a sentence with a footnote[^1].

[^1]: This is the footnote content.
```

### Abbreviations

```markdown
The HTML specification is maintained by the W3C.

*[HTML]: Hyper Text Markup Language
*[W3C]: World Wide Web Consortium
```

### Escaping Characters

Use backslash to escape special characters:

```markdown
\* Not a bullet
\# Not a heading
\[Not a link\]
```

---

## Typography Classes (Just the Docs)

### Font Size

```markdown
{: .fs-1 }  # Smallest
{: .fs-2 }
{: .fs-3 }
{: .fs-4 }
{: .fs-5 }  # Default
{: .fs-6 }
{: .fs-7 }
{: .fs-8 }
{: .fs-9 }  # Largest
```

### Font Weight

```markdown
{: .fw-300 }  # Light
{: .fw-400 }  # Normal
{: .fw-500 }  # Medium
{: .fw-700 }  # Bold
```

### Text Color

```markdown
{: .text-grey-dk-000 }
{: .text-grey-dk-100 }
{: .text-grey-dk-200 }
{: .text-grey-dk-300 }
{: .text-blue-000 }
{: .text-blue-100 }
{: .text-blue-200 }
{: .text-blue-300 }
```

### Text Alignment

```markdown
{: .text-left }
{: .text-center }
{: .text-right }
```

---

## Best Practices

### 1. Use Descriptive Headings

❌ Bad:
```markdown
## Step 1
## Step 2
```

✅ Good:
```markdown
## Install Dependencies
## Configure the Application
```

### 2. Keep Lines Short

Aim for 80-100 characters per line for better readability in version control.

### 3. Use Consistent Formatting

Stick to one style for lists, emphasis, and code blocks throughout your documentation.

### 4. Add Blank Lines

Use blank lines to separate sections and improve readability:

```markdown
## Heading

Paragraph 1 with content.

Paragraph 2 with more content.

```

### 5. Include Code Examples

Always provide working code examples that users can copy and run.

### 6. Link Related Content

Help users discover related topics by adding links to relevant pages.

---

## Quick Reference

### Most Common Syntax

```markdown
# Heading

**Bold** and *italic*

- List item
- Another item

1. Numbered item
2. Another numbered item

[Link](url)

`inline code`

```language
code block
```

| Table | Header |
|-------|--------|
| Data  | More   |

> Blockquote

{: .note }
Callout box
```

---

## Practice Exercise

Try creating a page with:

1. A descriptive title and front matter
2. At least three heading levels
3. Both ordered and unordered lists
4. A code block with syntax highlighting
5. A table with at least 3 columns
6. Links to other pages
7. A callout box

---

## Additional Resources

- [GitHub Flavored Markdown Spec](https://github.github.com/gfm/)
- [Just the Docs Documentation](https://just-the-docs.github.io/just-the-docs/)
- [Kramdown Syntax](https://kramdown.gettalong.org/syntax.html)
- [Markdown Guide](https://www.markdownguide.org/)

---

## Related Topics

- [Adding Content]({% link docs/getting-started/adding-content.md %})
- [Features Overview]({% link docs/getting-started/features.md %})
- [Sample Guide]({% link docs/guides/sample-guide.md %})
