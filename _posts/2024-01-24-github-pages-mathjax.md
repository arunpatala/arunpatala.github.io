---
layout: post
title:  "Setting Up a GitHub Pages Compatible Jekyll Blog on Windows 10"
date:   2024-01-24 10:53:18 +0530
categories: AI
---

# Setting Up a GitHub Pages Compatible Jekyll Blog on Windows 10

# GitHub Pages Supported Versions

Before starting your project, make sure you're using the versions of dependencies that are supported by GitHub Pages. The current supported versions are:

- **Jekyll:** 3.9.3
- **Kramdown:** 2.3.2
- **Minima:** 2.5.1
- **Ruby:** 2.7.4

For more details, refer to the [GitHub Pages Dependency Versions](https://pages.github.com/versions/).


This guide provides step-by-step instructions on setting up a Jekyll blog environment on Windows 10 that is compatible with GitHub Pages.

## 1. Install Ruby

GitHub Pages uses Ruby version 2.7.4, so you'll need to install this version.

- **Download Ruby Installer:**
  - Go to [RubyInstaller for Windows](https://rubyinstaller.org/downloads/).
  - Download Ruby+Devkit 2.7.4-1 (x64) for a 64-bit version of Windows or the 32-bit version if applicable.

- **Install Ruby:**
  - Run the installer.
  - Follow the installation prompts and ensure you check “Add Ruby executables to your PATH”.

## 2. Install Specific Versions of Jekyll and Bundler

- **Install Jekyll 3.9.3:**
  ```sh
  gem install jekyll -v 3.9.3
  ```

- **Install Bundler (specify version if required):**
  ```sh
  gem install bundler -v 2.1.4
  ```

## 3. Create a New Jekyll Site

- **Create and navigate to your new site's directory:**
  ```sh
  jekyll new myblog
  cd myblog
  ```

## 4. Configure Jekyll

- **Minimal `_config.yml` File:**
  ```yaml
  title: My Blog
  email: your-email@example.com
  description: "Just another Jekyll site"
  baseurl: "" # the subpath of your site, e.g., "/blog"
  url: "http://example.com" # the base hostname & protocol for your site
  markdown: kramdown
  theme: minima
  ```

- **Gemfile:**
  ```ruby
  source "https://rubygems.org"
  gem "jekyll", "3.9.3"
  gem "minima", "~> 2.5"
  group :jekyll_plugins do
    gem "jekyll-seo-tag"
  end
  gem "kramdown-parser-gfm", "1.1.0"
  ```

- **Install the theme:**
  ```sh
  bundle install
  ```

## 5. Enable MathJax

- **Edit your theme’s default layout (_layouts/default.html).**
- **Add the following in the `<head>` section:**
  ```html
  <script type="text/javascript" async
    src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
  </script>
  ```

## 6. Write Your Blog Post

- **Create a new Markdown file in _posts:**
  - Name it `YYYY-MM-DD-title.md`.
- **Add front matter:**
  ```yaml
  ---
  layout: post
  title: "Your Post Title"
  date: YYYY-MM-DD HH:MM:SS -0000
  categories: category1 category2
  ---
  ```

## 7. Test Locally

- **Run your Jekyll site:**
  ```sh
  bundle exec jekyll serve
  ```
- **View your site at http://localhost:4000.**

## 8. Publish to GitHub Pages

- **Create a GitHub repository and push your Jekyll site files.**
- **Enable GitHub Pages in your repository settings.**

## 9. Updating Your Site

- **Make changes locally and push them to your GitHub repository.**
- **GitHub Pages will automatically rebuild and publish your site.**

## If There is No `head.html` in Your Theme

- **Find and copy the Minima theme's `head.html` from its gem installation directory.**
- **Create your own `head.html` in the `_includes` directory of your project and add the MathJax script.**
- **Build and serve your site to check the changes.**

---

This markdown content is now ready for use in a blog post, ensuring no information is missing and the structure is clear and concise.