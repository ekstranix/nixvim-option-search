## Why

This Hugo site was forked from home-manager-option-search and needs to serve as a search engine for NixVim options instead. The build script, config, and CI workflow still target the home-manager flake. NixVim uses different branch naming (`main`/`nixos-YY.MM` vs `master`/`release-YY.MM`), a different flake output (`#options-json` vs `#docs-json`), and a different result path (`share/doc/nixos/` vs `share/doc/home-manager/`).

## What Changes

- Rewrite `scripts/build_and_parse_hm_options.rb` to target the NixVim flake, rename to `build_and_parse_nixvim_options.rb`
- Update `config.yaml` release values from `release-YY.MM` → `nixos-YY.MM`, default from `master` → `main`, fix repo typo, and mark logo as TODO
- Update `.github/workflows/update.yml` to use new env var names, release values, and script path
- Remove old home-manager JSON data files from `static/data/`

## Capabilities

### New Capabilities
- `nixvim-options-build`: Build and parse NixVim options JSON from the nix-community/nixvim flake, supporting `main` (unstable) and `nixos-YY.MM` stable release branches

### Modified Capabilities
<!-- No existing specs to modify -->

## Impact

- **Build script**: Complete rewrite of flake target, env vars, and paths
- **Config**: Release list changes from 7 entries to 5 (main + 4 releases: nixos-25.11, nixos-25.05, nixos-24.11, nixos-24.05)
- **CI workflow**: Updated release matrix and script reference
- **Data files**: Old `options-release-*.json` and `options-master.json` files will be replaced by `options-main.json` and `options-nixos-*.json`
- **No frontend changes needed**: The Hugo theme and JS search are already generic enough
