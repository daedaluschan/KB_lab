# AI Instruction Library

A reusable Markdown instruction library for AI prompts, agents, coding standards, and workflow playbooks. The library is published as a MkDocs Material site and deployed to GitHub Pages at <https://daedaluschan.github.io/KB_lab/>.

## What this repository contains

This repository is a documentation-first project. Source content lives in `docs/`, while `mkdocs.yml` controls the published site configuration, theme, plugins, navigation behavior, and Markdown extensions.

```text
.
├── docs/
│   ├── index.md
│   ├── tags.md
│   ├── instructions/
│   │   ├── index.md
│   │   └── reusable-instruction-template.md
│   └── assets/
│       └── images/
├── .github/
│   └── workflows/
│       └── pages.yml
├── mkdocs.yml
├── requirements.txt
└── README.md
```

## Use cases

Use this library to collect and publish reusable guidance such as:

- Prompt patterns for consistent AI output.
- Agent operating procedures for repeatable autonomous workflows.
- Coding standards for teams, languages, frameworks, or repositories.
- Workflow playbooks for planning, implementation, review, and release.
- Templates that make new instructions easier to write and maintain.

## Local development

### Prerequisites

- Python 3.12 or compatible Python 3 version.
- `pip` for installing MkDocs dependencies.

### Install dependencies

```bash
python -m pip install -r requirements.txt
```

### Preview the site locally

```bash
mkdocs serve
```

MkDocs serves the site locally and reloads as Markdown files change.

### Build the site

```bash
mkdocs build --strict
```

Strict builds are preferred because they catch warnings that could break deployment or produce incomplete documentation.

## Authoring guidance

- Add reusable instruction content under `docs/`.
- Start new instruction pages from `docs/instructions/reusable-instruction-template.md` when possible.
- Use YAML front matter tags to improve discoverability through the generated tags page.
- Keep `docs/tags.md` with the inline `<!-- material/tags -->` marker so the Material tags plugin can render the tag index.
- Use the MkDocs Material features already enabled in `mkdocs.yml`, including admonitions, collapsible details, tabs, task lists, code blocks, and Mermaid diagrams.
- Keep generated output such as `site/` out of version control.

## Site configuration

Important project metadata is maintained in `mkdocs.yml`:

- `site_name: AI Instruction Library`
- `site_url: https://daedaluschan.github.io/KB_lab/`
- `repo_url: https://github.com/daedaluschan/KB_lab`
- `repo_name: daedaluschan/KB_lab`

The site uses:

- [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) for the theme.
- `mkdocs-awesome-pages-plugin` for folder-based page ordering.
- The Material tags plugin for tag-based discovery.
- `mkdocs-minify-plugin` to minify generated HTML.

Dependency versions are pinned in `requirements.txt`.

## Deployment

GitHub Pages deployment is handled by `.github/workflows/pages.yml`. The workflow:

1. Runs on pushes to `main`, `master`, or `work`, and can also be started manually.
2. Checks out the repository.
3. Sets up Python and installs dependencies from `requirements.txt`.
4. Runs `mkdocs build --strict`.
5. Uploads the generated `site/` directory with the official Pages artifact action.
6. Deploys the artifact to GitHub Pages.

When editing the workflow, keep the current Node.js 24-ready first-party action versions and the official Pages artifact upload path unless there is a deliberate migration plan.

## Recommended checks before committing

Run these checks after changing documentation, MkDocs configuration, dependencies, or the Pages workflow:

```bash
ruby -e 'require "yaml"; YAML.load_file(".github/workflows/pages.yml"); YAML.load_file("mkdocs.yml"); puts "YAML files parse"'
mkdocs build --strict
```

For workflow changes, also verify that custom tar/gzip artifact packaging has not been reintroduced.

## Contributing content

1. Create or update Markdown under `docs/`.
2. Add useful tags in front matter.
3. Keep pages focused on one reusable behavior, standard, or workflow.
4. Preview locally with `mkdocs serve`.
5. Validate with `mkdocs build --strict`.
6. Commit the Markdown and configuration changes, but do not commit generated `site/` output.
