# Bank bonus feed contract

Bundled: `data/bank_account_bonuses.csv`
Default remote: `https://raw.githubusercontent.com/searchlightdev/credit-card-manager-releases/main/data/bank_account_bonuses.csv`

UTF-8 CSV, one row per offer/tier, unique headers and offer IDs, equal row widths;
quote embedded commas, quotes and newlines. Required columns:

`offer_id,bank_name,account_name,account_type,is_business,bonus_cash_cents,requirements,source_url,offer_url,expiration_date,monthly_fee_cents,fee_waiver_note,eligibility_note,last_verified`

- `offer_id`, `bank_name`, `account_name`, `requirements`: nonempty; preserve stable tier IDs.
- `account_type`: checking, savings, money_market, cd, brokerage, or other.
- `is_business`: true or false.
- `bonus_cash_cents`: nonnegative safe integer cents, gross advertised cash reward.
- `source_url`: required HTTPS; `offer_url`: HTTPS or blank.
- `expiration_date`: valid YYYY-MM-DD or blank; `last_verified`: required valid date of source verification, not eligibility certification. Do not advance dates for copied annotations.
- `monthly_fee_cents`: nonnegative safe integer cents or blank.
- `fee_waiver_note`, `eligibility_note`: text or blank. Unknown is not a guarantee of free banking or eligibility.
- `requirements`: source terms, imported as a manual-confirmation requirement. ROI modeling does not create tracking deadlines or mark requirements met.

## Optional ROI columns (backward-compatible)

All optional fields may be blank or absent: unknown is not zero. Included columns
must have a field in every row. Invalid values and unknown headers reject the entire refresh atomically; only the required and optional columns documented here are supported.

- `required_funding_cents`: nonnegative safe integer cents. Total capital required for this exact bonus tier, including **all cumulative required direct deposits**, even if sequential. Two required $500 deposits mean $1,000 capital; never assume recycling the first deposit. Do not substitute fee-waiver balances, double-count overlapping deposit/balance requirements, or invent an unspecified minimum. Combined DD/balance offers with any unknown component remain unknown. Zero is valid data but ROI is undefined without a positive denominator.
- `hold_days`: nonnegative safe integer days. Original informational balance-hold field with its anchor in `requirements`; it is **not** used alone as the modeled interest duration. When an explicit inclusive interval is supplied, it must equal `hold_end_day - hold_start_day + 1`. Enrollment-relative holds, funding windows, payout delays and keep-open conditions are different concepts.
- `funding_kind`: `direct_deposit`, `balance`, `mixed`, or `debit_spend`. The latter two preserve known requirement kinds without pretending that a partial balance or purchase amount supplies the complete ROI denominator. Unknown kinds remain blank.
- `apy_percent`: stated nonnegative finite decimal APY (e.g. `4.56`, not `0.0456`, `4.56%`, exponent notation or a generic current rate guessed for this account). Explicit `0` is distinct from blank. For balance offers, verify the rate applies to the entire required capital for this period/tier. Tiered or variable rates without a verified applicable rate remain unknown. DD modeling always uses the user's explicit **0% APY** assumption, not this field.
- `latest_deposit_day`, `earliest_withdrawal_day`: nonnegative safe integer offsets from **the timeline anchor = day 0** (account opening when absent). The first is the latest allowed qualifying deposit; the second is the first day capital can be withdrawn without jeopardizing the bonus, after all applicable balance conditions. A daily balance required **through day 60** means withdrawal **day 61**, not day 60. A requirement to keep the account open does not by itself require the full capital to remain deposited.
- Alternatively `latest_deposit_date`, `earliest_withdrawal_date`: actual valid YYYY-MM-DD calendar endpoints with the same semantics. Use dates OR opening-day offsets, never both. Reversed windows are invalid. Equal endpoints are valid (zero elapsed days). Missing endpoints stay missing. Do not silently treat coupon enrollment as account opening.

