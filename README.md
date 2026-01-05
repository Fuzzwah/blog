# Fuzzwah's AI Learning Blog

A place for Fuzzwah to share AI learnings

## About This Blog

This is a GitHub Pages site powered by Jekyll. Blog posts are created by adding markdown files to the `_posts` directory.

## How to Add a New Blog Post

1. Create a new markdown file in the `_posts` directory
2. Name the file using the format: `YYYY-MM-DD-title-of-post.md`
3. Add front matter at the top of the file:

```markdown
---
layout: post
title: "Your Post Title"
date: YYYY-MM-DD HH:MM:SS -0000
author: Fuzzwah
---

Your post content goes here...
```

4. Write your post content using Markdown
5. Commit and push the file to GitHub
6. GitHub Pages will automatically build and publish your post

## Running Locally

To test the site locally before publishing:

1. Install dependencies:
   ```bash
   bundle install
   ```

2. Build and serve the site:
   ```bash
   bundle exec jekyll serve
   ```

3. Open your browser to `http://localhost:4000`

## Site Structure

- `_config.yml` - Jekyll configuration
- `_layouts/` - HTML templates for pages and posts
- `_posts/` - Blog posts in Markdown format
- `assets/css/` - Stylesheets
- `index.html` - Home page that lists all posts

## Markdown Features

Your posts support standard Markdown including:

- Headers (h1-h6)
- **Bold** and *italic* text
- Lists (ordered and unordered)
- Links
- Code blocks (inline and fenced)
- Blockquotes
- And more!

## GitHub Pages

This site is automatically published via GitHub Pages. Any changes pushed to the `main` branch will trigger a GitHub Actions workflow that builds and deploys the site.

The workflow is defined in `.github/workflows/jekyll.yml` and handles:
- Installing Ruby and dependencies
- Building the Jekyll site
- Deploying to GitHub Pages

To enable GitHub Pages for this repository:
1. Go to Settings → Pages
2. Under "Build and deployment", select "GitHub Actions" as the source

