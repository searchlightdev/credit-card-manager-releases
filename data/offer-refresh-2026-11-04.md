# Offer refresh 2026-11-04 — sources, evidence and assumptions

Ledgers: `data/bank_account_bonuses.refresh-2026-11-04.json` (bank: 13 appended offers plus 3 stable-ID in-place updates, each with exact full before/after rows and step groups under `changes`) and `data/card-offer-refresh-2026-11-04.json` (cards: 7 in-place updates and 4 appended rows, full before/after rows). Per-item discovery dispositions, article update evidence, index-only occurrences and the 62 reviewed outside-category items are recorded in each ledger under `discovery`.

## Window and discovery

- Run 2026-11-04T14:40:22-05:00; calendar 7-day lookback in America/New_York starts 2026-10-28T14:40:22-04:00 (169 elapsed hours across the 2026-11-01 DST change). Whole-day overlap scanned from 2026-10-28T00:00:00-04:00.
- Sources: Doctor of Credit RSS `/category/bank-account-bonuses/feed/` and `/category/credit-cards/feed/`, pages 1-2 each (both reach before the window start); the all-category feed pages 1-8 (independently fetched, 116 in-window items, 54 unique category URLs, 62 outside-category items reviewed with no additional signup lead); and the DoC bank and card best-offer indices for index-only additions/removals that were not re-dated in RSS.
- Coverage gaps: a post updated without re-dating would not surface; RSS pubDate is DoC's latest re-dated date, so original-vs-update comes from each article's dated update notes (recorded per item under `discovery` in both ledgers). The Heritage Financial CU post is dated 2026-11-07, after the run, and is counted as an outside-window anomaly, not an in-window item.
- Bank: {"in_window_items":26,"added":8,"update":1,"unresolved":5,"already_present":3,"expired":1,"excluded":8} plus 1 outside-window anomaly (Heritage). Cards: {"in_window_items":29,"update":3,"added":2,"already_present":1,"unresolved":3,"excluded":19,"cross_listed":1}. Unique in-window items: 26 + 29 - 1 cross-listed = 54.
- Index-only occurrences (separate from feed totals): bank 2026-11-02 removed E*TRADE $400 and 2026-10-31 removed Synchrony Savings $100 via Rakuten (both expired in place), 2026-10-28 added Connexus (already present from 2026-10-27); cards 2026-10-28 added U.S. Bank Business Essentials Plus (DoC article originally published 2026-09-29; newly listed in the window) and removed U.S. Bank Leverage Business.

## Evidence rules applied

