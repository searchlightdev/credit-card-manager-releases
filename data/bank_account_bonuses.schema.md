# Bank bonus feed contract

Bundled: `data/bank_account_bonuses.csv` and `data/bank_account_bonus_steps.csv`
Default remote: `https://raw.githubusercontent.com/searchlightdev/credit-card-manager-releases/main/data/bank_account_bonuses.csv`
(the steps file is fetched from the same directory).

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
- `requirements`: the offer's terms as prose. Structured requirements live in the
  steps feed; tracking an offer creates one requirement per step, or a single
  manual-confirmation requirement when the offer has no steps.

## Feed formats and publishing order

A main CSV with a `total_capital_cents` column is the **steps format**: its
requirements and ROI inputs come from `bank_account_bonus_steps.csv`. Without that
column it is the **legacy format**, whose flat ROI columns (below) are still read,
so a cached legacy feed models exactly as before. A steps-format header that also
carries any legacy flat ROI column is rejected.

Refresh fetches the main CSV and, for the steps format, the steps CSV beside it;
both are validated together (no steps for unlisted offers) and both caches plus
the refresh time are replaced in one transaction. A missing or invalid steps file
fails the refresh and keeps the previous pair. A legacy-format refresh clears the
steps cache. Reading the cache never throws on steps whose offer has gone.

Installed apps reject unknown headers, so **release the app before publishing a
feed with new columns** to the companion repo; publish both files in one commit.
Older apps keep their previous cache when a refresh is rejected.

## Requirement steps feed

`data/bank_account_bonus_steps.csv`, one row per step:

`offer_id,step,type,start_day,due_day,due_date,hold_days,hold_until_day,hold_until_date,amount_cents,count,min_each_cents,note`

Only `offer_id`, `step` and `type` headers are required; absent or blank values are
unknown (null), never zero. A header-only file is valid.

- `offer_id`: an offer in the main CSV. `step`: 1..N per offer, no gaps or duplicates.
- `type`: `balance` (maintain a balance), `direct_deposit`, `transactions`
  (debit purchases, bill pay and similar, with a note), `deposit` (one-time funding,
  no hold) or `other` (a dated manual confirmation such as e-statement enrollment;
  its `note` is required and is the requirement).
- Day offsets count from the offer's `timeline_anchor` (account opening when blank).
- `start_day`: when the window opens. A recurring requirement is **several steps**,
  not one: "$500 DD each month for three months" is three `direct_deposit` steps with
  `start_day` 0/31/61 and `due_day` 30/60/90. For `balance` it is the bank's
  maintenance start, which may follow the funding deadline; it never affects ROI.
- `due_day` or `due_date` (not both): the deadline to complete or fund.
- `balance` only: `hold_days` ("hold N days from funding"; tracking only, never ROI)
  or `hold_until_day` / `hold_until_date` (keep the balance **through** this day,
  inclusive; the next day is the first safe withdrawal). A step uses day offsets or
  calendar dates, not both.
- `amount_cents`: the balance or deposit amount, or the cumulative total for direct
  deposits and transactions. `count`: how many deposits or transactions (at least 1).
  `min_each_cents`: per-item minimum; logged items below it don't count, and
  `amount_cents` must be at least `min_each_cents × (count or 1)`.
- `note`: anything the columns don't capture ("employer ACH only; Zelle doesn't count").

Fields that don't apply to a type (a count on a balance, a hold on a deposit,
amounts on `other`) reject the whole file.

## Offer-level columns (steps format)

- `total_capital_cents`: blank derives the ROI capital from steps; a number states
  it (required for mixed or overlapping legs, which are never summed); the literal
  `unknown` refuses to derive it where a step amount isn't the real capital (e.g. an
  offer that also requires keeping existing balances, or a trivial membership share).
- `keep_open_days`: how long the account must stay open to keep the bonus; drives
  the tracked account's "can close after" date. Not a step and not capital.
- `geography_note`: a county, footprint, employer or membership restriction narrower
  than the state rule. See geography below.
