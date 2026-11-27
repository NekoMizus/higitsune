# Higitsune Changelog

## 2026-11-26 | External dashboard stylesheet

### Summary

Moved the dashboard's styling into `styles.css`.

### Changes

The page loads the stylesheet through a relative link. The stylesheet now includes category accents, floating decorations, SVG presentation, and tooltip placement rules.

### Rationale

Keeping styles together makes the dashboard theme easier to maintain and keeps the HTML focused on structure.

### Implementation

Preserved the original rule order, responsive breakpoints, and animation settings. Decoration rules target the existing child order, category rules use each section's heading reference, and JavaScript supplies live tooltip coordinates as CSS custom properties.

### Files

- `index.html` links the stylesheet and provides markup and runtime tooltip coordinates.
- `styles.css` replaces the unused placeholder rules with the dashboard styles.
- `CHANGELOG.md` records the change.

### Impact

The stylesheet resolves alongside the page on static hosting and introduces no external dependencies.

### Validation

Headless Firefox before-and-after screenshots matched pixel for pixel at 360 × 720 and 1100 × 720 with reduced motion enabled. Taller captures matched outside the tooltip region, including the calculator. The tooltip fixture confirmed that the new tooltip rendered inside the viewport. `git diff --check` passed.

### Limitations

Chromium and a deployed page were not tested.

## 2026-11-26 | Calculator ANS, square root, and keypad colors

### Summary

Added ANS and square-root keys and rearranged the keypad into six rows of equally sized buttons.

### Changes

The last two rows are `( 0 ) %` and `ANS . √ =`. Mint marks numeric input, peach marks editing controls, blue marks functions, lavender marks arithmetic operators, and rose marks equals.

### Rationale

The layout gives zero and percent the same footprint as the other keys and makes the different controls easier to scan.

### Implementation

ANS recalls the most recent successful calculation from History. The square-root key wraps a current number or opens a square-root group for input. The evaluator applies the root when the expression is evaluated and records its notation in History.

### Files

- `index.html` updates the keypad layout, styles, and calculator behavior.
- `CHANGELOG.md` records the change.

### Impact

ANS works after Clear and after an error. Square roots support grouped input and report an error for negative arguments.

### Validation

Headless Firefox passed checks at 360 px for the requested key order, equal widths, five color groups, ANS recall, simple and nested roots, negative roots, percent regression, and history formatting. `git diff --check` passed.

### Limitations

Chromium and manual assistive technology were not tested.

## 2026-11-26 | Calculator percent and parentheses

### Summary

Added percent and parentheses to the calculator keypad and keyboard input.

### Changes

Parentheses group expressions and can be nested. Percent divides the preceding number or group by 100. Adjacent operands and groups insert a visible multiplication sign.

### Rationale

Users can enter grouped arithmetic and percentage calculations from the dashboard.

### Implementation

The evaluator uses value and operator stacks to apply grouping, postfix percent, and standard binary precedence. Input handling keeps incomplete groups visible until they can be evaluated. History records the expression with its parentheses and percent symbols.

### Files

- `index.html` updates the keypad, input handling, evaluator, and history formatting.
- `CHANGELOG.md` records the change.

### Impact

Existing arithmetic remains available. Percent is a standalone conversion, so `200 + 10%` evaluates to `200.1`.

### Validation

Headless Firefox passed checks for keypad and keyboard entry, grouped precedence, nested and adjacent groups, percent on numbers and groups, incomplete parentheses, Backspace, division-by-zero propagation, and history formatting. `git diff --check` passed.

### Limitations

Chromium and manual assistive technology were not tested.

## 2026-11-26 | Calculator order of operations

### Summary

The calculator evaluates multiplication and division before addition and subtraction.

### Changes

Mixed expressions now follow standard arithmetic precedence. Operators with equal precedence execute left to right.

### Rationale

An expression such as `9 + 9 × 2` should evaluate to 27.

### Implementation

Evaluation accumulates multiplication and division within each term before combining terms with addition and subtraction. The final result is rounded to 12 significant digits, and history reflects the entered expression.

