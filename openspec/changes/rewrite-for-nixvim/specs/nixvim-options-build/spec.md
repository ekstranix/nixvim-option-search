## ADDED Requirements

### Requirement: Build script fetches NixVim options from flake
The build script SHALL fetch options JSON by running `nix build github:nix-community/nixvim/${RELEASE}#options-json` where `${RELEASE}` is determined by the `NIXVIM_RELEASE` environment variable.

#### Scenario: Default release (unstable)
- **WHEN** `NIXVIM_RELEASE` is not set
- **THEN** the script SHALL use `main` as the release branch

#### Scenario: Stable release specified
- **WHEN** `NIXVIM_RELEASE` is set to `stable`
- **THEN** the script SHALL read `release_current_stable` from `config.yaml` and use that value

#### Scenario: Explicit release specified
- **WHEN** `NIXVIM_RELEASE` is set to a specific value (e.g., `nixos-25.05`)
- **THEN** the script SHALL use that value directly as the release branch

### Requirement: Build script reads options from correct path
The script SHALL read the generated options from `./result/share/doc/nixos/options.json`.

#### Scenario: Successful options read
- **WHEN** the nix build completes successfully
- **THEN** the script SHALL parse `./result/share/doc/nixos/options.json` as JSON

### Requirement: Build script outputs JSON to static/data
The script SHALL write parsed options to `static/data/options-${RELEASE}.json` where `${RELEASE}` matches the branch name used for the build.

#### Scenario: Output for main branch
- **WHEN** building the `main` branch
- **THEN** the output file SHALL be `static/data/options-main.json`

#### Scenario: Output for stable branch
- **WHEN** building the `nixos-25.11` branch
- **THEN** the output file SHALL be `static/data/options-nixos-25.11.json`

### Requirement: Config lists NixVim releases with correct branch names
The `config.yaml` SHALL list releases using NixVim's branch naming convention: `main` for unstable and `nixos-YY.MM` for stable releases.

#### Scenario: Release dropdown values
- **WHEN** the site is loaded
- **THEN** the release dropdown SHALL offer: `main`, `nixos-25.11`, `nixos-25.05`, `nixos-24.11`, `nixos-24.05`

### Requirement: CI workflow builds all supported releases
The GitHub Actions workflow SHALL build options for `main`, `nixos-25.11`, and `nixos-25.05` using the `NIXVIM_RELEASE` environment variable.

#### Scenario: CI execution
- **WHEN** the workflow runs (on push or daily schedule)
- **THEN** the script SHALL be invoked once per release with the appropriate `NIXVIM_RELEASE` value
