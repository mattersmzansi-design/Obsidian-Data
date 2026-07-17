# CLAUDE.md

Guidance for AI assistants (Claude Code and others) working in this repository.

## What this is

A **single-page, fully self-contained stakeholder portal** for **Squnga'esihle
Trading (Pty) Ltd**, a KwaZulu-Natal (South Africa) project-management, social-
facilitation, and housing-development enterprise. It presents portfolio metrics,
a social-facilitation methodology timeline, statutory registers (CIDB / NHBRC),
and placeholder modules for community media and handover celebrations.

There is **no build step, no package manager, and no dependencies**. The entire
site is one HTML file with inline CSS and vanilla JavaScript.

## Repository layout

```
index.html   The complete portal — HTML structure, inline <style>, inline <script>
README.md    Human-facing overview of features and notes
CLAUDE.md    This file
```

That is the whole project. There is no `src/`, no toolchain, no CI.

## How to run / preview

Open `index.html` directly in a browser — there is nothing to install or compile.
For a local server (e.g. to avoid `file://` quirks): `python3 -m http.server` from
the repo root, then visit the printed URL.

## Structure of `index.html`

The file is organized top to bottom as:

1. **`<head>` → `<style>`** — all CSS. Design tokens live in `:root` as CSS custom
   properties (`--brand-green: #116943`, `--blueprint-dark`, `--accent-sage`,
   `--industrial-gray`, borders). The aesthetic is a deliberate "industrial /
   blueprint" theme using a monospace font (`Courier New`).
2. **`<body>`** — semantic sections marked with HTML comments:
   - Header navigation (fixed top bar)
   - Dashboard core grid: community workshops (left) + compliance matrix (right)
   - Brand campaign & client appreciation row (Community Reels, handover portal)
   - Verification & editing modals
   - Corporate information footer
3. **`<script>`** (near the bottom) — all interactivity as plain functions on the
   global scope, wired up via inline `onclick=` attributes.

### JavaScript model

State lives in three plain object literals at the top of the `<script>` block, and
four global functions read from them:

- `projects` → `switchProject(key)` updates the portfolio metric tiles
  (`metric-units`, `metric-status`, `metric-stage`). Keys: `potshini`, `edendale`,
  `amahlongwa`.
- `steps` → `switchStep(num)` swaps the methodology-timeline description
  (`timeline-text`) for phases `1`–`3`.
- `certifications` → `openModal(certKey)` / `closeModal()` drive the shared modal
  (`verification-modal`, `modal-title-text`, `modal-body-text`). Keys include
  `cidb`, `nhbrc`, `reel1`–`reel3`, `stone`, `handover`.

Modal/tile content is stored as HTML strings inside these objects and injected with
`innerHTML`, so edits to copy usually happen in the data object, not the markup.

## Conventions

- **Everything stays in one file.** Do not add build tooling, external CSS/JS files,
  or CDN dependencies unless explicitly asked — self-containment is the point.
- **Style via CSS custom properties.** Reuse the `:root` tokens (especially
  `--brand-green` `#116943`) rather than hard-coding new hex values.
- **Interactivity pattern:** add a global `function` in the `<script>` block and
  wire it with an inline `onclick=`, matching the existing style. To add a new
  metric/timeline/modal item, extend the relevant data object (`projects`, `steps`,
  `certifications`) and add the corresponding DOM element with a matching `id` or
  `onclick` key.
- Content is business/marketing copy; several sections are intentional
  `[ PLACEHOLDER BOX ]` slots awaiting real media assets.

## Known notes

- The hero panel references a local image `owner making presentation.jpg` that is
  **not committed**; a solid brand-sage colour is the fallback until the file is
  dropped next to `index.html` (or the `background-image` URL is edited).
- Registration numbers, B-BBEE, CIDB/NHBRC and CSD details in the modals are
  presented content — verify against source documents before treating any value as
  authoritative.

## Git workflow

- Active development branch: `claude/squnga-esihle-portal-g9okam`.
- Commit with clear, descriptive messages and push with
  `git push -u origin claude/squnga-esihle-portal-g9okam`.
- Do **not** open a pull request unless explicitly asked.
