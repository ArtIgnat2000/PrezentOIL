# Copilot instructions — ПроНефть

Purpose
- Help AI coding assistants be immediately productive in this small static-site repo.

Big picture
- Single-page static HTML: the entire app lives in `Index.htm` (HTML + inline CSS + inline JS).
- The project is an educational slide-style quest for children (6 slides, ids `slide1`..`slide6`).
- UI state is driven by three small JS functions in the file: `showSlide()`, `updateProgress()`, `toggleCheck()`.

Key files
- [Index.htm](Index.htm) — single source of truth: layout, styles, scripts, and content.

What an agent should know before editing
- There is no build system, package manager, or tests. Preview locally by opening `Index.htm` in a browser.
- Preserve UTF-8 encoding and the Russian text; directory name contains Cyrillic (`ПроНефть`) — avoid renaming paths.
- CSS and JS are inline. If extracting code to new files, update the `<script>` and `<link>` references and keep behavior identical.

Patterns and conventions (project-specific)
- Slides are numbered by element id: `slide1` → `slide6`. Progress dots correspond to slide indexes (0-based in JS).
- Navigation: `showSlide(currentSlide ± 1)` is used by buttons and keyboard handlers. Keep the `totalSlides` constant in sync.
- Visual state is controlled by adding/removing classes and setting `style.display`. Avoid removing `updateProgress()` calls when changing navigation logic.
- The interactive checklist uses `.check-item` elements and `toggleCheck(element)` to flip `checked` class and icon text.

Editing examples (concrete)
- To add a slide: append a `.slide` block, give it id `slideN` where N = totalSlides+1, add a corresponding `.progress-dot` and increment `totalSlides` in the script.
- To change the active-dot logic: modify `updateProgress()` which toggles the `active` class on `.progress-dot` elements.
- To refactor JS into `main.js`: move the functions, include `<script src="main.js"></script>` before `</body>`, and ensure `main.js` runs after DOM load.

Debugging & preview
- Local preview: open `Index.htm` in any browser (Chrome/Edge recommended). Use DevTools to inspect `.slide`, `.progress-dot`, and JS console errors.
- Keyboard navigation: ArrowLeft / ArrowRight are wired — verify event listener remains attached if you refactor scripts.

Integration & external dependencies
- No external build or CI is present. Images use external placeholder URLs; if adding local assets, place them next to `Index.htm` and use relative paths.

Commit and PR guidance for AI
- Keep changes small and focused (this is a single-file site). When extracting files, include a short PR description explaining why (performance, maintainability).
- Preserve existing behavior: screenshots or a short video of the UI before/after are helpful for reviewers.

When to ask the maintainer
- If you plan to add a build system, routing, or move to a multi-file app, ask about intended hosting and audience first.
- Ask before renaming `Index.htm` or the repo folder (Cyrillic path may be significant).

Follow-ups
- Want me to split inline CSS/JS into separate files and add a minimal `README.md` with preview steps? Reply and I'll propose a patch.
