# Credit Card Manager

A desktop app for tracking credit card churning. It keeps your cards, signup
bonuses, recurring credits, annual-fee renewals, 5/24 status, and point balances
in one place, across multiple people and businesses.

It runs locally and stores everything in a SQLite file on your computer. There
is no server and no account.

## Download

<!-- download-links:start (auto-updated by the release workflow — do not edit by hand) -->
- **macOS (Apple Silicon):** [credit-card-manager-0.1.67-arm64.dmg](https://github.com/searchlightdev/credit-card-manager-releases/releases/download/v0.1.67/credit-card-manager-0.1.67-arm64.dmg)
- **Windows:** [credit-card-manager-0.1.67-x64.exe](https://github.com/searchlightdev/credit-card-manager-releases/releases/download/v0.1.67/credit-card-manager-0.1.67-x64.exe)
- **Linux:** not currently published
<!-- download-links:end -->

macOS: open the `.dmg` and drag the app to Applications. Windows: run the
`.exe`. Your data is created the first time you open the app, and the app
updates itself silently from then on. All versions are on the
[releases page](https://github.com/searchlightdev/credit-card-manager-releases/releases).

## Features

- Cards across people and businesses, with statement and payment dates, annual
  fees, and status (applied, open, closed, rejected).
- Signup bonuses with spend targets, deadlines, and progress. The bonus value is
  calculated from your point valuations rather than typed in by hand.
- Point programs, each with an owner, a balance, and a cents-per-point value.
- Recurring benefits with use-by dates, a view of what's available or expiring,
  and a used/not-used toggle.
- 5/24 tracking per person, including when the next slot opens up.
- Referrals between the people you manage.
- Credit report import. Load an Equifax PDF and it creates a card for each
  account, matching what it can to known products; you fill in the rest.
  Business cards don't appear on your personal credit report, so add those by
  hand using the guided business-card wizard.
- A list of cards that are missing important details, so you can complete them
  over time instead of all at once.
- Export to JSON (with restore) or CSV.

## Privacy

Nothing leaves your machine. Credit report PDFs and any spreadsheet you import
are only read locally. Use the export feature to keep your own backups.

## About this repo

The app's source lives in a private repo; this public companion repo is what
the app itself reaches without authentication:

- **Releases** — the desktop app's auto-updater (electron-updater) checks this
  repo's GitHub Releases.
- **`data/signup_bonuses.csv`** — the signup-bonus offer feed the app refreshes
  weekly, fetched raw from the `main` branch. The CSV here is the source of
  truth; edit it here and installed apps pick it up on their next refresh.