- `apy_percent`, `apy_source_url`, `apy_as_of`, `timeline_anchor`,
  `deferred_bonus_cents`, `opening_deposit_cents`, `application_channel`, `audit_note`:
  as described under the flat columns below.

### ROI derived from steps

The ROI model itself is unchanged; its inputs come from steps:

- Funding kind: direct-deposit steps only → `direct_deposit`; one `balance` or
  `deposit` step → `balance`; transactions with amounts only → `debit_spend`; more
  than one capital family or more than one balance/deposit leg → `mixed`; none →
  unknown. Transactions with only a count or minimum are conditions, not capital, so
  a balance plus five purchases is still a balance offer.
- Capital: the stated `total_capital_cents`, else for DD the sum of step amounts
  (when every DD step has one), for balance the leg's amount; mixed, spend and unknown
  kinds have no derived capital.
- DD: count is the sum of step counts (1 each when blank); the recycling model
  divides capital by it. Per-deposit minimum is the step's, or the largest when every
  step states one.
- Balance: committed **from the funding deadline through the last hold day,
  inclusive** (`hold_until_day − due_day + 1` days; with dates, the elapsed days to
  the day after `hold_until_date`). A `deposit` step has no hold, so its withdrawal
  day stays unknown.

`data/bank_account_bonus_steps.migration.json` records how every row got its steps.
They were first drafted mechanically from the frozen 446-row inventory
(`scripts/transcribe-bank-offer-steps.ts`; `drafted_status`), then all 446 rows were
transcribed from their audited terms with `scripts/bank-backfill.ts` (1,093 steps),
reading the first-party page where the terms were ambiguous, and independently
audited for geography and step fidelity. The interest-start rule above moved the five
Bank of America business tiers from 60 to 61 committed days. 46 non-audited rows
changed their modeled ROI, each with a recorded `roi_change_reason` citing the terms —
mostly recurring monthly direct deposits recycled as separate deposits. The 24
originally audited tiers keep their models. `tests/unit/bankOfferStepsMigration.test.ts`
gates the bundled pair against the inventory baseline on exactly those terms.

## Geography columns

`geography` and `geography_states` are additive CSV columns. Legacy feeds without
them remain readable as **unknown**, never nationwide.

| `geography` | `geography_states` | Meaning |
| --- | --- | --- |
| `nationwide` | empty | Verified 50 states + DC. Territories are **needs review**, not automatically included or excluded. |
| `included_states` | nonempty | Complete allowed residence list; a known residence outside it is unavailable. |
| `excluded_states` | nonempty | Complete excluded residence list; all other supported US state/territory codes are allowed. Use only when this entire complement is verified. |
| `unknown` (or absent/empty) | empty | No structured rule established. Review source terms. |

Lists are semicolon-separated canonical uppercase USPS codes, e.g. `NY;NJ;PR`.
Supported residence codes are the 50 states, DC, AS, GU, MP, PR and VI. No full
names, lowercase, surrounding/internal whitespace, empty list items, duplicate
codes, military mail codes or freely associated sovereign-country codes are
accepted. A row has exactly one rule. Included/excluded without a list and
nationwide/unknown with a list are contradictions; the entire refresh is rejected.

No runtime inference from `eligibility_note`, bank footprint, application channel,
online availability, or prose is permitted. A blank rule is never nationwide.

When an offer is limited to part of a state (a county, ZIP, branch radius, employer
or membership field), record the state(s) and put the narrower limit in
`geography_note`. A residence in a listed state is then **eligible with a caveat**:
the server returns `geographyCaveat`, the reason reads "Also required: …", and the
UI shows it beside "Geography confirmed". Every structured rule (121 of 446 rows) has an
entry with its quoted evidence in `bank_account_bonuses.geography.json`; that ledger
is documentation, not a runtime override. Entries marked `verified_first_party` were
read on the first-party page; the rest were transcribed from the audited eligibility
terms. Hedged footprints, branch locations and online availability were left unknown.
All other bundled rows remain unknown until researched.

