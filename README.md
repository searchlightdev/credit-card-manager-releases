# Credit Card Manager — releases & data

## Download

<!-- download-links:start (auto-updated by the release workflow — do not edit by hand) -->
- **macOS (Apple Silicon):** [credit-card-manager-0.1.67-arm64.dmg](https://github.com/searchlightdev/credit-card-manager-releases/releases/download/v0.1.67/credit-card-manager-0.1.67-arm64.dmg)
- **Windows:** [credit-card-manager-0.1.67-x64.exe](https://github.com/searchlightdev/credit-card-manager-releases/releases/download/v0.1.67/credit-card-manager-0.1.67-x64.exe)
- **Linux:** not currently published
<!-- download-links:end -->

All versions are on the [releases page](https://github.com/searchlightdev/credit-card-manager-releases/releases).
The app updates itself silently after the first install.

## What this repo is

Public companion repo for [Credit Card Manager](https://github.com/searchlightdev/credit-card-manager)
(source is private). It exists so the app can reach two things without
authentication:

- **Releases** — the desktop app's auto-updater (electron-updater) checks this
  repo's GitHub Releases.
- **`data/signup_bonuses.csv`** — the signup-bonus offer feed the app refreshes
  weekly, fetched raw from the `main` branch.

The CSV is the source of truth for the in-app offer feed; edit it here and
installed apps pick it up on their next refresh.
