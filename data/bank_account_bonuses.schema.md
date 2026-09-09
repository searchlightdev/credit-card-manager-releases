# Bank bonus feed contract

Live feed: https://raw.githubusercontent.com/searchlightdev/credit-card-manager-releases/main/data/bank_account_bonuses.csv

UTF-8 CSV, one row per offer/tier. Preserve stable offer IDs. All rows must have the same number of fields as the unique header; quote embedded commas, quotes and newlines correctly.

Required columns:

`offer_id,bank_name,account_name,account_type,is_business,bonus_cash_cents,requirements,source_url,offer_url,expiration_date,monthly_fee_cents,fee_waiver_note,eligibility_note,last_verified`

- `offer_id`, `bank_name`, `account_name`, `requirements`: nonempty; IDs unique.
- `account_type`: checking, savings, money_market, cd, brokerage, or other.
- `is_business`: true or false.
- `bonus_cash_cents`: nonnegative safe integer cents, advertised gross cash reward.
- `source_url`: required HTTPS source URL. `offer_url`: HTTPS or blank.
- `expiration_date`: valid YYYY-MM-DD or blank. `last_verified`: required valid YYYY-MM-DD source-check date, not a guarantee of issuer eligibility. Never advance it merely for copied annotations.
- `monthly_fee_cents`: nonnegative safe integer cents or blank.
- `fee_waiver_note`, `eligibility_note`: text or blank. Unknown is not a guarantee of free banking or eligibility.

## Optional funding and hold columns

- `required_funding_cents`: nonnegative safe integer cents or blank/absent for unknown. Explicit total qualifying deposit, balance or cumulative direct-deposit amount for the advertised bonus tier. Do not use fee-waiver balances, double-count overlapping funding/hold requirements, or invent an unspecified minimum. If separate simultaneous funding requirements are not all established, leave blank. Zero is valid but cannot establish ROI.
- `hold_days`: nonnegative safe integer days or blank/absent for unknown. Informational balance-hold duration, with the anchor retained in `requirements`. Funding deadlines, payout delays, debit-spend periods and keep-open/clawback periods are not balance holds. Zero means explicitly no balance hold; blank does not mean zero.

ROI is the one-time gross `bonus_cash_cents / required_funding_cents * 100`, not annualized or APY. Recommendations in supporting clients require strictly greater than 7%, without rounding at the boundary. Hold days never change ROI. Fees, taxes, interest and opportunity cost are not modeled. Missing/zero funding cannot establish ROI. Requirements remain manual prose, not automatically inferred tracking rules.

Legacy feeds may omit both columns. When included, every row includes both fields, even if blank. Older clients can ignore these optional columns; publishing data does not add ROI functionality to clients that lack it.

## Funding annotation verification (2026-09-08)

The initial live feed at `64c7e7d1a6791e7890c929e395f84e7484bfd292` and the application's bundled snapshot at `4772313afe94d47112aeeae5306e8197161e8c1d` had identical values in all original columns for all 24 rows. The bundled snapshot supplied candidate annotations (16 funding, 9 hold values), not proof of current terms. Current issuer pages were checked independently before adopting values:

| Rows | Primary source | Adopted funding / hold |
| --- | --- | --- |
| Wells Fargo Everyday $500 | https://accountoffers.wellsfargo.com/offerbonus/ | $1,000 qualifying electronic deposits; hold blank (90 days is the funding window, not a hold) |
| Chase Savings $200 | https://account.chase.com/consumer/banking/checkingandsavingsoffer | $15,000 new money; 90-day hold from enrollment, preserving the issuer's anchor |
| Capital One $300 / $750 / $1,500 | https://www.capitalone.com/bank/bonus1500/ | $20,000 / $50,000 / $100,000; 90 days after the 15-day initial funding period |
| Wells Fargo business $400 / $550 / $825 | https://accountoffers.wellsfargo.com/business-checking-bonus/ | $2,500 / $10,000 / $25,000; hold left blank rather than translating the day-30/day-60 endpoints into a guessed duration. Five posted transactions also required; qualifying deposits can satisfy these, so no additional spend amount is assumed. |
| Bank of America business $400 / $750 | https://promotions.bankofamerica.com/smallbusiness/biz2toffer | $5,000 / $15,000; 60 calendar days (days 31 through 90 inclusive); account must remain open for payout separately |
| TruStone $250 / $350 / $500 | https://trustonefinancial.org/checking-and-savings/checking-accounts/ | $1,000 / $2,500 / $5,000 direct deposits plus $50 e-statement component; hold blank |