### Files

- `index.html` updates the evaluator and history formatting.
- `CHANGELOG.md` records the change.

### Impact

The keypad and keyboard continue to evaluate only when requested. Existing history entries are kept as recorded.

### Validation

Headless Firefox passed checks for mixed precedence, left-to-right evaluation at equal precedence, rounding, division by zero, history formatting, and incomplete expressions. `git diff --check` passed.

### Limitations

Chromium was not tested.

## 2026-11-26 | Calculator expression entry

### Summary

The calculator displays the full expression while the user enters it.

### Changes

Enter and the equals key evaluate a complete expression. A trailing operator stays visible, and Backspace can remove it.

### Rationale

Users can review and edit a calculation before evaluating it.

### Implementation

The calculator holds entered operands and operators separately from the current number. On evaluation it applies the existing left-to-right arithmetic and records the completed expression and result in history.

### Files

- `index.html` updates calculator input, display, and evaluation.
- `CHANGELOG.md` records the change.

### Impact

The keypad and keyboard both show pending expressions. History recall can fill the current operand, and the calculator keeps keyboard shortcuts scoped to the keypad.

### Validation

Headless Firefox passed the requested 9+9+9 sequence, incomplete Enter, equals, mixed operations, Backspace, sign changes, division-by-zero recovery, history recall, and calculator slash input. `git diff --check` passed.

### Limitations

Calculations follow entry order. Chromium and manual assistive technology were not tested.

## 2026-11-26 | Calculator history dropdown

### Summary

Replaced the calculator memory row with a pastel History dropdown that retains the 30 latest completed calculations.

### Changes

Each entry shows the expression and result, newest first. Selecting an entry recalls its result as the current operand. The dropdown uses a text label and a Unicode clock symbol.

### Rationale

Users can revisit recent calculations and see how each saved result was produced.

### Implementation

A native disclosure contains a scrollable list of recall buttons. Chained operations use parentheses to show evaluation order. Successful calculations are saved to browser-local storage, and a value from the old memory register appears as a saved value when no history exists.

### Files

- `index.html` adds the history dropdown, expression tracking, recall, and persistence.
- `CHANGELOG.md` records the change.

### Impact

Clearing the display leaves saved history available. Error results are excluded from history. The icon requires no external asset.

### Validation

Headless Firefox passed checks for opening and closing the dropdown, chained expressions, recall as an operand, error exclusion, the 30-entry limit, reload persistence, and dropdown fit at 360 px. `git diff --check` passed.

### Limitations

Chromium, manual assistive technology, and migration from an existing memory register were not tested.

## 2026-11-26 | Calculator memory

### Summary

Added a single calculator memory register with store, recall, add, subtract, and clear buttons.

### Changes

The pastel memory row provides MS, MR, M+, M−, and MC. An M indicator appears when a value is stored, including zero.

### Rationale

Users can reuse a value across calculations or resume with it after reloading the page.

### Implementation

The calculator stores one finite number in browser-local storage and keeps an in-page copy if storage is unavailable. A recalled number can serve as an operand. Typing a digit after recall starts a fresh entry.

### Files

- `index.html` adds memory controls, styles, and persistence.
- `CHANGELOG.md` records the change.

### Impact

The calculator's Clear button resets the current calculation. MC clears the saved number. Memory arithmetic follows the calculator's 12-significant-digit rounding.

### Validation

Headless Firefox passed checks for all memory controls, recall as an operand, replacing a recalled entry, stored zero, division-by-zero handling, persistence across browser launches, invalid saved data, and horizontal fit at 360 px. `git diff --check` passed.

### Limitations

Chromium and manual assistive technology checks were not run.

## 2026-11-26 | Pastel calculator

### Summary

Added a basic calculator beneath the shortcut board.

### Changes

The calculator supports addition, subtraction, multiplication, division, decimals, sign changes, deleting the last digit, and clearing. It displays an error for non-finite results.

### Rationale

The calculator makes everyday arithmetic available from the dashboard and uses a compact pink and lavender panel that matches the existing theme.

