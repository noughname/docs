---
title: Troubleshooting
layout: default
nav_order: 5
has_children: true
---

# Troubleshooting

Common issues and their solutions.

{: .fs-6 .fw-300 }

This section helps you resolve common problems you might encounter while working with this knowledge base.

---

## Quick Fixes

### Site Not Building

**Symptom**: GitHub Actions workflow fails or Jekyll build errors

**Solutions**:
1. Check the Actions tab for detailed error messages
2. Verify YAML syntax in `_config.yml`
3. Ensure all internal links use the correct syntax
4. Check for missing front matter in new pages

### Search Not Working

**Symptom**: Search returns no results or doesn't appear

**Solutions**:
1. Ensure `search_enabled: true` in `_config.yml`
2. Clear browser cache and reload
3. Check that JavaScript is enabled
4. Verify pages aren't excluded with `search_exclude: true`

### Navigation Issues

**Symptom**: Pages don't appear in sidebar or appear in wrong order

**Solutions**:
1. Check `nav_order` values (must be numbers)
2. Verify `parent` name matches exactly (case-sensitive)
3. Ensure parent page has `has_children: true`
4. Check for duplicate `nav_order` values

### Broken Links

**Symptom**: Clicking links results in 404 errors

**Solutions**:
1. Use Jekyll link tags: `{% raw %}{% link path/to/file.md %}{% endraw %}`
2. Verify file paths are correct (case-sensitive)
3. Check `baseurl` is correct in `_config.yml`
4. Ensure linked files exist

---

## Build Issues

### Front Matter Errors

**Error**: `YAML Exception reading file`

**Cause**: Invalid YAML syntax in front matter

**Solution**:
```yaml
# ❌ Wrong - missing closing dashes
---
title: My Page
layout: default

# ✅ Correct
---
title: My Page
layout: default
---
```

### Plugin Not Found

**Error**: `Could not find gem 'jekyll-plugin'`

**Solution**:
```bash
# Add to Gemfile
gem "jekyll-plugin", "~> 1.0"

# Install
bundle install

# Rebuild
bundle exec jekyll build
```

### Liquid Syntax Errors

**Error**: `Liquid Exception: Liquid syntax error`

**Common causes**:
```markdown
# ❌ Wrong - unescaped curly braces
Use {{ variable }} like this

# ✅ Correct - escape with raw tags
{% raw %}Use {{ variable }} like this{% endraw %}

# ❌ Wrong - incorrect link syntax
[Link]({% link file.md)

# ✅ Correct
[Link]({% raw %}{% link docs/file.md %}{% endraw %})
```

---

## Local Development Issues

### Bundle Install Fails

**Error**: Permission denied or gem installation errors

**Solutions**:

```bash
# Use bundler path
bundle install --path vendor/bundle

# Or install gems to user directory
gem install bundler --user-install

# Update RubyGems
gem update --system
```

### Port Already in Use

**Error**: `Address already in use - bind(2)`

**Solutions**:

```bash
# Use different port
bundle exec jekyll serve --port 4001

# Find and kill process using port 4000
lsof -ti:4000 | xargs kill -9

# Or on Windows
netstat -ano | findstr :4000
taskkill /PID <process_id> /F
```

### Gemfile.lock Conflicts

**Error**: Bundler version conflicts

**Solutions**:

```bash
# Remove lock file
rm Gemfile.lock

# Reinstall
bundle install

# Update bundler
gem install bundler

# Update specific gem
bundle update jekyll
```

---

## GitHub Pages Issues

### Changes Not Appearing

**Symptom**: Pushed changes don't show on site

**Solutions**:
1. Check Actions tab for build status
2. Wait 1-2 minutes for deployment
3. Clear browser cache (Ctrl+F5 or Cmd+Shift+R)
4. Check if commit triggered workflow

### CSS Not Loading

**Symptom**: Site appears unstyled

**Solutions**:

