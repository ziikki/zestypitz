+++
date = '2025-10-28T14:37:52-07:00'
draft = true
title = 'How to Deploy a Hugo Site to GitHub Pages'
+++

So you've cloned a Hugo repository. Great! This guide will walk you through the two main phases: running the site on your local machine for development, and then setting up automated deployment to a live server using GitHub Pages.

## 1. Running Your Hugo Site Locally

Before deploying, you'll want to run the site on your own computer. This lets you preview changes in real-time. The repository you cloned is just the source code; you need the Hugo software to build and serve it.

### Install Hugo

You only need to do this once. The easiest way to install Hugo on macOS is with [Homebrew](https://brew.sh/).

**Important**: Be sure to install the `extended` version, as many modern Hugo themes require it for processing stylesheets (Sass/SCSS).

```bash
brew install hugo
```

_For Windows or Linux instructions, refer to the [official Hugo installation guide](https://gohugo.io/installation/)._

### Navigate to Your Repository

Open your terminal and change into the directory where you cloned the repository.

```bash
cd /path/to/your/hugo-site
```

### Initialize the Theme Submodule

Many Hugo themes are included as Git submodules. If the `themes/` directory in your project is empty, you'll need to run this command to pull down the theme's files.

```bash
git submodule update --init --recursive
```

### Run the Local Server

This is the command you'll use most often during development.

```bash
hugo server -D
```

Here's what that does:

- `hugo server` starts Hugo's fast development server.
- The `-D` flag (or `--buildDrafts`) tells Hugo to include posts that are marked as drafts.

Once the server is running, you can view your site by opening a web browser and navigating to `http://localhost:1313/`. The site will automatically refresh whenever you save a change to a file.

## 2. Deploying to GitHub Pages with GitHub Actions

We will use GitHub Actions to create a modern, automated deployment pipeline. When you push your source code (e.g., your Markdown posts) to the `main` branch, the Action will automatically build the static HTML and deploy it to a separate `gh-pages` branch, which will serve your live site.

### Step 1: Create a CNAME File

This is a critical step if you are using a custom domain. Without this file, the GitHub Action will not know which domain to use.

1.  Navigate to the `static/` folder in your Hugo project.
2.  Create a new file named `CNAME` (all uppercase, no file extension).
3.  Inside that file, add **only** your domain name. For example:

```
repitz.com
```

4.  Save the file, then add and commit it to Git. This file ensures that GitHub Pages will use your custom domain.

### Step 2: Create the GitHub Action Workflow

1.  In the root of your repository, create a new directory structure: `.github/workflows/`.
2.  Inside the `workflows` directory, create a new file named `deploy.yml`.
3.  Copy and paste the following code into `deploy.yml`:

```yaml
name: Deploy Hugo Site to Pages

on:
  # Runs on pushes to the main branch
  push:
    branches:
      - main

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

permissions:
  contents: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive # Fetches the theme (if it's a submodule)
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: "latest" # Use the latest Hugo
          extended: true # Use the Extended version

      - name: Build
        run: hugo --minify # Build the site (hugo creates the 'public' folder)

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
          # The CNAME file in your static/ folder will be copied automatically
```

### Step 3: Commit and Push the Workflow

Commit the new workflow file to your repository and push it to the `main` branch.

```bash
git add .github/workflows/deploy.yml
git commit -m "feat: Add GitHub Action for deployment"
git push origin main
```

This initial push will trigger the Action for the first time. You can monitor its progress in the **Actions** tab of your GitHub repository.

### Step 4: Configure Repository's Pages Settings

This is a one-time configuration step.

1.  In your GitHub repository, go to **Settings > Pages**.
2.  Under "Build and deployment," set the **Source** to **Deploy from a branch**.
3.  Under "Branch," select the `gh-pages` branch and the `/(root)` folder.
4.  Click **Save**.

And that's it! Your site is now set up for automated deployment. To publish new content, you just need to `git push` your changes to the `main` branch, and the GitHub Action will handle the rest.
