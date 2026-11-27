# Higitsune Changelog

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