- `hold_start_day`, `hold_end_day`: nonnegative safe integer offsets defining an **inclusive interest interval**. Both must be present together. Start must be at or after `latest_deposit_day` when known; end must not precede start; `earliest_withdrawal_day`, when supplied, must equal end + 1. Unsafe arithmetic and periods over 36,500 days are rejected. Missing funding/withdrawal endpoints remain unknown, not inferred. Do not mix this interval with calendar endpoints.
- `timeline_anchor`: `account_opening`, `coupon_enrollment`, or `membership_establishment`; never infer an anchor from prose. Applies to day offsets and qualifying DD windows; the UI labels the actual anchor.
- `dd_min_deposit_cents`: nonnegative safe integer minimum per deposit; `dd_min_count`: positive safe integer count; `dd_window_days`: nonnegative safe integer qualifying-window days. All may be independently unknown. When minimum and count are supplied their product must be safe, and known required capital cannot be below that product. These fields never fill missing capital or replace the seven-day DD model.
- `apy_source_url`: HTTPS URL or blank; `apy_as_of`: valid YYYY-MM-DD rate date or blank, not automatically the source-check date. Partial provenance remains partial; neither field invents an APY.
- `audit_note`: text or blank, retained in `roi_input_notes` and shown visibly alongside the ROI. Explicit audited fields take precedence over legacy annotations.
- `required_balance_cents`: known balance leg, including mixed offers whose total capital is unresolved; never substituted for the total denominator.
- `opening_deposit_cents`: known user-funded minimum opening amount. Bank-funded membership shares are zero user capital with payer explained in the audit note. Whether opening funds overlap a promotional DD is disclosed, not silently added or recycled.
- `monthly_spend_cents`, `spend_months`: safe nonnegative integers defining known purchase requirements; their product must be safe. Spending consumes capital and does not establish a deposit hold.
- `maintenance_start_day`, `maintenance_end_day`: paired ordered nonnegative safe integers for the bank's inclusive balance-maintenance interval. Separate from the modeled interest interval: Capital One maintenance days 16–105 is 90 days but latest funding day 15 to withdrawal day 106 earns modeled interest for 91 days. BofA uses the explicitly requested 60-day model.

The original row-by-row audit and calculations are in `bank_account_bonuses.audit.json` and
the readable summary in `bank_account_bonuses.audit.md`. That original 24-row audit
has 21 promotional calculations and three precise unresolved denominators (Chase
checking, Chase combined, Percapita). A calculated ROI is not certification of
current enrollment availability. Higher BofA tiers preserve user-stated amounts
with conspicuous current-campaign conflicts; the account's zero APY is verified.
Ancillary opening funds and applicant-specific costs remain disclosed separately
from the cumulative promotional capital model.

The expanded 446-row snapshot and all 294 discovery dispositions are reconciled in
`bank_account_bonuses.inventory.json`. It preserves the original audit as a regression
baseline rather than rewriting its historical counts. First-party evidence determines
new cash tiers, restrictions and known inputs; unresolved source values remain blank
with diagnostics. Calendar endpoint columns already supported above are included in
the expanded CSV header. No additional enum or client parser support is required.

### Calculation and ranking

- DD: every required deposit is capital at 0% APY, locked for seven days per deposit. Cumulative capital is the denominator; interest is zero. Seven days is an explicit model assumption, **not a bank promise** about funds availability or bonus eligibility.
- Balance: with an explicit pair, `days = hold_end_day - hold_start_day + 1`; otherwise `days = earliest_withdrawal - latest_deposit`; interest is `floor(required_funding_cents * ((1 + apy_percent/100)^(days/365) - 1))` cents. Actual UTC elapsed calendar days / 365, APY compounding, rounded **down** to cents. Whole APY years use exact decimal rational compounding to avoid losing a cent through binary floating-point (e.g. 4.56% of 100000 cents is exactly 4560 cents).
- ROI is `(bonus_cash_cents + modeled_interest_cents) / required_funding_cents * 100`, **nonannualized**, gross before fees, taxes and opportunity cost. The threshold is strictly **greater than 7%**, tested using integer cents before display rounding. Invalid supplied terms, periods longer than 36,500 days (100 APY years), or unsafe/nonfinite results fail closed.
- If capital is verified and positive but kind, APY or endpoints are missing, the app may show a **bonus-only lower bound**. Interest remains `null`/unknown, not an invented zero-rate estimate. The nonnegative interest model can only increase gross return, so the lower bound may qualify if bonus alone is strictly above 7%. Missing inputs are listed visibly. At/below 7%, incomplete offers remain in **Needs inputs**; they are not declared definitively ineligible.
- Unknown/zero capital cannot establish ROI. These offers remain visible with precise diagnostics rather than disappearing behind a selection gate.
- Sort personal and business together by descending computable ROI/lower bound, then descending cash bonus, then ascending offer ID (code-point order). Distinguish lower bounds from fully modeled returns.
- Calculation output includes `totalReturnCents` (the bonus-plus-interest ROI numerator, not principal plus earnings; null when ROI is unavailable) and `assumptions` for presentation. For `bonus-only-lower-bound`, this total is only the known bonus, while `interestCents` stays null. The candidate also retains the exact tier bonus, qualifying capital and source date/day endpoints; `lockedDays`, `modeledApyPct`, `roiBasis` and `missingInputs` describe the calculation without guessing missing terms.

