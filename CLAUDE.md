# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Site Overview

This is a Jekyll-based GitHub Pages blog 

## Development Commands

### Local Development
```bash
# Install dependencies
bundle install

# Serve the site locally with live reload and draft posts
bundle exec jekyll serve --drafts --livereload

# Serve without drafts
bundle exec jekyll serve

# Build the site
bundle exec jekyll build
```

### Jekyll Configuration
- **Theme**: Minimal Mistakes (v4.27.3) with "neon" skin
- **Base URL**: `` (empty for GitHub Pages root domain)
- **Site URL**: `https://jbeight-devops.github.io`
- **Pagination**: 5 posts per page
- **Plugins**: jekyll-paginate, jekyll-sitemap, jekyll-gist, jekyll-feed, jekyll-include-cache

## Architecture and Structure

### Content Organization
- **Posts**: `_posts/` - Blog posts following `YEAR-MONTH-DAY-title.markdown` format
- **Pages**: `_pages/` - Static pages (about, categories, tags, post archive)
- **Layouts**: `_layouts/` - Custom layout templates
- **Data**: `_data/navigation.yml` - Site navigation configuration
- **Site**: `_site/` - Generated static site (excluded from git)

### Key Configuration Files
- `_config.yml` - Main Jekyll configuration with theme settings, author info, and site metadata
- `Gemfile` - Ruby dependencies including Jekyll ~4.3.4 and jekyll-feed
- `README.md` - Contains the primary development command: `bundle exec jekyll serve --drafts --livereload`

### Navigation Structure
The site uses a main navigation bar with:
- Home (/)
- Posts (/posts/)
- Categories (/categories/)
- Tags (/tags/)
- About (/about/)

### Theme Features
- Author profile with bio: 
- Post categorization and tagging system
- Liquid-based category and tag archives
- Reading time estimates
- Social sharing capabilities (currently disabled)
- Responsive design with neon color scheme

### Content Defaults
All posts use:
- Layout: single
- Author profile: enabled
- Read time: enabled
- Share buttons: enabled
- Related posts: enabled
- Comments: disabled

## Development Notes

- The site uses Jekyll 4.3.4 with Ruby gems managed via Bundler
- Live reload is available during development for immediate preview of changes
- Draft posts can be previewed using the `--drafts` flag
- The theme is remotely sourced from mmistakes/minimal-mistakes
- HTML compression is enabled for production builds