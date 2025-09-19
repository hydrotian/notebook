# CLAUDE.md

## Project Overview

Tian's Notebook - A Jekyll-powered blog about computational hydrology hosted on GitHub Pages using the So Simple theme.

- **URL**: https://simhydro.com/notebook
- **Author**: Tian (hydro.tian@gmail.com, @tianzhou, @hydrotian)

## Project Structure

```
├── _config.yml          # Jekyll configuration
├── _data/               # Jekyll data files
├── _includes/           # Jekyll includes
├── _layouts/            # Jekyll layouts
├── _posts/              # Blog posts (Markdown files)
├── _sass/               # Sass stylesheets
├── assets/              # CSS, JS, and other assets
├── categories/          # Category pages
├── images/              # Image assets
├── interactive/         # Interactive content
├── posts/               # Posts index
├── tags/                # Tag pages
├── Gemfile              # Ruby dependencies (Jekyll, GitHub Pages)
├── index.md             # Homepage
└── readme.md            # Project documentation
```

## Ruby Setup

macOS comes with outdated system Ruby. Use rbenv for proper Ruby management:

```bash
# Install rbenv via Homebrew
brew install rbenv ruby-build

# Install Ruby 3.1.0
rbenv install 3.1.0
rbenv global 3.1.0

# Add to shell profile
echo 'eval "$(rbenv init -)"' >> ~/.zshrc
source ~/.zshrc

# Verify installation
ruby -v
```

## Development Commands

```bash
# Setup (first time or after Ruby changes)
rm Gemfile.lock  # If encountering dependency issues
bundle update

# Local development
bundle exec jekyll serve        # Serve with live reload
bundle exec jekyll serve --drafts  # Include draft posts
bundle exec jekyll build       # Build for production
```

## Content Management

- **Posts**: `_posts/YYYY-MM-DD-title.md` format
- **Drafts**: `_drafts/title.md` (no date prefix)
- **Interactive content**: `interactive/` directory
- **Images**: `images/` directory

## Configuration

- **Theme**: So Simple (mmistakes/so-simple-theme)
- **Markdown**: Kramdown processor
- **Math**: MathJax enabled
- **Comments**: Disqus (shortname: simhydro)
- **Search**: Full content search
- **Features**: Reading time, SEO tags, RSS feed, pagination