People may store nullable `residenceState`, edited in People and included in JSON
backups. Older databases/JSON backups migrate or restore with null; unrelated
updates do not erase residence. A missing residence needs review, even for a
nationwide offer. Business offers always need geographic review because business
location is not recorded; personal residence is never used as business location.

The server reports `geographyStatus` (`eligible`, `unavailable`, `needs_review`)
independently of account history. Confirmation is **geography only**, not a claim
that all bank requirements are met. It evaluates each scoped owner, intersects
with duplicate-history eligibility for actionable rows, and returns separately
confirmed `eligibleEntities` and unconfirmed `reviewEntities`. Mixed-owner filters
operate at candidate level. Unknowns remain browsable and manually trackable with
warnings; a known geographic mismatch is rejected again when adding to tracking.
No saved owners still allows browsing with needs-review labels and disabled tracking.

## Legacy flat ROI columns

Read only from legacy-format feeds (no `total_capital_cents`); the steps format
derives the same fields from steps and rejects these headers. All may be blank or
absent: unknown is not zero. Included columns must have a field in every row.
Invalid values and unknown headers reject the entire refresh atomically.

- `required_funding_cents`: nonnegative safe integer cents. Total capital required for this exact bonus tier, including all cumulative required direct deposits. Do not substitute fee-waiver balances, double-count overlapping deposit/balance requirements, or invent an unspecified minimum. Combined DD/balance offers with any unknown component remain unknown. Zero is valid data but ROI is undefined without a positive denominator.
- `hold_days`: nonnegative safe integer days. Original informational balance-hold field with its anchor in `requirements`; it is **not** used alone as the modeled interest duration. When an explicit inclusive interval is supplied, it must equal `hold_end_day - hold_start_day + 1`.
- `funding_kind`: `direct_deposit`, `balance`, `mixed`, or `debit_spend`. Unknown kinds remain blank.
- `latest_deposit_day`, `earliest_withdrawal_day`: nonnegative safe integer offsets from the timeline anchor (day 0). A daily balance required **through day 60** means withdrawal **day 61**. Alternatively `latest_deposit_date`, `earliest_withdrawal_date`: calendar endpoints with the same semantics; never both forms.
- `hold_start_day`, `hold_end_day`: an inclusive interest interval, both or neither; start at or after `latest_deposit_day`; `earliest_withdrawal_day`, when supplied, must equal end + 1.
- `dd_min_deposit_cents`, `dd_min_count` (positive), `dd_window_days`: descriptive DD terms; known required capital cannot be below minimum × count.
- `required_balance_cents`, `monthly_spend_cents` × `spend_months`, `maintenance_start_day`/`maintenance_end_day` (paired, ordered): descriptive legs, never substituted for the capital denominator.

Columns kept in both formats:

- `apy_percent`: stated nonnegative finite decimal APY (e.g. `4.56`, not `0.0456`, `4.56%`, exponent notation or a generic current rate guessed for this account). Explicit `0` is distinct from blank. For balance offers, verify the rate applies to the entire required capital for this period/tier. DD modeling always uses the user's explicit **0% APY** assumption, not this field.
- `timeline_anchor`: `account_opening`, `coupon_enrollment`, or `membership_establishment`; never infer an anchor from prose. Tracking uses the opened date in its place and says so in the requirement's notes.
- `apy_source_url`: HTTPS URL or blank; `apy_as_of`: valid YYYY-MM-DD rate date or blank, not automatically the source-check date.
- `audit_note`: text or blank, retained in `roi_input_notes` and shown visibly alongside the ROI.
- `application_channel`: `online`, `branch_only`, or blank when not established by direct evidence from the offer page.
- `deferred_bonus_cents`: portion of the advertised bonus paid a year or more after opening, stated by the offer's own terms; cannot exceed the advertised bonus. The ROI numerator and recommendation gate use only the immediate portion.
- `opening_deposit_cents`: known user-funded minimum opening amount. Whether opening funds overlap a promotional DD is disclosed, not silently added or recycled.

