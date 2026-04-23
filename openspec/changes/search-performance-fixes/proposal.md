## Why

The NixVim options JSON is ~8.9 MB with thousands of options, 6x larger than home-manager's dataset. The current search implementation runs search, sort, and full DOM rendering on every keystroke with only a 100ms debounce, causing the input field to freeze while typing. This needs fixing now because the site is unusable for NixVim's dataset size, and the fixes must remain compatible with the home-manager site that shares the same theme.

## What Changes

- Increase search debounce from 100ms to a configurable value (default 250ms)
- Cap the number of rendered results to a configurable limit (default 200), with a "show more" mechanism
- Raise the sort-skip threshold from 1 character to a configurable value (default 3) to avoid expensive sorting on broad queries
- All values are configurable via Hugo `config.yaml` params, with sensible defaults in the theme so existing sites don't break

## Capabilities

### New Capabilities
- `configurable-search-performance`: Hugo-configurable search performance parameters (debounce, max rendered results, sort threshold) injected from config.yaml into the theme's JS via hidden DOM elements

### Modified Capabilities
<!-- No existing specs -->

## Impact

- **Theme repo** (`hugo-theme-extranix-options-search`): JS changes in `static/js/script.js`, template changes in `layouts/index.html` to inject config params
- **nixvim-option-search**: Add performance params to `config.yaml` (or rely on theme defaults)
- **home-manager-option-search**: Verify theme defaults work without config changes; optionally add params
- No breaking changes — all params have defaults matching or improving current behavior