The only changes to original fields are the three TruStone `requirements` strings: `Within 45 days,` becomes `Within 60 days,`. The current issuer's offer disclosure explicitly says 60 days; the linked secondary article retains 45. Every other original value, including every identifier, URL and verification date, is unchanged.

Do not adopt the bundled funding/hold annotations for `bank-of-america-business-1000`, `bank-of-america-business-1500`, or `bank-of-america-business-2500`. Their existing `offer_url` confirms only the $400/$750 tiers. Both higher-tier links from the source article returned the bank's "page ... unavailable" message in a real browser:

- https://promotions.bankofamerica.com/smallbusiness/bizq2offer?promoCode=UA2CIS
- https://promotions.bankofamerica.com/smallbusiness/bizq2offer?promoCode=UA2CIS&source_id=10001SCK0512263&cm_sp=SB-Checking-_-BT019-_-SCE1HZ7C01_Hero_NH_CSBD_0526_CheckingUntargetedCLBOs_Consumer_mastheadCta

A search-index copy of the generic higher-tier page also disagrees with the article about the highest-tier deposit and describes targeting. It is not live verification. Leave both new fields blank for these three rows; do not substitute a different campaign, silently claim the higher tiers are currently verified, or revise their historical source data without verified replacement terms. They cannot produce ROI recommendations in supporting clients.

The other eight unknown-funding rows remain unknown: Chase checking and combined (unspecified checking minimum); U.S. Bank, PSECU and all three Bank of Hawaii tiers (unresolved/incomplete terms); Percapita (spend promotion, not a verified deposit amount). This update does not newly certify those offers. Existing warnings and dates remain intact.

Result: 24 rows, 13 known / 11 unknown funding values, 6 known / 18 unknown hold values. This is intentionally more conservative than the bundled candidate snapshot.

## Refresh and caches

The app persists the validated CSV in `bank_offer_feed_csv`, with a separate `bank_offer_feed_refreshed_at`. Bundled seeding only fills an absent cache; replacing the remote CSV does not itself replace an existing local cache. A successful refresh atomically replaces the CSV and refresh timestamp; failure preserves the last good snapshot.

In the reviewed implementation (`4772313afe94d47112aeeae5306e8197161e8c1d`):

- Desktop checks on startup when bank tracking is enabled and the last successful refresh is absent or at least seven days old. This is a startup check, not a continuously running seven-day timer. A long-running desktop session may keep old data until manual refresh or a later eligible restart.
- Use **Bank Recommendations > Refresh feed** for an immediate fetch, independent of the weekly age check. Web/iOS has this manual path; no equivalent automatic weekly bank refresh was found there.
- Successful mutations invalidate renderer queries centrally, so active recommendations refetch after the feed refresh. No database deletion or reset is needed.
- Fetch uses the configured HTTPS URL without a cache-busting query or `no-store`. The raw endpoint returned `Cache-Control: max-age=300` during verification (five minutes). GitHub raw HTTP/CDN and browser caches can briefly return the old body. Wait for cache expiry and refresh again if necessary; other cache locations may propagate at different times. The publication handoff verifies the unmodified default URL, not just a cache-busted URL.
- A custom configured feed URL continues to use that URL, not this repository.

No app release is needed to retrieve these columns in an already-supporting client. This data-only change does not release or install the separate application implementation.
