## ADDED Requirements

### Requirement: Configurable search debounce
The theme SHALL read `search_debounce_ms` from a hidden input element injected by the Hugo template. If not set, the default SHALL be `250`. The debounce timer SHALL delay search execution until the user stops typing for the configured duration.

#### Scenario: Default debounce (no config)
- **WHEN** the site does not set `search_debounce_ms` in `config.yaml`
- **THEN** the search SHALL debounce at 250ms

#### Scenario: Custom debounce
- **WHEN** the site sets `search_debounce_ms: 150` in `config.yaml`
- **THEN** the search SHALL debounce at 150ms

### Requirement: Configurable max rendered results
The theme SHALL read `search_max_results` from a hidden input element injected by the Hugo template. If not set, the default SHALL be `200`. The `updateOptionsTable` function SHALL render at most this many rows.

#### Scenario: Default cap (no config)
- **WHEN** the site does not set `search_max_results` in `config.yaml`
- **THEN** at most 200 result rows SHALL be rendered in the DOM

#### Scenario: Custom cap
- **WHEN** the site sets `search_max_results: 500` in `config.yaml`
- **THEN** at most 500 result rows SHALL be rendered in the DOM

#### Scenario: Results exceed cap
- **WHEN** the search returns more results than `search_max_results`
- **THEN** a message SHALL be displayed indicating "Showing X of Y results"
- **THEN** a "Show all" control SHALL be available to render the full result set

#### Scenario: Results within cap
- **WHEN** the search returns fewer results than `search_max_results`
- **THEN** all results SHALL be rendered and no cap message SHALL appear

### Requirement: Configurable sort minimum character threshold
The theme SHALL read `search_sort_min_chars` from a hidden input element injected by the Hugo template. If not set, the default SHALL be `3`. The custom sort comparator SHALL only execute when the query length meets or exceeds this threshold.

#### Scenario: Default threshold (no config)
- **WHEN** the site does not set `search_sort_min_chars` in `config.yaml`
- **THEN** results SHALL only be sorted when the query is 3 or more characters

#### Scenario: Query below threshold
- **WHEN** the user types a 2-character query and threshold is 3
- **THEN** results SHALL be displayed in js-search insertion order (no custom sort)

#### Scenario: Query meets threshold
- **WHEN** the user types a 3-character query and threshold is 3
- **THEN** results SHALL be sorted using the custom relevance comparator

### Requirement: Hugo template injects performance params
The Hugo template SHALL inject each performance parameter as a hidden `<input>` element with a unique `id`, using the existing pattern established by `useMarkdownParser` and `release_current_stable`.

#### Scenario: Param set in config.yaml
- **WHEN** `search_max_results: 300` is set in `config.yaml` under `params`
- **THEN** the template SHALL render `<input type="hidden" id="search_max_results" value="300" />`

#### Scenario: Param not set in config.yaml
- **WHEN** `search_max_results` is not set in `config.yaml`
- **THEN** the template SHALL NOT render the hidden input, and JS SHALL use its built-in default
