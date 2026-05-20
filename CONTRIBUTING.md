# Contributing

Thank you for your interest in contributing to the Subnautica 2 Modding documentation! This guide covers everything you need to know to contribute effectively.

## Ways to contribute

- **Suggest content** - [open an issue](../../issues) to propose a new guide, flag inaccurate information, or suggest improvements
- **Submit changes** - all content changes are made via pull requests - please do not push directly to `main`!

## Getting started

### Prerequisites

- [Ruby](https://www.ruby-lang.org/en/downloads/) installed locally
- [Bundler](https://bundler.io/) (`gem install bundler`)
- A Markdown editor (see [Authoring Tools](#authoring-tools) below)

### First-time setup

Clone the repository and install dependencies. Commands must be run from the repository root folder:

```
bundle install
```

This only needs to be done once.

### Previewing locally

To build and serve the site locally:

```
bundle exec jekyll serve
```

The site will be available at `http://127.0.0.1:4000`. The server will watch for changes and rebuild automatically. Hit `Ctrl+C` twice to stop.

## Authoring tools

Any text editor will work for editing Markdown files. The following dedicated Markdown editors are recommended:

- **[Typora](https://typora.io/)** - a really nice WYSIWYG Markdown editor, used to create this file and the Beginners Guide (paid, one-time licence).
- **[MarkText](https://www.marktext.cc/)** - a capable open source alternative to Typora.

## Repository structure

```
└── 📁Subnautica2Modding.github.io
    └── 📁_sass
        └── 📁custom			<- Custom layout
    └── 📁.github
        └── 📁workflows			<- GitHub Actions deployment workflows (do not modify!)
    └── 📁assets
        └── 📁images			<- Site wide images, logo etc
    └── 📁beginners-guide
    └── 📁scripts				<- Helper scripts and tools for local development
    ├── _config.yml				<- Site configuration
    ├── Gemfile					<- Gem dependencies
    ├── Gemfile.lock			<- Locked Gem dependencies
```

## Writing pages

### Front matter

Every page must include front matter at the top of the file. Use the appropriate template below:

**Top-level section page:**

```yaml
---
layout: default
title: Your Section Title
nav_order: 1
has_children: true
---
```

**Child page:**

```yaml
---
layout: default
title: Your Page Title
parent: Your Section Title
nav_order: 1
---
```

**Grandchild page:**

```yaml
---
layout: default
title: Your Page Title
parent: Your Parent Page Title
grand_parent: Your Section Title
nav_order: 1
---
```

### Navigation order (`nav_order`)

- Every page must have a `nav_order` value.
- Use sequential integers starting from `1`, scoped within each section.
- Do not duplicate `nav_order` values within the same section - duplicates will cause unpredictable ordering.
- When inserting a new page between existing pages, renumber as needed to keep the sequence clean.

### Folder structure

- Each top-level section should have its own folder at the repository root.
- Grandchild pages should be in a subfolder named after the parent page.
- Have a look at the beginners-guide folder for an example.

### Images

- Store images used by a section's pages in an `images/` subfolder within that section's folder.
- Reference images using relative paths: `![Alt text](images/your-image.png).`
- Store site-wide images (logo, favicon, etc.) in `assets/images/` at the repository root.
- Use descriptive filenames in lowercase with hyphens, e.g. `mod-setup-screen.png`.

## Gem and dependency management

- **Do not modify `Gemfile`** unless you have a specific, agreed reason to add or change a dependency.
- **Do not run `bundle update`** as part of routine work - only run it deliberately when a gem update is needed, and always commit the resulting .`Gemfile.lock`.
- If you delete `Gemfile.lock` to resolve a dependency issue, run `bundle install` to regenerate it and commit the result.

## Pull requests

- Keep pull requests focused - one topic or fix per PR where possible.
- Include a clear description of what you've changed and why.
- Ensure the site builds and renders correctly locally before submitting.
