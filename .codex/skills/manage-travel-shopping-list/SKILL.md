---
name: manage-travel-shopping-list
description: "Maintain the KB Lab trip shopping checklist: add, edit, remove, or mark purchased items for Dad, Mom, or Jinny; attach supplied or generated product images; and add destination sections. Use whenever a user asks to change the travel shopping list or its item images."
---

# Manage Travel Shopping List

Update `docs/travel/hong-kong-japan-shopping-list.md` as the source of truth. Read the repository `AGENTS.md` before editing for the current list conventions.

## Item workflow

1. Identify the requested destination, item name, owner, and whether it is bought. Use an unchecked `- [ ]` status for a new item unless the user says it is already purchased; then use `- [x]`.
2. Add or update the matching destination table row. Keep the bilingual columns `已買 / Purchased`, `物品 / Item`, `擁有人 / Owner`, and `相片 / Photo`; owners are `Dad`, `Mom`, and `Jinny`.
3. If the user supplies an image already in `docs/assets/images/travel-shopping/`, reference it. If the user asks for an image and none is supplied, use the `imagegen` skill to create a neutral, unbranded product-reference image, then copy the selected asset into that folder with a clear, collision-free filename.
4. Embed every image with a relative Markdown path and `{ width="160" }`, for example `![Item](../assets/images/travel-shopping/item.png){ width="160" }`. Use `—` only when no image is requested or available.
5. Place items with no decided purchase location in the `## 待定 TBD` table. Do not change the page title or navigation for this holding section.
6. When adding a destination, create a `##` section and matching table, then update the page title, `docs/travel/.pages`, and `docs/travel/index.md` to include it.

## Finish

- Run `git diff --check` after edits. Run `mkdocs build --strict` when MkDocs is available.
- Do not commit or push unless the user explicitly requests publication.
