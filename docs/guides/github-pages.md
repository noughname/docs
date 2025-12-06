---
title: GitHub Pages Setup
layout: default
parent: Guides
nav_order: 4
---

# GitHub Pages Setup Guide

Learn how to deploy documentation with GitHub Pages.

{: .fs-6 .fw-300 }

This guide walks you through setting up GitHub Pages for your documentation site, from initial configuration to custom domains.

---

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## What is GitHub Pages?

GitHub Pages is a free static site hosting service that takes files from a GitHub repository and publishes them as a website.

### Benefits

- **Free hosting**: No cost for public repositories
- **HTTPS enabled**: Free SSL certificates
- **CDN powered**: Fast global delivery
- **Easy deployment**: Automatic from Git pushes
- **Custom domains**: Use your own domain name
- **Version controlled**: Full history in Git

### Limitations

- Static sites only (no server-side processing)
- 1 GB repository size limit
- 100 GB bandwidth per month soft limit
- 10 builds per hour

---

## Prerequisites

Before starting, ensure you have:

- A GitHub account
- A repository with your documentation
- Basic understanding of Git
- Jekyll site ready to deploy (like this one!)

---

## Quick Setup

### Step 1: Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** → **Pages**
3. Under "Build and deployment":
   - **Source**: GitHub Actions (recommended)
   - Or select a branch (e.g., `main`)

### Step 2: Configure Repository

Ensure your `_config.yml` has the correct settings:

```yaml
# _config.yml
url: https://username.github.io
baseurl: /repository-name

# For a user/organization site
url: https://username.github.io
baseurl: ""
```

{: .warning }
Replace `username` with your GitHub username and `repository-name` with your repo name!

### Step 3: Push Your Code

```bash
git add .
git commit -m "Setup GitHub Pages"
git push origin main
```

### Step 4: Wait for Build

- Go to **Actions** tab to see build progress
- Wait 1-2 minutes for first deployment
- Visit `https://username.github.io/repository-name/`

---

## GitHub Actions Deployment

### Why Use GitHub Actions?

Modern approach with benefits:
- More control over build process
- Access to full Jekyll environment
- Can use any Jekyll plugins
- Better error messages
- Faster builds

### Workflow File

Create `.github/workflows/jekyll-gh-pages.yml`:

```yaml
name: Deploy Jekyll site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.2'
          bundler-cache: true
      
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v4
      
      - name: Build with Jekyll
        run: bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"
        env:
          JEKYLL_ENV: production
      
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### Understanding the Workflow

**Trigger**: Runs on push to `main` or manual trigger

**Build job**:
1. Checks out code
2. Sets up Ruby and installs dependencies
3. Configures Pages settings
4. Builds Jekyll site
5. Uploads build artifacts

**Deploy job**:
1. Takes build artifacts
2. Deploys to GitHub Pages
3. Provides deployment URL

---

## Configuration Details

### Jekyll Configuration

Essential `_config.yml` settings:

```yaml
# Site settings
title: Your Site Title
description: Site description
url: https://username.github.io
baseurl: /repo-name

# Theme
theme: just-the-docs
# Or use remote_theme for GitHub Pages
remote_theme: just-the-docs/just-the-docs@v0.7.0

# Plugins (must be supported by GitHub Pages)
plugins:
  - jekyll-seo-tag
  - jekyll-sitemap
  - jekyll-feed
  - jekyll-github-metadata

# Exclude files from build
exclude:
  - README.md
  - Gemfile
  - Gemfile.lock
  - node_modules
  - vendor
  - .github
```

### Gemfile Configuration

Your `Gemfile` should include:

```ruby
source "https://rubygems.org"

# GitHub Pages gem (includes Jekyll and approved plugins)
gem "github-pages", "~> 228", group: :jekyll_plugins

# Theme
gem "just-the-docs", "~> 0.7.0"