The original row-by-row audit and calculations are in `bank_account_bonuses.audit.json` and
the readable summary in `bank_account_bonuses.audit.md`. That original 24-row audit
has 21 promotional calculations and three precise unresolved denominators (Chase
checking, Chase combined, Percapita). A calculated ROI is not certification of
current enrollment availability. The expanded 446-row snapshot and all 294 discovery
dispositions are reconciled in `bank_account_bonuses.inventory.json`, which stays
frozen as the historical baseline for the steps transcription.

### Calculation and ranking

- DD: the fewest allowed equal deposits — cumulative capital ÷ required count (1 when unstated) — each locked for seven days at 0% APY, recycling the same funds between deposits (operator instruction 2026-09-09). With the total unknown, a known per-deposit minimum still bounds the capital. Seven days is an explicit model assumption, **not a bank promise**.
- Balance: with an explicit interval, `days = hold_end_day - hold_start_day + 1`; otherwise `days = earliest_withdrawal - latest_deposit`; interest is `floor(capital * ((1 + apy_percent/100)^(days/365) - 1))` cents, rounded **down**. Whole APY years use exact decimal rational compounding.
- ROI is `(immediate bonus + modeled interest) / capital * 100`, gross before fees, taxes and opportunity cost. The recommendation gate annualizes it over the modeled commitment days and requires strictly **greater than 7%**, tested in integer cents; with an unknown duration the nonannualized return is the lower bound. Invalid terms, periods over 36,500 days, or unsafe results fail closed.
- If capital is verified and positive but kind, APY or endpoints are missing, the app may show a **bonus-only lower bound**; interest stays unknown, not zero. Unknown/zero capital cannot establish ROI; those offers stay visible with precise diagnostics.
- Sort personal and business together by descending annualized ROI (or lower bound), then descending cash bonus, then ascending offer ID.

For example, BofA business: fund by day 30, maintain through day 90, first safe withdrawal day 91 → **61 committed days**. At 0% APY, 250000 bonus cents / 25000000 capital cents is **1%** nonannualized ROI.

## Legacy snapshot and guarded annotations

The original legacy snapshot (before explicit audited feed metadata) had SHA-256
`467162a23fd144df30e9e85a3073b0e7872d4a18c17e384e9e354c91a78eaae6`:
24 rows, 13 known funding amounts, 6 known original hold fields. These are historical
snapshot facts; never fill unknown amounts from historical prose.

`bank_account_bonus_roi_annotations.json` adds reviewed model metadata to a
**legacy-format** row only when every original CSV field matches exactly. An
explicit nonblank feed model takes precedence, and changed source terms, amounts or
dates invalidate the overlay. It never applies to the steps format.

## Eligibility and tracking

No owner selection is needed to browse. The application internally evaluates all
compatible people/businesses in the optional selected scope. A tracked open/applied
account **at the same bank and of the same account type** excludes that entity —
a savings account doesn't make you an existing checking customer. Combined offers
(`account_type` other), combined accounts and untyped accounts match any type.
Another eligible entity keeps the offer in the ranked list; if every compatible
entity is excluded, the offer is blocked. Business identity is authoritative even if
an account's stored owner is null or a former owner. Closed history warns for manual
review; no cooldown is inferred. Expired offers are blocked.

With no compatible saved entities, browsing still works, but adding requires a
valid person and (for business offers) a business owned by that person. Tracking
revalidates ownership, duplicates, expiration, ROI and known geographic mismatches
server-side transactionally, links the bonus to its `offer_id`, sets the account's
keep-open days, and creates one requirement per step (see
`docs/bank-bonus-tracking.md`). Personal offers stay personal even when the view
includes a selected business.

## Cache and compatibility

Seeding fills an absent cache with the bundled pair; it never replaces an existing
cache. Desktop startup refreshes a week-old feed when bank tracking is enabled;
web/iOS uses manual Refresh feed. Successful mutations invalidate active renderer
queries. Custom feed URLs need the steps file beside them once they publish the
steps format.
