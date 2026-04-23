## Context

This Hugo site was forked from home-manager-option-search. The owner has already updated branding in `config.yaml` (title, URLs, menu links, footer) but the build pipeline, release naming, and CI still target home-manager. The site needs to build and serve NixVim options instead.

NixVim and home-manager both produce options JSON in the standard NixOS format, so the parsing logic is identical — only the flake coordinates and naming conventions differ.

## Goals / Non-Goals

**Goals:**
- Build script fetches and parses NixVim options from `github:nix-community/nixvim`
- Support `main` (unstable) + 4 stable releases: `nixos-25.11`, `nixos-25.05`, `nixos-24.11`, `nixos-24.05`
- CI workflow builds all supported releases on push and daily schedule
- Config and CI use consistent naming throughout

**Non-Goals:**
- Logo/branding images (owner will handle separately)
- Performance optimization for larger JSON (~8.9 MB vs ~1.4 MB) — deferred
- Frontend/theme changes (already generic)

## Decisions

### 1. Environment variable naming: `NIXVIM_RELEASE`

Replaces `HM_RELEASE`. Matches the project name and avoids confusion with any home-manager tooling that might be present in the build environment.

### 2. Direct branch name mapping in config

Release values in `config.yaml` will use exact NixVim branch names (`main`, `nixos-25.11`, etc.). No translation layer — the value from config goes directly into the flake URI. This is simpler and matches how the home-manager version worked.

### 3. Script rename to `build_and_parse_nixvim_options.rb`

Clean break from the home-manager origin. The script path appears in the workflow and could be called manually, so a clear name prevents confusion.

### 4. Keep parsing logic unchanged

NixVim uses the same NixOS options JSON format (literalExpression, declarations, etc.). The `isLiteralExpression`, `getValFor`, and `parseVal` methods work as-is.

### 5. Result path: `./result/share/doc/nixos/options.json`

NixVim's `#options-json` output places the file at `share/doc/nixos/options.json` (not `share/doc/nixvim/`). This is the NixOS convention that NixVim follows.

## Risks / Trade-offs

- **[8.9 MB JSON payload]** → Deferred. Current js-search may be slow on mobile. Will need investigation (lazy loading, pagination, or server-side search) in a future change.
- **[Old data files left behind]** → Clean up `static/data/options-master.json` and `options-release-*.json` to avoid confusion. These won't be regenerated but could be served if present.
- **[NixVim branch availability]** → Older branches (pre-24.05) may not have `#options-json` output. Limiting to 4 recent releases mitigates this.
