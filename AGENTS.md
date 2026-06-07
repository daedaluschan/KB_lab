# AGENTS.md

## Repository purpose

This repository publishes a reusable Markdown instruction library with MkDocs Material and GitHub Pages. Source documents live under `docs/`, and the site is configured by `mkdocs.yml`.

## Working conventions

- Keep reusable instruction content in Markdown under `docs/`.
- Keep generated build output such as `site/` out of version control.
- Prefer small, focused changes and run `mkdocs build --strict` before committing documentation or MkDocs configuration updates.
- Use `requirements.txt` for Python dependency changes.
- The GitHub Pages workflow lives at `.github/workflows/pages.yml`; update it carefully because it controls production deployment.

## MkDocs authoring notes

- Use MkDocs Material features already enabled in `mkdocs.yml`, including admonitions, collapsible details, tabs, task lists, and Mermaid diagrams.
- The Material tags plugin no longer needs a `tags_file` option. Do not re-add `tags_file`; strict builds fail on that warning.
- Keep the inline tags marker in `docs/tags.md` for the tags index page.
- Maintain the project-site metadata in `mkdocs.yml` for this repository:
  - `site_url: https://daedaluschan.github.io/KB_lab/`
  - `repo_url: https://github.com/daedaluschan/KB_lab`
  - `repo_name: daedaluschan/KB_lab`

## GitHub Pages deployment lessons learned

The Pages build and deployment were fixed after several strict-mode and deployment issues. Preserve these lessons when editing the workflow:

1. **Avoid deprecated Material tags configuration**
   - Problem: `mkdocs build --strict` failed with `Plugin 'material/tags' option 'tags_file': This setting is not required anymore`.
   - Fix: remove `tags_file` from `mkdocs.yml` and use the inline `<!-- material/tags -->` marker in `docs/tags.md` instead.

2. **Use Node.js 24-ready GitHub Actions**
   - Problem: GitHub Actions warned that Node.js 20 actions were deprecated.
   - Fix: use Node.js 24-ready first-party action versions and keep `FORCE_JAVASCRIPT_ACTIONS_TO_NODE24: true` in the workflow environment.
   - Current expected action versions:
     - `actions/checkout@v6`
     - `actions/setup-python@v6`
     - `actions/configure-pages@v6`
     - `actions/upload-pages-artifact@v5`
     - `actions/deploy-pages@v5`

3. **Use the official Pages artifact action**
   - Problem: A manually created `tar`/`gzip` artifact uploaded through generic `actions/upload-artifact` allowed the deploy request to start but failed with the vague message `Deployment failed, try again later`.
   - Fix: remove custom artifact packaging and upload the built `site/` directory with `actions/upload-pages-artifact@v5`, which produces the artifact layout expected by `actions/deploy-pages@v5`.

4. **Configure Pages before upload/deploy**
   - Add `actions/configure-pages@v6` before dependency installation, build, and artifact upload so GitHub Pages metadata is initialized through the official Pages workflow path.

## Recommended validation commands

Run these commands after changing workflow, dependency, or MkDocs configuration files:

```bash
ruby -e 'require "yaml"; YAML.load_file(".github/workflows/pages.yml"); YAML.load_file("mkdocs.yml"); puts "YAML files parse"'
mkdocs build --strict
```

For workflow changes, also verify the expected action versions remain present and old custom artifact packaging does not return.
