# Credit Card Manager — releases & data

Public companion repo for [Credit Card Manager](https://github.com/searchlightdev/credit-card-manager)
(source is private). It exists so the app can reach two things without
authentication:

- **Releases** — the desktop app's auto-updater (electron-updater) checks this
  repo's GitHub Releases.
- **`data/signup_bonuses.csv`** — the signup-bonus offer feed the app refreshes
  weekly, fetched raw from the `main` branch.

The CSV is the source of truth for the in-app offer feed; edit it here and
installed apps pick it up on their next refresh.