### Implementation

Native buttons and a live output element provide the keypad and display. Local JavaScript applies operations in entry order without evaluating user text as code. Calculator keyboard input is scoped to the panel so the slash search shortcut still works elsewhere.

### Files

- `index.html` contains the calculator markup, styles, and logic.
- `CHANGELOG.md` records the change.

### Impact

The calculator adds a panel below the links and keeps its state in memory for the current page.

### Validation

Headless Firefox checks at 360 px and 1100 px passed for arithmetic, decimals, sign changes, clear and delete, division by zero and recovery, operator replacement, keyboard input, the search shortcut, and calculator placement. `git diff --check` passed.

### Limitations

Calculations use browser number arithmetic and round results to 12 significant digits. Chromium and manual assistive technology checks were not run.

## 2026-11-26 | DuckDuckGo site filters

### Summary

DuckDuckGo searches now use the same checked sites as Google searches.

### Changes

The site count and clear button are available on DuckDuckGo, and the site checkbox labels describe filtering web searches.

### Rationale

Users can switch web search engines while keeping the same set of selected sites.

### Implementation

DuckDuckGo uses a bare `site:` operator for one domain and the existing parenthesized `OR` expression for multiple domains. Both web engines use the existing deduplicated domain selection.

### Files

- `index.html` extends site filtering and its controls to DuckDuckGo.
- `CHANGELOG.md` records the change.

### Impact

Existing saved site selections apply to both web engines. YouTube continues to submit the plain query.

### Validation

Headless Firefox passed checks for DuckDuckGo URLs with zero, one, and two sites, Google and YouTube submissions, retained selections, the site count, and clearing filters. `git diff --check` passed.

### Limitations

The Firefox check captured generated URLs before navigation. DuckDuckGo documents the single-domain operator. Live handling of the multi-domain expression was not verified.

## 2026-11-26 | Search engine selection

### Summary

Added Google, DuckDuckGo, and YouTube radio buttons below the search bar.

### Changes

Google is the initial default. The page remembers the last selected engine and updates the search field and submit button labels when the selection changes.

### Rationale

A visible, keyboard-accessible selector lets users switch search engines from the same input.

### Implementation

Native radio buttons select fixed HTTPS endpoints. The form encodes the query with URLSearchParams and stores the engine separately from existing site-filter preferences.

### Files

- `index.html` adds the selector, responsive styling, and search handling.
- `CHANGELOG.md` records the change.

### Impact

Checked site filters apply to Google searches and reappear when users switch back to Google. The existing slash shortcut still focuses the search field.

### Validation

Headless Firefox checks at 360 px and 1100 px passed for the default, all three generated URLs, query encoding, option persistence and restoration, site filters, blank query rejection, and horizontal layout. `git diff --check` passed.

### Limitations

Firefox checks captured the destination URLs before navigation. Live search results, manual keyboard interaction, and Chromium were not tested.

## 2026-11-26 | Randomized wallpapers

**Category:** Feature

### Summary

Added randomized wallpaper selection with persistent user preferences.

### Changes

- Added a wallpaper randomization setting.
- Implemented automatic wallpaper selection on page load.
- Added a fallback for missing wallpaper assets.

### Rationale

Randomization provides visual variety without requiring users to manually select wallpapers. Fallback behavior prevents missing assets from leaving the dashboard with a broken background.

### Implementation

- Used Math.random() to select from available wallpapers.
- Stored the randomization preference using localStorage.
- Reused the existing background-rendering functionality instead of introducing a separate rendering system.

### Files

- `index.html`: Added the randomization setting.
- `style.css`: Styled the new setting.
- `script.js`: Implemented wallpaper selection, persistence and fallback behavior.

### Impact

- Existing manually selected wallpapers remain supported.
- No additional dependencies or network requests.
- GitHub Pages compatibility preserved.

### Validation

- Verified randomized selection.
- Verified preference persistence after reloading.
- Verified fallback behavior with missing assets.

### Limitations

- Mobile browser testing was not performed.