- Every recorded term was read from raw first-party text on 2026-11-04: operator browser captures (2UFi, Axos, Wells Fargo, Quorum incl. membership and fee schedule, Citi incl. its Offers legal tab, U.S. Bank Business Essentials Plus and business card comparison, Southwest Plus, Ameriprise, Navy Federal) or raw HTML saved by plain curl GET (CapEd, NW Plus, Acclaim, Firstmark, E*TRADE expiry redirect, Quorum rates, Citi script chunk, Chase Southwest Premier/Priority, Chase Ink Cash/Unlimited, Barclays Earner Plus terms). Discovery WebFetch summaries were only used to find pages.
- Geography is unknown on every new bank row: membership charters, branch footprints and DoC titles are not offer residence rules. ChexSystems is unknown on the twelve new-campaign rows; only the Citi checking-only tier `citi-bundled-regular-checking-325` carries the existing Citi rows' secondary "yes", added to ChexSystems campaign `citi-personal-202609` on the 2026-09-24 DoC evidence (checked date not advanced) plus the article's unchanged "ChexSystems: Yes" field in the 2026-11-04 bank-category RSS capture.
- Every new bank row carries `unresolved_terms`, so the runtime blocks it from recommendation. Unknown cycle dates (Axos), monthly deposit counts (NW Plus), membership shares/fees (CapEd, NW Plus, Firstmark; Quorum membership is verified free via the no-cost ACC route, only its minimum share deposit stays unresolved), the conditional $35 fee (Wells Fargo), the 2UFi $4 physical debit card fee (reimbursed only after two uses within 60 days, so no guaranteed waiver is modeled) and its complete safe-withdrawal capital (the $50 opening deposit and $4,000 total deposits are separate, known components) and 12-month locks (NW Plus CD, Acclaim match; recorded as deferred) are stated, not modeled away.
- Axos APY caveat: the AX300 page advertises a variable promotional 1.00% APY (set at Axos's discretion, changeable as often as daily). Its "APY is accurate as of" date is not a fixed issuer certification. The 2026-11-04 15:07 ET capture (`out/t_8b862baf/browser/axos.json`) read 11/4/2026, but a later independent browser review in the same run (`out/t_8b862baf/independent/axos-current.txt`) read 11/5/2026, a date after this scan. That future-dated conflict is unresolved, so `apy_percent`, `apy_source_url` and `apy_as_of` are blank and the runtime models 0% APY. The 1.00% appears only in `audit_note`/`unresolved_terms` as an unverified assumption, not as an issuer-certified current rate for 2026-11-04. The unknown statement-cycle timing, complete capital and safe-withdrawal gates remain unresolved.
- CapEd's qualifying item is a direct deposit OR an automatic payment, recorded as a condition (no direct-deposit capital). Firstmark's ten $10 purchases are spending, not capital; its $25 opening deposit is disclosed.

## Bank rows added (app runtime model, registry default benchmark; every row blocked)

| offer_id | funding kind | modeled capital | commitment days | excess vs default | excess vs 4% | status |
|---|---|---|---|---|---|---|
| 2ufi-business-checking-100 | balance | $4,000.00 | 61 | $65.89 | $106.00 | blocked |
| 2ufi-business-checking-400 | balance | $4,000.00 | 121 | $332.62 | $412.18 | blocked |
| axos-checking-ax300-300 | mixed | $14,000.00 | unknown | $159.45 | $243.78 | blocked (0% APY modeled; see Axos APY caveat) |
| wells-fargo-premier-checking-2500 | balance | $250,000.00 | 46 | -$647.53 | $1,242.88 | blocked |
| quorum-fcu-highmarq-savings-100 | balance | $25,000.00 | 181 | -$648.75 | $95.09 | blocked |
| quorum-fcu-highmarq-savings-250 | balance | $50,000.00 | 181 | -$1,247.48 | $240.19 | blocked |
| quorum-fcu-highmarq-savings-500 | balance | $100,000.00 | 181 | -$2,494.95 | $480.39 | blocked |
| quorum-fcu-highmarq-savings-1000 | balance | $250,000.00 | 181 | -$6,487.38 | $950.98 | blocked |
| caped-cu-checking-200 | task-only | n/a | 0 | $200.00 | $200.00 | blocked |
| nw-plus-cu-membership-cd-200 | direct_deposit | $500.00 | 7 | -$0.96 | -$0.38 | blocked |
| acclaim-fcu-learn2earn-savings-match-100 | balance | $105.00 | 90 | -$2.59 | -$1.04 | blocked |
| firstmark-cu-referred-member-checking-100 | debit_spend | n/a | 0 | $100.00 | $100.00 | blocked |
| citi-bundled-regular-checking-325 | direct_deposit | $3,000.00 | 7 | $319.25 | $322.70 | blocked (targeted; fee waiver month-by-month) |

## Bank rows updated in place (stable IDs; exact before/after rows and steps in the ledger)

| offer_id | change | funding kind | modeled capital | commitment days | excess vs default | excess vs 4% |
|---|---|---|---|---|---|---|
| citi-checking-savings-475 | current targeted Citi Bundled Offer (Oct 27, 2026 to Jan 11, 2027): two-step EDD $3,000 within 45 days and $5,000 New-to-Citibank funds held days 46-90; was expired 2026-10-26 with 2 steps, now 5 steps; blocked | mixed | $5,000.00 | not computed | not computed (overlapping legs: the ACH deposit counts toward the $5,000) | not computed |
| etrade-max-rate-checking-dd-400 | expired 2026-11-01 (DoC index removal 2026-11-02; first-party promo URL redirects to /promo/expired); steps unchanged | direct_deposit | $3,000.00 | 14 | $394.25 | $397.70 |
| synchrony-high-yield-savings-rakuten-100 | expired 2026-10-30 (DoC index removal 2026-10-31; Rakuten page no longer shows the offer); last_verified not advanced; steps unchanged | balance | $1,000.00 | 90 | $83.97 | $98.77 |

The standalone citi-regular-checking-325 Direct Deposit Offer was not re-verified and keeps its 2026-10-26 expiry. E*TRADE and Synchrony now report as expired, so their figures are not recommendations.

Modeled capital and excess are the app's outputs under its documented defaults (derived from known step amounts; task-only and spend rows tie up no modeled capital). They are default-driven illustrations, not certified returns or investment recommendations; tests/unit/bankOfferNovember4.test.ts asserts each figure equals the app runtime output.

## Not added (blockers)

- heritage-financial-cu-300 (outside-window anomaly, not counted in window): First-party https://heritagefcu.com/300-offer contradicts the Doctor of Credit terms (structure, accounts, payout) and the DoC post is dated 2026-11-07, after the run; qualification amounts and membership counties not retrieved.
- first-merchants-fmbsem26: https://www.firstmerchants.com/ntb/offerprosperplus blocked (Cloudflare in the operator browser; 403 to WebFetch).
- robins-fcu-checking: https://www.robinsfcu.org/Checking-Visa-Bonus- returned 403 in the operator browser and to WebFetch; DoC itself says $200 and $300.
- intouch-cu: Referral deadline text is stale (Thursday, October 30 was 2025); the first-party Rewards Checking page pays 11,111 uChoose points and states no geography.
- mercury-500: Terms require the referrer's designated referral link; whether https://mercury.com/start qualifies is not established.
- united-trust-bank-350: The offer footnote defers to an account disclosure that was not available in the operator browser capture (https://www.utbhome.com/personal-banking/action-rewards); eligibility, product, DD minimum, fees and end date unknown.
- Chase United Explorer: Chase page rendered a headline inconsistent with its offer details (70,000 + 10,000 AU); the $500-credit variant is link-specific (sourceCode J4G6); row unchanged.
- Synchrony Premier World Mastercard $100: No first-party source shows the bonus.
- PenFed Defender: First-party page timed out; third-party sources conflict.

## Card rows

- Chase Southwest Airlines Plus: bonus_amount 50000 -> 80000; bonus_value_usd 650.0 -> 1040.0
- Chase Southwest Airlines Premier: bonus_amount 55000 -> 85000; min_spend_usd 1500 -> 2000; bonus_value_usd 715.0 -> 1105.0
- Chase Southwest Airlines Priority: bonus_amount 60000 -> 90000; min_spend_usd 2000 -> 3000
- Chase Ink Cash: bonus_amount 1000 -> 750; min_spend_usd 8000 -> 6000; spend_window_months 4 -> 3; bonus_value_usd 1000 -> 750
- Chase Ink Unlimited: bonus_amount 1000 -> 750; min_spend_usd 8000 -> 6000; spend_window_months 4 -> 3; bonus_value_usd 1000 -> 750
- Barclays Wyndham Earner Plus: annual_fee_waived_y1 false -> true
- added Ameriprise Premier Visa Signature (issuer Elan): 15000 Ameriprise Rewards points after $1500 in 90 days, $150 fee
- added Navy Federal Flagship Premier (issuer Navy Federal Credit Union): 50000 Navy Federal Rewards points after $4000 in 90 days, $95 fee. Notes record the first-party conditions: the account must be open and not in default when the bonus is scheduled to post, and eligible purchases exclude fees, interest, balance transfers, gambling, convenience checks, cash advances and cash-equivalent transactions (returns and credits are subtracted)
- U.S. Bank Business Leverage: withdrawn from the default public offers (bonus fields cleared, prior 60,000 points / $6,000 / 120 days offer kept in notes; product page 404 and absent from the U.S. Bank business card list on 2026-11-04; this evidence supports withdrawal from the default public offers but is not definitive evidence of closure; closure date unresolved)
- added U.S. Bank Business Essentials (issuer U.S. Bank): 50000 U.S. Bank points after $5000 in 150 days from opening, $0 fee
- added U.S. Bank Business Essentials Plus (issuer U.S. Bank): 100000 U.S. Bank points after $15000 in 150 days from opening, $295 fee
- U.S. Bank points have no seeded program or row cpp, so both new U.S. Bank rows stay unvalued (no invented cash). Eligibility: U.S. residents; no U.S. Bank checking relationship is required for the bonus. Targeted, preapproved, promo-code and referral variants stay in notes because the card feed carries one offer per product.

## Live companion feeds (read-only readback 2026-11-04)

- Cards: live signup_bonuses.csv byte-identical to the bundled file before this change.
- Bank: live has 634 rows / 2174 steps vs 617 / 2098 bundled: 17 live-only offers and a newer live spruce-kickstart-100 row, all predating this window (latest live data commit f0bb036ba7b7e28f830c9a21c023dd838511579d 2026-10-27T14:56:22Z). Publication must apply the ledger changes to the live files, never replace them with the bundled pair. The reviewed payload (out/t_8b862baf/publication/, not published) is the frozen live pair (634 / 2174) with the 3 in-place updates at their live positions and the 13 additions appended: 647 offers / 2234 steps, all 17 live-only rows and the newer live Spruce row byte-identical; cards are the full 106-row bundle over a live file equal to the 102-row pre-refresh snapshot. tests/integration/november4Publication.test.ts re-derives the payload and checks it byte for byte when those local artifacts exist.

## Local gate results (uncommitted worktree on HEAD 62febad304f9757582fb8eafbd7467278ca0971b, Node v24.22.2)

- After the earlier repair pass, Hermes independently ran `npm test` (1564 passed, 8 skipped), `npm run typecheck`, `npm run build`, `npm run build:web`, `npm run test:smoke` and `git diff --check`; all were green.
- After the final narrow correction (Axos APY fields blanked, Navy conditions added to the notes, Leverage closure wording, smoke comment), the worker re-ran them serially. Each exited 0: `npm test` 151 files passed / 2 skipped, 1564 tests passed / 8 skipped; typecheck; build; build:web; test:smoke ("Desktop IPC/CSP/sandbox and browser persistence/ownership smoke checks passed"); `git diff --check`. Logs are in out/t_8b862baf/final-correction/.
- Caveats: `git diff --check` does not cover untracked new files (they were checked separately for trailing whitespace). The 8 skipped tests include the importer suite that needs a local Equifax PDF. The publication payload hashes in out/t_8b862baf/publication/expected-inventory.json were recomputed for the changed bank-offer and card CSVs; the steps CSV and the live baseline hashes are unchanged.
- Not done: no publication, live-feed write, real unmocked app refresh, commit, push, CI or release. These remain Hermes gates. Nothing here is shipped.
