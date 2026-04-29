# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

This is a **GitHub profile repository** (`Vibhu2/vibhu2`). The primary file is `README.md`, which renders as Vibhu Bhatnagar's public GitHub profile. There is no application code to build, test, or lint.

---

## Automation (GitHub Actions)

Two workflows run daily at **18:30 UTC** (midnight IST) and can also be triggered manually via `workflow_dispatch`:

### `psgallery-update.yml`
Queries the PowerShell Gallery OData API and updates version/download badges in `README.md`. The Python script uses HTML comment markers as injection points:

- `<!-- PSGALLERY-VERSION:VB.ModuleName -->` / `<!-- /PSGALLERY-VERSION:VB.ModuleName -->` — wraps the version badge
- `<!-- PSGALLERY-DL:VB.ModuleName -->` — wraps the download count badge
- `<!-- PSGALLERY-TABLE-END -->` — marks where new module rows are appended

**API filter**: `startswith(Id,'VB') and Authors eq 'Vibhu_Bhatnagar' and IsLatestVersion eq true`

### `blog-post-workflow.yml`
Uses [`gautamkrishnar/blog-post-workflow@v1`](https://github.com/gautamkrishnar/blog-post-workflow) to pull the 5 latest posts from `https://pwsh.in/index.xml` (Hugo RSS). Injects them between:

- `<!-- BLOG-POST-LIST:START -->` / `<!-- BLOG-POST-LIST:END -->`

---

## README Structure Rules

The README uses HTML comment markers for automated content insertion. **Do not remove or rename these markers** or the workflows will stop updating correctly:

| Marker | Purpose |
|--------|---------|
| `<!-- PSGALLERY-VERSION:ModuleId -->` … `<!-- /PSGALLERY-VERSION:ModuleId -->` | Version badge per module |
| `<!-- PSGALLERY-DL:ModuleId -->` … `<!-- /PSGALLERY-DL:ModuleId -->` | Download badge per module |
| `<!-- PSGALLERY-TABLE-END -->` | Auto-insert anchor for new module rows |
| `<!-- BLOG-POST-LIST:START -->` … `<!-- BLOG-POST-LIST:END -->` | Blog post list managed by `blog-post-workflow` |

To **add a new PowerShell module row manually**, insert a table row with the correct marker pairs before `<!-- PSGALLERY-TABLE-END -->`. The automation will then keep its badges updated.

---

## Badge Style Convention

All shields.io badges use a consistent style: `color=5391FE` (PowerShell blue), `style=flat`. Match this when adding new badges.