For example, BofA funding deadline day30, interest days31–90, withdrawal day91 models **60 interest days**, not the 61 elapsed days from funding deadline to withdrawal. At 0% APY, 250000 bonus cents / 25000000 capital cents is **1%** nonannualized ROI. No funding-day interest is added outside an explicit interval.

## Legacy snapshot and guarded annotations

The original legacy snapshot (before explicit audited feed metadata) had SHA-256
`467162a23fd144df30e9e85a3073b0e7872d4a18c17e384e9e354c91a78eaae6`:
24 rows, 13 known funding amounts, 6 known original hold fields. Its three higher
Bank of America business tiers ($1,000/$1,500/$2,500) had blank funding and hold
fields. These are historical snapshot facts, not constraints on a subsequently
verified audited CSV. New explicit fields supersede the legacy fallback; never
fill unknown amounts from historical prose.

`bank_account_bonus_roi_annotations.json` adds reviewed model metadata only when
**every original CSV field matches exactly**, including requirements, funding,
bonus tier, URLs, hold, fees, eligibility, dates and identifiers. An explicit
nonblank feed model takes precedence. Changed source terms, amounts or dates
invalidate the overlay. No annotation sets a funding amount or APY. This works
for both the bundled seed and unchanged live rows after Refresh feed; the raw
cache remains the received CSV. Both native and web adapters test this behavior.

Verified primary-source provenance (from the companion snapshot review):

| Offers | Primary source | Model metadata |
| --- | --- | --- |
| Wells Fargo Everyday $500 | https://accountoffers.wellsfargo.com/offerbonus/ | Verified $1,000 qualifying electronic deposits; DD model |
| TruStone $250/$350/$500 | https://trustonefinancial.org/checking-and-savings/checking-accounts/ | Verified cumulative DD $1,000/$2,500/$5,000; includes e-statement component. Current issuer completion window is 60 days, not the older article's 45. |
| Wells Fargo business $400/$550/$825 | https://accountoffers.wellsfargo.com/business-checking-bonus/ | Balance funded by opening+30, daily collected balance through day60, first withdrawal day61: 31 modeled days. Five qualifying transactions are separate; qualifying deposits can satisfy these. APY unknown. |
| Bank of America business $400/$750 only | https://promotions.bankofamerica.com/smallbusiness/biz2toffer | Balance funded by day30, maintained days31–90 inclusive, first withdrawal day91: legacy elapsed model 61 days; explicit days31–90 model 60 days. APY unknown in the legacy annotation. |
| Capital One savings $300/$750/$1,500 | https://www.capitalone.com/bank/bonus1500/ | Fund by opening+15; hold 90 days after funding period (days16–105 inclusive); conservative withdrawal day106: 91 modeled days from latest funding. APY unknown. |
| Chase Savings $200 | https://account.chase.com/consumer/banking/checkingandsavingsoffer | Verified balance model. Funding/hold anchored to coupon enrollment, not opening; no opening-relative endpoints or APY asserted. |

The Chase checking and combined rows receive only source-exact diagnostic notes:
the DD amount is unspecified, so the known savings component cannot supply the
combined capital denominator. They remain unknown, not misclassified models.
Other unresolved rows remain unannotated. No source-check dates are advanced.

## Eligibility and tracking

No owner selection is needed to browse. The application internally evaluates all
compatible people/businesses in the optional selected scope. A matching tracked
open/applied account excludes that entity; another eligible entity keeps the
offer in the ranked list. If every compatible entity is excluded, the offer is
blocked. Business identity is authoritative even if an account's stored owner is
null or a former owner. Closed history warns for manual review; no cooldown is
inferred. Expired offers are blocked. Row labels expose eligible entities, not
guarantees about geography, targeting or prior-bonus restrictions.

With no compatible saved entities, browsing still works, but adding requires a
valid person and (for business offers) a business owned by that person. Tracking
revalidates ownership, duplicates, expiration and ROI server-side transactionally.
Personal offers stay personal even when the view includes a selected business.
Filters are optional; missing-input offers are visible by default.

## Cache and compatibility

Seeding only fills an absent persistent cache. Refresh replaces the validated CSV
and timestamp atomically; errors preserve both. Desktop startup checks bank feed
age when bank tracking is enabled; web/iOS uses manual Refresh feed. Successful
mutations invalidate active renderer queries. Existing custom feed URLs remain
unchanged. Legacy feeds with positive funding can establish only a labeled
bonus-only lower bound unless a reviewed exact-source annotation applies.
