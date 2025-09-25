# InsightfulAffiliate_NextGenCopyAI — Maintenance Agent (Codex)

## Goals (do all of these)
1) Repository hygiene: detect moved/renamed files and fix intra-repo paths for images, CSS files, and manifest references.
2) Validate & normalize website code blocks for **Systeme.io** (HTML/CSS/JS snippets must be copy-paste ready).
3) Reconcile duplicates: there may be 2 CSS files and 2 web manifests; unify or point everything to the canonical ones and remove dead refs.
4) Produce a **checklist report** and **copy-paste blocks** (Markdown) for each landing page/section.

## Scope (keep runtime/cost tight)
- Only scan and edit these paths:
  - `landing_pages/`
  - `website_code_block_ORGANIZED/`
  - `assets/` (images, css, icons)
  - `docs/` (only for writing reports to `docs/ai_outputs/`)
- Ignore: `.git`, `node_modules`, `dist`, `build`, `logs`, `venv`, zips.

## House style (Systeme.io rules of thumb)
- Snippets must be **self-contained**: inline `<style>` allowed, but prefer linking **one** canonical CSS (relative path).
- Avoid smart quotes and non-UTF-8 glyphs.
- Keep `<head>`/`<html>` tags **out** of snippets meant for page sections; include only when producing a full page.
- Preserve and surface metadata placeholders (e.g., `{{utm}}`, `{{order_bump}}`) — do not remove or expand them.

## Canonicals (choose or create)
- Canonical CSS: `assets/css/ngcai.css` (create if missing; migrate rules from duplicates).
- Canonical Manifest: `assets/site.webmanifest` (migrate icons/shortcuts; update all refs).
- Canonical Icons: ensure `assets/icons/` contains the icon set the manifest references; fix missing sizes.

## Tasks
1) **Inventory**
   - List all HTML/CSS/manifest files under scope.
   - Identify duplicate CSS/manifest files and which files reference them.

2) **Unify**
   - Pick canonical CSS + manifest (as above).
   - Update all HTML to point to those canonicals with correct **relative** paths.
   - Remove stale/duplicate refs (only delete files if there is a confirmed 1:1 migration and no remaining refs).

3) **Fix paths**
   - For images `<img>`, favicons, and manifest icons: fix broken/absolute paths to correct relative paths.

4) **Validate for Systeme.io**
   - For each landing page/section, emit:
     - A “Section” snippet (no `<html>/<head>`, only body block) ready to paste into Systeme’s block.
     - If a **full page** snippet is required, provide a separate “Full HTML” version with `<html>…</html>`.
   - Ensure CSS either links to canonical CSS once, or provide a minimal inline `<style>` when truly necessary.

5) **Outputs**
   - Write a Markdown report at: `docs/ai_outputs/repo_maintenance_report.md` with:
     - Summary (files touched, refs updated, deleted duplicates).
     - Table of each page/section and its snippet locations.
     - “Next actions” list if any items need manual confirmation.
   - For each processed page/section, write copy-paste snippets to:
     - `docs/ai_outputs/snippets/<slug>-section.md`
     - (optional) `docs/ai_outputs/snippets/<slug>-full.html.md`

## Safety / constraints
- Never change `.gitignore`, `.editorconfig`, or `.gitattributes`.
- Do not touch anything outside the scope paths.
- Prefer edits that **reduce** future maintenance (one CSS, one manifest).
- If unsure, emit TODOs in the report instead of guessing.

## Acceptance check (do this at the end)
- Run a broken-link pass (relative references only) and include results in the report.
- Confirm every HTML page/section now references the canonical CSS/manifest.
- Confirm all updated files are valid UTF-8 and free of smart quotes.

## Deliverables summary
- `docs/ai_outputs/repo_maintenance_report.md` (overview + checklist)
- `docs/ai_outputs/snippets/*` (copy-paste ready blocks)