```yaml
# Verify _config.yml
url: https://username.github.io
baseurl: /repo-name  # Must match repository name

# Use relative_url filter
{% raw %}<link rel="stylesheet" href="{{ '/assets/css/style.css' | relative_url }}">{% endraw %}
```

### Custom Domain Not Working

**Symptom**: Custom domain shows 404 or doesn't resolve

**Solutions**:

1. **Check CNAME file**:
   ```
   docs.example.com
   ```

2. **Verify DNS settings**:
   ```bash
   dig docs.example.com +short
   # Should show: username.github.io
   ```

3. **Wait for DNS propagation**: Can take up to 48 hours

4. **Re-enable custom domain**:
   - Go to Settings → Pages
   - Remove and re-add custom domain
   - Check "Enforce HTTPS"

---

## Content Issues

### Images Not Displaying

**Symptom**: Broken image icons or missing images

**Solutions**:

```markdown
# ❌ Wrong - absolute path
![Image](/images/screenshot.png)

# ✅ Correct - relative to baseurl
{% raw %}![Image]({{ site.baseurl }}/assets/images/screenshot.png){% endraw %}

# ✅ Or relative to current file
![Image](../../assets/images/screenshot.png)

# Check image file exists and path is correct
```

### Code Blocks Not Highlighted

**Symptom**: Code blocks appear plain without colors

**Solutions**:

````markdown
# ❌ Wrong - missing language
```
def hello():
    print("Hello")
```

# ✅ Correct - specify language
```python
def hello():
    print("Hello")
```

# Supported: python, javascript, bash, yaml, etc.
````

### Tables Not Rendering

**Symptom**: Table markdown displays as text

**Solutions**:

```markdown
# ❌ Wrong - missing pipes or misaligned
| Header 1 | Header 2
|----------|----------
| Cell 1 | Cell 2 |

# ✅ Correct
| Header 1 | Header 2 |
|----------|----------|
| Cell 1   | Cell 2   |

# Ensure blank line before table
```

---

## Performance Issues

### Slow Build Times

**Symptom**: Jekyll takes a long time to build

**Solutions**:

```yaml
# _config.yml - exclude unnecessary files
exclude:
  - README.md
  - Gemfile.lock
  - node_modules
  - vendor
  - .git
  - .vscode
  - '*.tmp'

# Use incremental builds (local only)
bundle exec jekyll serve --incremental

# Reduce number of posts/pages if possible
```

### Slow Page Load

**Symptom**: Pages take long to load in browser

**Solutions**:

1. **Optimize images**:
   ```bash
   # Resize large images
   convert large.jpg -resize 1200x output.jpg
   
   # Compress
   optipng image.png
   jpegoptim image.jpg
   ```

2. **Minimize plugins**: Only use necessary plugins

3. **Enable caching**: Browser should cache static assets

---

## Search Issues

### Search Returns No Results

**Symptom**: Typing in search box shows no matches

**Solutions**:

```yaml
# Check _config.yml
search_enabled: true

# Ensure pages aren't excluded
search_exclude: false

# Wait for search index to build
# May take a few seconds on first load
```

### Search Index Not Updating

**Symptom**: New content doesn't appear in search

**Solutions**:

1. Clear browser cache
2. Force rebuild site
3. Check search configuration:

```yaml
search:
  heading_level: 2  # Index h1-h2
  previews: 3
  preview_words_before: 5
  preview_words_after: 10
```

---

## Theme Issues

### Theme Not Applying

**Error**: Site uses default Jekyll theme instead of Just the Docs

**Solutions**:

```yaml
# _config.yml - use remote_theme for GitHub Pages
remote_theme: just-the-docs/just-the-docs@v0.7.0

# NOT theme (which requires gem)
# theme: just-the-docs

# Gemfile
gem "just-the-docs", "~> 0.7.0"
```

### Dark Mode Not Working

**Symptom**: Site shows light theme

**Solution**:

```yaml
# _config.yml
color_scheme: dark

# Rebuild site
bundle exec jekyll build
```

