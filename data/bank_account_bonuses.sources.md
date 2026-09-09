# Bank-account bonus snapshot

Source checked: **2026-09-09**. Discovery source: [Doctor of Credit — Best Bank Account Bonuses](https://www.doctorofcredit.com/best-bank-account-bonuses/), plus the five latest bank-category pages through August 9.

The expanded CSV contains **446 cash offer rows: 321 personal and 125 business**, including distinct tiers and explicitly conditional alternatives. The original 24 audited rows remain, with one source-verified Chase fee correction. All **243 listing occurrences** in the live September best-account index and **51 additional recent category entries** have a disposition in [the inventory](bank_account_bonuses.inventory.json): 188 included, 10 already represented, 16 duplicates, 30 excluded, and 50 unresolved. Counts are listing occurrences, not banks or guaranteed current enrollments. The historical category archive is not treated as a list of active offers.

Every added row was checked against first-party terms; Doctor of Credit is discovery, not authority for amounts or conditions. Regional, targeted, in-branch, youth, existing-customer and business restrictions remain visible in the row. Noncash/investment-only offers, unsupported stacks and confirmed expired offers are excluded with evidence; blocked or conflicting current campaigns remain unresolved rather than invented. The inventory records each discovery-to-bank mapping, original bundled/live comparison, added IDs and exact structured values. It also explains successor-bank redirects, same-name institution mismatches, and conflicting campaign dates. Several category entries were future-dated September 12 when retrieved September 9; their article dates are not verification dates.

The public companion repository's `data/bank_account_bonuses.csv` remains the live source of truth; the app's copy is an offline seed, not an automatic publisher. Refreshing in the app downloads this CSV, not Doctor of Credit. Existing persistent caches are not overwritten by seeding: use **Refresh feed** for immediate updates, especially on web/iOS. Desktop checks age on startup when bank tracking is enabled. No parser/schema/UI extension was needed; the larger feed remains below the existing 2 MB limit.

## Curation

- Read the latest article update and the offer section, not just the discovery-page headline. Many articles retain old fine print. Where they conflict, explicitly warn in the row; do not infer a deadline, promo code, fee, or eligibility guarantee.
- `last_verified` records when source evidence was checked, not an issuer eligibility guarantee. Added rows use verified first-party offer URLs. Unknown values are blank, never invented zeroes; the row's audit note and inventory explain unresolved inputs.
- Express cash rewards in integer cents, with a separate stable row per documented deposit tier. Do not value miles, stock, or points as cash. The initial feed omits noncash offers and uncertain portal/referral stacks.
- Chase's combined $900 offer includes the $300 checking and $200 savings components; these rows are alternatives, not additive rewards. The combined offer requires two accounts and is marked `other` with an explicit manual-tracking warning.
- Blank expiration/fee fields mean unknown, not unlimited availability/free banking. Advertised bonuses are gross rewards before fees, taxes, or the opportunity cost of held funds.
- Requirements are plain-language notes. Adding an offer to tracking creates a manual-confirmation requirement; review it and add structured deposit/hold/direct-deposit requirements as needed. No automatic qualification or cooldown decision is inferred from prose.

## Original snapshot articles (historical provenance)

- [Wells Fargo Everyday Checking](https://www.doctorofcredit.com/wells-fargo-500-checking-bonus/): current headline and quoted offer terms agree on $500 despite an older $400 sentence.
- [Chase checking and savings](https://www.doctorofcredit.com/targeted-chase-900-checking-savings-bonus/): latest expiration October 14, 2026; checking fee left unknown because older fee figures remain.
- [U.S. Bank Smartly](https://www.doctorofcredit.com/u-s-bank-450-100-checking-bonus/): only the unambiguous $8,000/$450 tier included; latest expiration September 8, 2026. Old promo-code and lower-tier text is not treated as current.
- [PSECU](https://www.doctorofcredit.com/pa-only-psecu-100-250-checking-bonus/): latest lifetime new-member-bonus exclusion retained; contradictory historical requirements explicitly flagged.
- [Capital One savings](https://www.doctorofcredit.com/capital-one-300-1500-savings-bonus-requires-20000-100000-deposit/): current tier list and latest lookback update used; no current expiration established.
- [Bank of Hawaii savings](https://www.doctorofcredit.com/hi-bank-of-hawaii-200-savings-bonus/): latest August 2026 tiers and 60-day hold used, with warning about historical 150-day language and inconsistent early-closure-fee information.
- [Wells Fargo business](https://www.doctorofcredit.com/wells-fargo-400-825-business-checking-bonus/): July update adds five transactions and September 8, 2026 expiration; post-March 2026 fee used.
- [Bank of America business](https://www.doctorofcredit.com/bank-of-america-400-750-business-checking-bonus/): five current tier amounts, offer-specific links and 12-month owner/signer restriction retained. Promotional fee waiver described as temporary, not a permanent zero fee.
- [TruStone](https://www.doctorofcredit.com/wi-mn-only-trustone-financial-350-checking-bonus/): January 2026 extension and revised direct-deposit tiers plus $50 e-statement reward; no optional referral reward included.
- [Percapita](https://www.doctorofcredit.com/percapita-fintech-300-checking-bonus-25-per-month-direct-deposit-not-required/): discovery-page summary only; enrollment cap and unknown eligibility/fees flagged.

## Maintenance

Update the live CSV in `searchlightdev/credit-card-manager-releases`, preserving stable IDs. Recheck the article and bank terms, update `last_verified`, and validate against `bank_account_bonuses.schema.md`. Remove withdrawn offers or set their verified expiration; the app replaces its cached feed on a successful refresh and preserves the last good cache on failure. Copy the validated live CSV into the private app repo when updating its bundled snapshot. Do not overwrite the credit-card CSV when maintaining the bank feed.
