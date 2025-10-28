+++
date = '2025-10-28T14:36:59-07:00'
draft = true
title = 'Initialize Hugo Repot'
+++

## How to Initialize Hugo Repo

This guide documents the steps to set up a Hugo site with the PaperMod theme from scratch.

### Prerequisites

- macOS (or Linux/Windows with appropriate package manager)
- Git installed
- Terminal/Command line access

### Step 1: Install Hugo

On macOS, install Hugo using Homebrew:

```bash
brew install hugo
```

Verify the installation:

```bash
hugo version
```

### Step 2: Initialize Hugo Site

Navigate to your project directory and initialize a new Hugo site:

```bash
cd /path/to/your/project
hugo new site . --force
```

The `--force` flag allows Hugo to initialize in a non-empty directory (if you already have files like README.md or .git).

### Step 3: Add PaperMod Theme

Add the PaperMod theme as a git submodule:

```bash
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

The `--depth=1` flag creates a shallow clone to save space and time.

### Step 4: Configure Hugo

Update the `hugo.toml` configuration file with your site settings:

```toml
baseURL = 'https://solareda.com/'
languageCode = 'en-us'
title = 'Zesty Pitz'
theme = 'PaperMod'

[params]
  env = "production"
  description = "Your site description"
  author = "Your Name"

  # Theme features
  ShowReadingTime = true
  ShowShareButtons = true
  ShowPostNavLinks = true
  ShowBreadCrumbs = true
  ShowCodeCopyButtons = true
  ShowWordCount = true

[params.homeInfoParams]
  Title = "Welcome to Your Site"
  Content = "A modern, fast, and responsive Hugo blog"

# Add menu items, social icons, etc.
```

### Step 5: Create Content Structure

Create the necessary directories for your content:

```bash
mkdir -p content/posts
```

### Step 6: Run Development Server

Start the Hugo development server to preview your site:

```bash
hugo server -D
```

Visit `http://localhost:1313` in your browser to see your site.

### Step 7: Create Your First Post

Create a new blog post:

```bash
hugo new posts/my-first-post.md
```

Edit the file in `content/posts/my-first-post.md` and set `draft: false` when ready to publish.

### Step 8: Build for Production

When ready to deploy, build the static site:

```bash
hugo
```

This generates your site in the `public/` directory.

## Useful Commands

- `hugo server -D` - Start development server with draft posts
- `hugo new posts/post-name.md` - Create a new post
- `hugo` - Build the site for production
- `git submodule update --remote --merge` - Update the PaperMod theme

## Resources

- [Hugo Documentation](https://gohugo.io/documentation/)
- [PaperMod Theme Documentation](https://github.com/adityatelange/hugo-PaperMod/wiki)
- [PaperMod Demo Site](https://adityatelange.github.io/hugo-PaperMod/)