# Performance booster
gem "listen", "~> 3.8"
```

---

## Types of GitHub Pages Sites

### Project Site

**URL format**: `https://username.github.io/repository-name/`

**Configuration**:
```yaml
baseurl: /repository-name
```

**Use case**: Documentation for a specific project

### User/Organization Site

**Repository name**: `username.github.io`

**URL format**: `https://username.github.io/`

**Configuration**:
```yaml
baseurl: ""
```

**Use case**: Personal portfolio or main organization site

---

## Custom Domains

### Setup Process

#### 1. Add CNAME File

Create a file named `CNAME` in your repository root:

```
docs.example.com
```

{: .note }
No protocol (http/https), no trailing slash!

#### 2. Configure DNS

Add DNS records with your domain registrar:

**For subdomain** (e.g., docs.example.com):
```
Type: CNAME
Name: docs
Value: username.github.io
```

**For apex domain** (e.g., example.com):
```
Type: A
Name: @
Value: 185.199.108.153
Value: 185.199.109.153
Value: 185.199.110.153
Value: 185.199.111.153
```

#### 3. Enable in Settings

1. Go to repository **Settings** → **Pages**
2. Enter your custom domain
3. Check "Enforce HTTPS" (after DNS propagates)
4. Wait for DNS to propagate (5 minutes - 48 hours)

### Verify Custom Domain

```bash
# Check DNS
dig docs.example.com +noall +answer

# Should show:
docs.example.com. 3600 IN CNAME username.github.io.
```

### Update Configuration

Update `_config.yml`:

```yaml
url: https://docs.example.com
baseurl: ""
```

---

## Troubleshooting

### Build Failures

#### Error: "Page build failed"

Check the Actions tab for detailed error messages.

Common causes:
- Syntax error in front matter
- Invalid YAML in `_config.yml`
- Missing dependency in Gemfile
- Broken internal links

**Solution**: Review error message and fix the issue.

#### Error: "Unsupported plugin"

```
Error: jekyll-unsupported-plugin is not supported by GitHub Pages
```

**Solution**: Remove plugin or switch to GitHub Actions workflow.

### Site Not Loading

#### Wrong URL

```
404 - Page Not Found
```

**Solution**: 
- Check `url` and `baseurl` in `_config.yml`
- Ensure trailing slash: `/docs/` not `/docs`
- Clear browser cache

#### CSS/Assets Not Loading

**Symptoms**: Plain HTML with no styling

**Solution**:
```yaml
# _config.yml - ensure baseurl is correct
baseurl: /repository-name
```

Update asset links:
```liquid
{% raw %}<link rel="stylesheet" href="{{ '/assets/css/main.css' | relative_url }}">{% endraw %}
```

### Custom Domain Issues

#### DNS Not Propagating

**Solution**: 
- Wait longer (up to 48 hours)
- Check DNS with: `dig yourdomain.com`
- Verify DNS records at registrar

#### HTTPS Not Available

**Solution**:
- Ensure DNS has propagated
- Uncheck and recheck "Enforce HTTPS"
- Wait up to 24 hours for certificate

#### Mixed Content Warnings

**Solution**: Update all asset URLs to use HTTPS or relative URLs.

---

## Security

### HTTPS

Always enable HTTPS:
- Protects user privacy
- Improves SEO
- Required for modern web features
- Free with GitHub Pages

### Content Security

Best practices:
- Don't commit sensitive data
- Use environment variables for secrets
- Review pull requests carefully
- Enable branch protection

### Access Control

For private documentation:
- Use a private repository (requires GitHub Pro)
- Or use authentication service
- Or self-host instead

---

## Performance Optimization

### Minimize Build Time

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
  - '*.sh'
  - '*.swp'
```

### Optimize Images

```bash
# Resize images before committing
convert input.jpg -resize 1200x output.jpg

