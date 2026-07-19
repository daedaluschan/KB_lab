# AGENTS.md

## Repository purpose

This repository publishes a reusable Markdown knowledge base with MkDocs Material and GitHub Pages. It includes reusable instruction-library content and school information, with source documents under `docs/` and site configuration in `mkdocs.yml`.

## Working conventions

- Keep reusable instruction and school information content in Markdown under `docs/`.
- Keep generated build output such as `site/` out of version control.
- Prefer small, focused changes and run `mkdocs build --strict` before committing documentation or MkDocs configuration updates.
- Use `requirements.txt` for Python dependency changes.
- The GitHub Pages workflow lives at `.github/workflows/pages.yml`; update it carefully because it controls production deployment.


## Adding new knowledge base entries

When a user asks to add a new instruction library entry, school information page, or other knowledge base content:

- Convert the provided content into clear, reusable Markdown suitable for the documentation collection.
- Place the new Markdown entry under the relevant `docs/` section, such as `docs/instructions/` for instruction content or `docs/Latymer/` for Latymer school information, unless the user specifies a more appropriate repository location.
- Decide and fill in appropriate tags for the entry using the repository's existing tagging conventions.
- Decide whether metadata is needed for the entry, and add relevant metadata when it improves discoverability, navigation, or maintenance.
- Update the relevant MkDocs Awesome Pages navigation file, such as `docs/.pages`, `docs/instructions/.pages`, or `docs/Latymer/.pages`, so the new entry appears under the appropriate site navigation tab.
- Update the relevant section index page, such as `docs/instructions/index.md` or `docs/Latymer/index.md`, to include a link and short description for the new entry when the section uses a curated list of available content.
- Verify discoverability after building by checking that the generated site output includes the new page in navigation, the section index, or the sitemap as appropriate.
- Preserve the user's intent while improving structure, headings, formatting, and consistency with the existing knowledge base.

## Maintaining the travel shopping list

The trip shopping list is maintained in `docs/travel/hong-kong-japan-shopping-list.md`. It is a living checklist for purchases assigned to Dad, Mom, or Jinny, currently organised into Hong Kong, Japan, and South Korea sections.

When a user asks to add or change a shopping item:

- Add or update a row in the table for the requested destination. Use the columns `已買`, `物品`, `擁有人`, and `相片`.
- New items are unchecked: use `- [ ]` in `已買`; mark a purchased item as `- [x]` only when the user says it has been bought.
- Keep the owner value exactly as `Dad`, `Mom`, or `Jinny` unless the user asks to introduce another owner.
- Store or use supplied images under `docs/assets/images/travel-shopping/` and embed them in the row using a relative path with `{ width="160" }`, for example `![item](../assets/images/travel-shopping/item.jpeg){ width="160" }`.
- Place items whose purchase location is undecided in the `## TBD` table. `TBD` is a holding section, not a destination, so do not add it to the page title or Travel navigation.
- If a new destination is needed, add a matching `##` section and table. Update the page title, `docs/travel/.pages`, and `docs/travel/index.md` so the navigation and overview describe every destination.
- Keep entries concise: use the item name provided by the user and do not add pricing, notes, or a completion date unless explicitly requested.

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
