# Copilot instructions — ПроНефть

## Purpose
Help AI coding assistants be immediately productive in this educational slide-based static website.

## Big picture
- **Single-page static HTML**: entire app in `index.htm` (757 lines: HTML + inline CSS + inline JS)
- **Educational slide quest for children**: 6 interactive slides about petroleum/oil for elementary school students
- **Zero dependencies**: no build system, no package manager, no frameworks, no tests
- **Russian language content**: all UI text and content in Russian; directory uses Cyrillic (`ПроНефть`)
- **UI state driven by 3 functions**: `showSlide(n)`, `updateProgress()`, `toggleCheck(element)`

## Key files
- [index.htm](index.htm) — single source of truth (layout, styles, scripts, content)
- `img/` — local image assets: `children.png`, `map.png`, `oil_tower.png`, `tancker.png`
- [README.md](README.md) — minimal project description (3 lines)

## Critical architecture patterns

### Slide management
- 6 slides with sequential IDs: `slide1`, `slide2`, ..., `slide6`
- JS variable `totalSlides = 6` must match actual slide count
- Only one slide visible at a time via `display: none`/`block` and `.active` class
- Navigation bounds enforced: `showSlide(n)` clamps `n` between 0 and `totalSlides - 1`

### Progress indicator
- Progress dots (`.progress-dot`) match slide count 1:1
- `updateProgress()` syncs active dot with `currentSlide` (0-indexed)
- **Critical**: When adding/removing slides, update both HTML dots and `totalSlides` constant

### Navigation flows
- Fixed bottom buttons: circular `.nav-btn` with left/right arrows
- Keyboard shortcuts: ArrowLeft/ArrowRight call `showSlide(currentSlide ± 1)`
- "Start quest" button on slide1: `onclick="showSlide(1)"`
- "Complete quest" button on slide6: `onclick="showSlide(0)"` (returns to start)

### Interactive elements
- Slide6 checklist: `.check-item` with `onclick="toggleCheck(this)"`
- Toggle adds/removes `.checked` class and changes icon: `◯` → `✓`
- Visual feedback: green background (`rgba(40, 167, 69, 0.2)`) when checked

## Responsive design breakpoints
- Mobile (`max-width: 768px`): single-column layouts, static nav buttons
- Desktop 1K+ (`min-width: 1600px`): larger fonts, max-width 1400px container, bigger images

## Developer workflows

### Preview/testing
```powershell
# Open in default browser
start index.htm
# Or specific browser
chrome index.htm
```
- No server needed — opens directly from filesystem
- Test keyboard navigation (Arrow keys) and interactive checklist (slide6)

### Image handling
- Local images: place in `img/` and reference as `img/filename.png`
- External placeholders still present: `https://placehold.co/...` (slide1, slide2, slide6)
- When replacing placeholders, use same dimensions or adjust parent `.illustration` styles

### Character encoding
- **MUST** save as UTF-8 with BOM or UTF-8 to preserve Russian text and Cyrillic path
- VS Code default (UTF-8) works; avoid ANSI/Windows-1251

## Common modification patterns

### Adding a slide
1. Insert new `<div id="slide7" class="slide">...</div>` before `</div><!-- .container -->`
2. Add `<div class="progress-dot" onclick="showSlide(6)"></div>` in `.progress-bar`
3. Update JS: change `const totalSlides = 6;` to `= 7;`
4. Test: verify progress dots update and navigation reaches new slide

### Changing color theme
- Primary accent: `#ff6b35` (orange) — used in buttons, borders, headings
- Background gradient: `#1a1a2e` → `#16213e` → `#0f3460` (dark blue tones)
- Update all instances for consistency (search for hex codes)

### Extracting CSS/JS to separate files
```html
<!-- Replace inline <style> with -->
<link rel="stylesheet" href="styles.css">

<!-- Replace inline <script> with -->
<script src="main.js"></script>
```
- Ensure `main.js` runs after DOM loads (place before `</body>` or use DOMContentLoaded)
- Keep initialization call: `updateProgress();` at script end

## Constraints & pitfalls

### What NOT to change
- File name `index.htm` (not `.html`) — likely intentional for legacy compatibility
- Cyrillic directory name `ПроНефть` — may be significant for hosting/deployment
- Inline icon characters in HTML (emoji: 🇺🇸, 🇷🇺, 🚗, etc.) — part of design language

### Animation dependencies
- Slide transitions use `.slideIn` keyframe animation (0.8s fade-in)
- Triggered by `display: block` change — refactoring display logic may break animations
- Progress dots use `transform: scale(1.2)` and color transition — preserve `.active` class behavior

### Event handler integrity
- Keyboard listener attached on DOM ready: `document.addEventListener('keydown', ...)`
- If refactoring JS to modules, ensure listener re-attaches after slide changes
- Inline `onclick` attributes used throughout (e.g., `onclick="showSlide(1)"`) — if moving to addEventListener, replace all inline handlers

## When to ask the maintainer
- Internationalizing content (adding English version) — may need locale routing
- Replacing slide navigation with URL routing (e.g., `#slide3`) — changes core architecture
- Adding analytics or external scripts — may conflict with educational/privacy requirements
- Significant restructuring (multi-file, build pipeline) — validate against deployment constraints