# Compress
optipng image.png
jpegoptim image.jpg
```

### Use Incremental Builds (Local)

```bash
bundle exec jekyll serve --incremental
```

{: .warning }
Incremental builds are experimental and may cause issues.

---

## Monitoring

### Check Build Status

Monitor your deployments:
1. Go to **Actions** tab
2. View workflow runs
3. Check logs for errors

### Analytics

Add analytics to track visitors:

#### Google Analytics

```yaml
# _config.yml
google_analytics: UA-XXXXXXXX-X
```

#### Plausible Analytics

Add to custom layout or `_includes/head_custom.html`:

```html
<script defer data-domain="yourdomain.com" 
  src="https://plausible.io/js/script.js"></script>
```

---

## Advanced Topics

### Multiple Environments

#### Production vs Staging

Use branches:
- `main` → production site
- `staging` → staging site

Configure different baseurl per branch:

```yaml
# _config.yml
url: https://username.github.io
baseurl: /docs  # production

# _config_staging.yml
url: https://username.github.io
baseurl: /docs-staging
```

### Automated Deployments

#### Deploy on Tag

```yaml
on:
  push:
    tags:
      - 'v*'
```

#### Deploy on Schedule

```yaml
on:
  schedule:
    - cron: '0 0 * * 0'  # Weekly on Sunday
```

### Preview Deployments

Use pull request workflows:

```yaml
on:
  pull_request:
    branches: ["main"]
```

Deploy to a preview URL for review before merging.

---

## Migration Guide

### From Other Platforms

#### From GitBook

1. Export markdown files
2. Update front matter format
3. Adjust image paths
4. Test internal links

#### From ReadTheDocs

1. Convert reStructuredText to Markdown (if needed)
2. Update configuration
3. Adjust theme-specific syntax
4. Test locally before deploying

#### From Custom Site

1. Extract content to Markdown
2. Set up Jekyll structure
3. Configure navigation
4. Migrate assets

---

## Best Practices

### 1. Test Locally First

Always test before pushing:

```bash
bundle exec jekyll serve
```

### 2. Use Descriptive Commit Messages

```bash
git commit -m "Add deployment guide for GitHub Pages"
```

### 3. Monitor Build Times

Keep builds under 10 minutes:
- Minimize number of files
- Optimize images
- Reduce plugin count

### 4. Version Control Everything

Commit all changes:
- Content files
- Configuration
- Theme customizations
- Assets

### 5. Document Your Setup

Add a README with:
- Setup instructions
- Deployment process
- Troubleshooting tips
- Contribution guidelines

---

## Checklist

Pre-deployment checklist:

- [ ] `_config.yml` configured correctly
- [ ] `url` and `baseurl` set properly
- [ ] Gemfile includes required gems
- [ ] GitHub Actions workflow added
- [ ] All links tested locally
- [ ] Images optimized
- [ ] No sensitive data in repo
- [ ] README.md updated
- [ ] License file included
- [ ] `.gitignore` configured

Post-deployment checklist:

- [ ] Site loads correctly
- [ ] Navigation works
- [ ] Search functional
- [ ] Images display
- [ ] Links work
- [ ] HTTPS enabled
- [ ] Custom domain configured (if applicable)
- [ ] Analytics added (if desired)

---

## Resources

### Official Documentation

- [GitHub Pages Docs](https://docs.github.com/en/pages)
- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [Just the Docs Theme](https://just-the-docs.github.io/just-the-docs/)

### Helpful Tools

- [GitHub Pages Health Check](https://github.com/github/pages-health-check)
- [Jekyll Doctor](https://jekyllrb.com/docs/configuration/options/#build-command-options)
- [DNS Checker](https://dnschecker.org/)

### Community

- [GitHub Community Forum](https://github.community/)
- [Jekyll Talk](https://talk.jekyllrb.com/)

---

## Related Topics

- [Jekyll Tips and Tricks]({% link docs/guides/jekyll-tips.md %})
- [Features Overview]({% link docs/getting-started/features.md %})
- [Working with Markdown]({% link docs/guides/markdown-guide.md %})
