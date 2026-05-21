# Contributing

Thank you for your interest in contributing to the Subnautica 2 Modding documentation! This guide covers everything you need to know to contribute effectively.

## Ways to contribute

- **Suggest content** - [open an issue](../../issues) to propose a new guide, flag inaccurate information, or suggest improvements
- **Submit changes** - all content changes are made via pull requests - please do not push directly to `main`!

## Getting started

### Prerequisites

The documentation site is built using [MkDocs](https://www.mkdocs.org/) with the [Material theme](https://squidfunk.github.io/mkdocs-material/).

To build and test the site locally, you will need:

- [Python](https://www.python.org/downloads/) installed locally
- A Markdown editor (see [Authoring Tools](#authoring-tools) below)

### First-time setup

Clone the repository and install dependencies. Commands must be run from the repository root folder:

```
pip install mkdocs
pip install mkdocs-material
```

This only needs to be done once.

### Previewing locally

To build and serve the site locally:

```
python -m mkdocs serve
```

The site will be available at `http://127.0.0.1:8000/`. The server will watch for changes and rebuild automatically. Hit `Ctrl+C` to stop.

## Authoring tools

Any text editor will work for editing Markdown files. The following dedicated Markdown editors are recommended:

- **[Typora](https://typora.io/)** - a really nice WYSIWYG Markdown editor, used to create this file and the Beginners Guide (paid, one-time licence).
- **[MarkText](https://www.marktext.cc/)** - a capable open source alternative to Typora.

## Repository structure

```
└── 📁Subnautica2Modding.github.io
    └── 📁.github
        └── 📁workflows			<- Custom workflow for deploying MkDocs
    └── 📁docs					<- All docs go in here
        └── 📁assets
            └── 📁images		<- Site wide images, logos, favicon
        └── 📁beginners-guide	<- Folder for each document/topic
            └── 📁images		<- images folder in every folder/subfolder
    ├── mkdocs.yml				<- MkDocs and Material config
```

## Writing pages

### Site Navigation

- All pages must be explicitly referenced in `mkdocs.yml`.
- Order in the navigation panel is explicitly defined by the order in the `nav` settings.

### Folder structure

- Each top-level section should have its own folder at the `docs` folder in the repository root.
- Every folder and sub-folder should have an `index.md`
- Create a new folder in the root of `docs` for new guides.
- Have a look at the beginners-guide folder for an example.

### Images

- Store images used by a section's pages in an `images/` subfolder within that section's folder.
- Reference images using relative paths and must use Linux paths.
- Store site-wide images (logo, favicon, etc.) in `docs/assets/images/` at the repository root.
- Use descriptive folder and filenames in lowercase with hyphens, e.g. `mod-setup-screen.png`.

## Pull requests

- Keep pull requests focused - one topic or fix per PR where possible.
- Include a clear description of what you've changed and why.
- Ensure the site builds and renders correctly locally before submitting.
