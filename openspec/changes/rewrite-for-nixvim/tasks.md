## 1. Build Script

- [x] 1.1 Create `scripts/build_and_parse_nixvim_options.rb` based on the existing HM script, with: env var `NIXVIM_RELEASE`, default branch `main`, flake URI `github:nix-community/nixvim`, output `#options-json`, result path `./result/share/doc/nixos/options.json`
- [x] 1.2 Delete old `scripts/build_and_parse_hm_options.rb`

## 2. Configuration

- [x] 2.1 Update `config.yaml` releases: change values to `main`, `nixos-25.11`, `nixos-25.05`, `nixos-24.11`, `nixos-24.05` and set `release_current_stable: nixos-25.11`
- [x] 2.2 Fix repo typo in `config.yaml`: `mixvim-option-search` → `nixvim-option-search`

## 3. CI Workflow

- [x] 3.1 Update `.github/workflows/update.yml`: change env var to `NIXVIM_RELEASE`, update release values to `nixos-25.11`/`nixos-25.05`, point to new script path

## 4. Cleanup

- [x] 4.1 Remove old home-manager data files from `static/data/` (options-master.json, options-release-*.json)