---

## Browser Issues

### Caching Problems

**Symptom**: Old content still showing after updates

**Solutions**:

- **Chrome/Edge**: Ctrl+F5 (Windows) or Cmd+Shift+R (Mac)
- **Firefox**: Ctrl+Shift+R (Windows) or Cmd+Shift+R (Mac)
- **Safari**: Cmd+Option+E then Cmd+R

Or:
1. Open DevTools (F12)
2. Right-click refresh button
3. Select "Empty Cache and Hard Reload"

### JavaScript Errors

**Symptom**: Console shows errors, features not working

**Solutions**:

1. Check browser console (F12)
2. Ensure JavaScript is enabled
3. Try different browser
4. Disable browser extensions
5. Check for conflicting scripts

---

## Git Issues

### Merge Conflicts

**Symptom**: Cannot push or merge due to conflicts

**Solutions**:

```bash
# Update local branch
git fetch origin
git pull origin main

# If conflicts occur
git status  # See conflicting files

# Edit files to resolve conflicts
# Look for markers: <<<<<<<, =======, >>>>>>>

# Mark as resolved
git add resolved-file.md

# Complete merge
git commit -m "Resolve merge conflicts"
git push origin main
```

### Large File Errors

**Error**: `file is too large` or exceeds 100 MB

**Solutions**:

```bash
# Remove large file from git history
git filter-branch --tree-filter 'rm -f large-file.zip' HEAD

# Or use git-lfs for large files
git lfs install
git lfs track "*.zip"
git add .gitattributes
```

---

## Getting Help

If you can't find a solution here:

### 1. Check Documentation

- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [Just the Docs Theme](https://just-the-docs.github.io/just-the-docs/)
- [GitHub Pages Docs](https://docs.github.com/en/pages)

### 2. Search Issues

- [Jekyll GitHub Issues](https://github.com/jekyll/jekyll/issues)
- [Just the Docs Issues](https://github.com/just-the-docs/just-the-docs/issues)

### 3. Community Support

- [Jekyll Talk Forum](https://talk.jekyllrb.com/)
- [GitHub Community](https://github.community/)
- Stack Overflow (tag: jekyll)

### 4. Debug Mode

Enable verbose output:

```bash
# Local development
bundle exec jekyll serve --verbose

# See detailed errors
bundle exec jekyll build --trace
```

---

## Preventive Measures

### Before Making Changes

1. **Test locally first**:
   ```bash
   bundle exec jekyll serve
   ```

2. **Validate YAML**:
   - Check front matter syntax
   - Use YAML validator online

3. **Check links**:
   - Test internal links
   - Verify image paths

4. **Commit frequently**:
   - Small, focused commits
   - Easy to revert if needed

### Regular Maintenance

1. **Update dependencies**:
   ```bash
   bundle update
   ```

2. **Monitor build times**:
   - Check Actions tab
   - Optimize if builds take > 5 minutes

3. **Review analytics**:
   - Check for 404 errors
   - Fix broken links

4. **Backup content**:
   - Git already provides versioning
   - Consider additional backups

---

## Diagnostic Checklist

When troubleshooting, check:

- [ ] Recent changes (what changed before the issue?)
- [ ] Error messages (read them carefully)
- [ ] Browser console (F12, check for errors)
- [ ] GitHub Actions logs (detailed build output)
- [ ] Configuration files (`_config.yml`, `Gemfile`)
- [ ] File permissions and paths
- [ ] Network connectivity
- [ ] DNS settings (for custom domains)
- [ ] Cache (clear browser cache)
- [ ] Dependencies (run `bundle install`)

---

## Related Topics

- [Adding Content]({% link docs/getting-started/adding-content.md %})
- [Jekyll Tips and Tricks]({% link docs/guides/jekyll-tips.md %})
- [GitHub Pages Setup]({% link docs/guides/github-pages.md %})
- [Features Overview]({% link docs/getting-started/features.md %})
