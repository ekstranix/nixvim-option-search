## Context

The theme `extranix-options-search` is shared between `home-manager-option-search` and `nixvim-option-search`. NixVim's options JSON is ~8.9 MB vs ~1.4 MB for home-manager, causing the search input to freeze on keystroke. Three bottlenecks compound: too-short debounce (100ms), expensive sorting on broad queries, and unbounded DOM rendering of all results.

The theme already uses a pattern of injecting Hugo params as hidden `<input>` elements (e.g., `useMarkdownParser`, `release_current_stable`) which JS reads at runtime. We follow this pattern.

Three repos involved:
- `hugo-theme-extranix-options-search/` — source of truth for theme changes
- `nixvim-option-search/` — develop and test here
- `home-manager-option-search/` — verify compatibility

## Goals / Non-Goals

**Goals:**
- Search input remains responsive while typing, even with NixVim's ~8.9 MB dataset
- All performance parameters are configurable via Hugo `config.yaml` with sensible defaults
- No breaking changes for home-manager-option-search (defaults work without config changes)

**Non-Goals:**
- Web Worker architecture (deferred to a future change)
- Virtual scrolling / infinite scroll
- Server-side search

## Decisions

### 1. Config injection via hidden inputs (existing pattern)

The theme template injects params as hidden `<input>` elements. JS reads these with fallback defaults. This matches the existing `useMarkdownParser` and `release_current_stable` pattern.

**Alternative considered:** Global JS config object in a `<script>` block. Rejected because the hidden input pattern is already established and changing patterns adds confusion.

### 2. Three configurable parameters

| Parameter | Hugo param | Default | Purpose |
|-----------|-----------|---------|---------|
| Debounce | `search_debounce_ms` | `250` | Delay before search fires after typing stops |
| Max results | `search_max_results` | `200` | Cap on DOM-rendered rows |
| Sort min chars | `search_sort_min_chars` | `3` | Minimum query length before sorting kicks in |

Defaults are chosen to work well for both datasets. Home-manager (smaller) won't notice the change. NixVim (larger) will feel dramatically better.

### 3. "Showing X of Y" indicator with "Show all" option

When results are capped, display a message like "Showing 200 of 3,847 results" with a "Show all" button. The button renders the full set (may lag on large datasets, but it's an explicit user choice).

### 4. Develop in theme repo, test via submodule

Changes go into `hugo-theme-extranix-options-search`. The nixvim site updates its submodule pointer to test. Once stable, home-manager site also updates its submodule pointer.

## Risks / Trade-offs

- **[Show all still freezes]** → Acceptable since it's an explicit user action, not the default experience. Can be replaced with "show 200 more" pagination later.
- **[Defaults may need tuning]** → Debounce of 250ms and cap of 200 are educated guesses. May need adjustment after real-world testing. Being configurable mitigates this.
- **[Sort skip at 3 chars changes result order]** → For 1-2 character queries, results will be unsorted (insertion order from js-search). This is fine because short queries return too many results for order to matter visually.
