## 1. Theme template changes (hugo-theme-extranix-options-search)

- [x] 1.1 Add hidden `<input>` elements for `search_debounce_ms`, `search_max_results`, and `search_sort_min_chars` in `layouts/index.html`, following the existing `{{ with .Site.Params.X }}` pattern
- [x] 1.2 Add a "Showing X of Y results — Show all" container element below the search form (hidden by default)

## 2. Theme JS changes (hugo-theme-extranix-options-search)

- [x] 2.1 Read performance config from hidden inputs with fallback defaults (250, 200, 3) at the top of `script.js`
- [x] 2.2 Update `SEARCH_INPUT_DEBOUNCE_MS` to use the configurable value instead of hardcoded 100
- [x] 2.3 Update `updateOptionsTable` to cap rendered rows at `search_max_results` and show/hide the "Showing X of Y" message
- [x] 2.4 Add "Show all" click handler that re-renders with the full result set
- [x] 2.5 Update the sort condition in `searchOptions` to use `search_sort_min_chars` instead of hardcoded `1`

## 3. Site configuration (nixvim-option-search)

- [x] 3.1 Copy changed theme files into submodule dir for local testing (submodule ref update deferred until theme is pushed)
- [x] 3.2 Test with `hugo serve` — verify debounce, result capping, sort threshold, and "Show all" behavior

## 4. Compatibility verification (home-manager-option-search)

- [x] 4.1 Copy changed theme files into home-manager submodule dir for local testing (submodule ref update deferred until theme is pushed)
- [x] 4.2 Verified `hugo` build succeeds — defaults work without any config.yaml changes
