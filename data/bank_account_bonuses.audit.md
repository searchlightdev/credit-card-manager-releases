# Bank offer audit — 2026-09-09

All 24 original bundled/live rows were matched by stable ID; original bytes were identical. The JSON companion contains every row, original snapshot, primary URLs, quoted terms, extracted values, provenance, retrieval conflicts and calculation. This report is an audit, not a claim that all campaigns are open or all applicants qualify.

Gross nonannualized return: (bonus + modeled account interest) / cumulative promotional capital. Direct deposits use 0% and seven days each, without recycling. Balance interest is floored to cents after APY compounding over modeled days / 365. Fees, taxes and opportunity cost are excluded. Ancillary opening funds and membership costs are explicitly disclosed, not silently counted twice.

| Offer ID | Capital cents | Bonus cents | Modeled days | Interest cents | ROI % |
|---|---:|---:|---:|---:|---:|
| wells-fargo-everyday-500 | 100000 | 50000 | 7 | 0 | 50.0 |
| chase-total-checking-300 | unresolved | 30000 | 7 | 0 | None |
| chase-savings-200 | 1500000 | 20000 | 61 | 25 | 1.335 |
| chase-checking-savings-900 | unresolved | 90000 | 61 | None | None |
| us-bank-smartly-450 | 800000 | 45000 | 7 | 0 | 5.625 |
| psecu-checking-300 | 100000 | 30000 | 7 | 0 | 30.0 |
| capital-one-performance-savings-300 | 2000000 | 30000 | 91 | 14793 | 2.23965 |
| capital-one-performance-savings-750 | 5000000 | 75000 | 91 | 36983 | 2.23966 |
| capital-one-performance-savings-1500 | 10000000 | 150000 | 91 | 73966 | 2.23966 |
| bank-of-hawaii-savings-75 | 500000 | 7500 | 61 | 8 | 1.5016 |
| bank-of-hawaii-savings-150 | 1000000 | 15000 | 61 | 16 | 1.5016 |
| bank-of-hawaii-savings-300 | 2000000 | 30000 | 61 | 33 | 1.50165 |
| wells-fargo-initiate-business-400 | 250000 | 40000 | 31 | 0 | 16.0 |
| wells-fargo-initiate-business-550 | 1000000 | 55000 | 31 | 0 | 5.5 |
| wells-fargo-initiate-business-825 | 2500000 | 82500 | 31 | 0 | 3.3 |
| bank-of-america-business-400 | 500000 | 40000 | 60 | 0 | 8.0 |
| bank-of-america-business-750 | 1500000 | 75000 | 60 | 0 | 5.0 |
| bank-of-america-business-1000 | 5000000 | 100000 | 60 | 0 | 2.0 |
| bank-of-america-business-1500 | 10000000 | 150000 | 60 | 0 | 1.5 |
| bank-of-america-business-2500 | 25000000 | 250000 | 60 | 0 | 1.0 |
| trustone-checking-250 | 100000 | 25000 | 7 | 0 | 25.0 |
| trustone-checking-350 | 250000 | 35000 | 7 | 0 | 14.0 |
| trustone-checking-500 | 500000 | 50000 | 7 | 0 | 10.0 |
| percapita-checking-300 | unresolved | 30000 | None | None | None |

## Row-by-row decisions and remaining evidence

### wells-fargo-everyday-500

Source: https://accountoffers.wellsfargo.com/offerbonus/

Rate: 0 percent; https://www.wellsfargo.com/checking/alt3/?utm_medium=www-redirect&utm_source=checking--alt3; as of 2026-09-09. DD model remains zero regardless of product rate.

Open through the offer with its bonus code by the expiration date. Receive at least $1,000 in qualifying electronic deposits within 90 days. Account must remain open for payout (within 30 days of meeting requirements). Ordinary transfers and Zelle do not qualify.

Verified $1,000 cumulative qualifying electronic deposits; no fixed deposit count/per-deposit minimum. Model uses promotional DD capital at 0% for seven days each, not bank-imposed hold. Separate $25 opening funding and residual balance to avoid closure may be needed; whether initial qualifying DD can cover opening funding is not established. Keep open through payout. Gross promotional ROI excludes ancillary opening funds/fees.

### chase-total-checking-300

Source: https://account.chase.com/consumer/banking/checkingandsavingsoffer

Rate: not stated percent; not established; as of not established. DD model remains zero regardless of product rate.

Open through the checking/savings offer and receive a qualifying payroll, pension or government-benefit direct deposit within 90 days of coupon enrollment. No minimum direct-deposit amount stated. This row is only the $300 checking component, not the combined $900 reward.

Precise missing denominator: issuer states a qualifying payroll/pension/government direct deposit but no numeric minimum. Actual deposit amount needed; do not invent $1 from micro-deposit exclusion. Seven-day 0% DD model applies once actual amount is supplied.

### chase-savings-200

Source: https://account.chase.com/consumer/banking/checkingandsavingsoffer

Rate: 0.01 percent; https://account.chase.com/consumer/banking/checkingandsavingsoffer; as of 2026-06-26. DD model remains zero regardless of product rate.

Deposit $15,000 in new money directly into savings within 30 days of coupon enrollment and maintain at least $15,000 for 90 days from coupon enrollment. This is only the savings component, not the combined $900 reward.

Primary savings APY 0.01% all balances/states. Conservative latest-funding interpretation: day 30 through day 90 inclusive, withdrawal day 91, 61 modeled days; bank says 90 days from coupon enrollment, NOT 90 days after deposit. Keep account open/unrestricted through payout. Confirm bank cutoff interpretation before funding.

### chase-checking-savings-900

Source: https://account.chase.com/consumer/banking/checkingandsavingsoffer

Rate: 0.01 percent; https://account.chase.com/consumer/banking/checkingandsavingsoffer; as of 2026-06-26. DD model remains zero regardless of product rate.

Requires TWO accounts opened together: qualifying direct deposit into checking within 90 days, plus $15,000 new money into savings within 30 days and held for 90 days from enrollment. Total reward includes $300 checking + $200 savings + $400 combined reward; do not add the component bonuses again. Track both accounts and confirm each requirement manually.

Mixed two-account model: known savings capital $15,000 at 0.01% APY; day-30 funding, conservative day-91 withdrawal (61 days). Additional checking direct-deposit amount has no issuer numeric minimum, so TOTAL cumulative capital and combined ROI remain unavailable, not $15,000 alone. DD leg modeled seven days at 0%; do not double-count component bonuses.

### us-bank-smartly-450

Source: https://www.usbank.com/splash/checking/2026-all-market-checking-offer.html

Rate: 0.001 percent; https://www.usbank.com/splash/checking/2026-all-market-checking-offer.html; as of 2026-05-31. DD model remains zero regardless of product rate.

Complete at least two qualifying payroll or government direct deposits totaling $8,000 within 90 days; enroll in online or mobile banking. Source includes old promo-code and lower-tier text: confirm the CURRENT offer code, opening deposit and complete terms on the bank page.

Exact campaign expired September 8, 2026; not currently open for enrollment. Two deposits totaling $8,000, no per-deposit minimum specified. At least $25 funding within 60 days; can be included in qualifying DD. Product APY 0.001% at this tier; requested DD model deliberately uses 0%, seven days per deposit.

### psecu-checking-300

Source: https://go.psecu.com/promo

Rate: 0.1 percent; https://www.psecu.com/rates; as of 2026-09-09. DD model remains zero regardless of product rate.

Establish qualifying new membership using 2026PROMO by December 31, 2026. Receive two recurring qualifying employer/government/pension direct deposits of at least $500 each into new savings or checking within 100 days of membership establishment. PSECU funds required $5 membership share. Bonus assessment after day 100, payout up to 45 days later; maintain membership/good standing.

Current primary campaign replaces stale 300PROMO and digital-login conditions. Two $500 deposits = $1,000 cumulative user capital. Checking APY 0.10%, savings 0.25%; requested DD model uses 0% and seven days each. Membership route/association cost is applicant-specific and excluded from gross ROI.

### capital-one-performance-savings-300

Source: https://www.capitalone.com/bank/bonus1500/

Rate: 3 percent; https://www.capitalone.com/bank/savings-accounts/online-performance-savings-account/; as of 2026-09-08. DD model remains zero regardless of product rate.

Open 360 Performance Savings with BONUS1500. Deposit $20,000 external new money within first 15 days; hold qualifying balance for 90 days after funding period, through 11:59 pm ET on final hold day. Keep account open/good standing through payout within 60 days afterward. No DD or monthly fee.

Observed official extracted APY 3.00% (variable); live numeric widget showed NaN, so reconfirm current rate before opening. Model assumes observed rate constant. Latest deposit day 15 to first safe withdrawal day 106 = 91 interest days including deposit day; required bank maintenance itself is days 16–105 (90 days). No fixed offer expiration stated.

### capital-one-performance-savings-750

Source: https://www.capitalone.com/bank/bonus1500/

Rate: 3 percent; https://www.capitalone.com/bank/savings-accounts/online-performance-savings-account/; as of 2026-09-08. DD model remains zero regardless of product rate.

Open 360 Performance Savings with BONUS1500. Deposit $50,000 external new money within first 15 days; hold qualifying balance for 90 days after funding period, through 11:59 pm ET on final hold day. Keep account open/good standing through payout within 60 days afterward. No DD or monthly fee.

Observed official extracted APY 3.00% (variable); live numeric widget showed NaN, so reconfirm current rate before opening. Model assumes observed rate constant. Latest deposit day 15 to first safe withdrawal day 106 = 91 interest days including deposit day; required bank maintenance itself is days 16–105 (90 days). No fixed offer expiration stated.

### capital-one-performance-savings-1500

Source: https://www.capitalone.com/bank/bonus1500/

Rate: 3 percent; https://www.capitalone.com/bank/savings-accounts/online-performance-savings-account/; as of 2026-09-08. DD model remains zero regardless of product rate.

Open 360 Performance Savings with BONUS1500. Deposit $100,000 external new money within first 15 days; hold qualifying balance for 90 days after funding period, through 11:59 pm ET on final hold day. Keep account open/good standing through payout within 60 days afterward. No DD or monthly fee.

Observed official extracted APY 3.00% (variable); live numeric widget showed NaN, so reconfirm current rate before opening. Model assumes observed rate constant. Latest deposit day 15 to first safe withdrawal day 106 = 91 interest days including deposit day; required bank maintenance itself is days 16–105 (90 days). No fixed offer expiration stated.

### bank-of-hawaii-savings-75

Source: https://www.boh.com/personal/bank-accounts/pr/savings-offer

Rate: 0.01 percent; https://www.boh.com/siteassets/files/retail-deposit/2026/2026-07-27-consumer-checking-and-savings-rate-sheet-for-state-of-hawaii.pdf; as of 2026-07-27. DD model remains zero regardless of product rate.

Open eligible Regular Savings; $100 opening deposit counts toward tier. Deposit $5,000 within 30 days and maintain qualifying daily balance from day 31 through day 90. Bonus within 120 days of opening; account open and in good standing at award.

Explicit Regular Savings Hawaii model: official 0.01% APY, not higher conditional Bankohana/Bonus Rate rates. Latest deposit day 30 to withdrawal day 91 models 61 interest days; required bank maintenance days 31–90 is 60. No fixed expiry found. Issuer start token 8/4/20206 is malformed and not normalized to a date. Territorial eligibility/product rates must be checked separately.

### bank-of-hawaii-savings-150

Source: https://www.boh.com/personal/bank-accounts/pr/savings-offer

Rate: 0.01 percent; https://www.boh.com/siteassets/files/retail-deposit/2026/2026-07-27-consumer-checking-and-savings-rate-sheet-for-state-of-hawaii.pdf; as of 2026-07-27. DD model remains zero regardless of product rate.

Open eligible Regular Savings; $100 opening deposit counts toward tier. Deposit $10,000 within 30 days and maintain qualifying daily balance from day 31 through day 90. Bonus within 120 days of opening; account open and in good standing at award.

Explicit Regular Savings Hawaii model: official 0.01% APY, not higher conditional Bankohana/Bonus Rate rates. Latest deposit day 30 to withdrawal day 91 models 61 interest days; required bank maintenance days 31–90 is 60. No fixed expiry found. Issuer start token 8/4/20206 is malformed and not normalized to a date. Territorial eligibility/product rates must be checked separately.

### bank-of-hawaii-savings-300

Source: https://www.boh.com/personal/bank-accounts/pr/savings-offer

Rate: 0.01 percent; https://www.boh.com/siteassets/files/retail-deposit/2026/2026-07-27-consumer-checking-and-savings-rate-sheet-for-state-of-hawaii.pdf; as of 2026-07-27. DD model remains zero regardless of product rate.

Open eligible Regular Savings; $100 opening deposit counts toward tier. Deposit $20,000 within 30 days and maintain qualifying daily balance from day 31 through day 90. Bonus within 120 days of opening; account open and in good standing at award.

Explicit Regular Savings Hawaii model: official 0.01% APY, not higher conditional Bankohana/Bonus Rate rates. Latest deposit day 30 to withdrawal day 91 models 61 interest days; required bank maintenance days 31–90 is 60. No fixed expiry found. Issuer start token 8/4/20206 is malformed and not normalized to a date. Territorial eligibility/product rates must be checked separately.

### wells-fargo-initiate-business-400

Source: https://accountoffers.wellsfargo.com/businesscheckinga

Rate: 0 percent; https://www.wellsfargo.com/assets/pdf/about/community-reinvestment/il-business-disclosures.pdf; as of 2026-09-09. DD model remains zero regardless of product rate.

Open Initiate Business Checking with offer code by November 10, 2026. Deposit $2,500 by day 30 and maintain that account balance through day 60. Complete five qualifying posted transactions by day 60 (debit purchases, Bill Pay or qualifying deposits). Minimum opening deposit $25 counts toward balance. Account must stay open/unrestricted through payout within 30 days after qualification.

Primary live campaign supersedes old September expiry. Latest funding day 30 through day 60 inclusive models 31 days; withdrawal day 61, not permission to close. Explicit non-interest-bearing disclosure; rate effective date not stated, checked 2026-09-09. Gross return before fees.

### wells-fargo-initiate-business-550

Source: https://accountoffers.wellsfargo.com/businesscheckinga

Rate: 0 percent; https://www.wellsfargo.com/assets/pdf/about/community-reinvestment/il-business-disclosures.pdf; as of 2026-09-09. DD model remains zero regardless of product rate.

Open Initiate Business Checking with offer code by November 10, 2026. Deposit $10,000 by day 30 and maintain that account balance through day 60. Complete five qualifying posted transactions by day 60 (debit purchases, Bill Pay or qualifying deposits). Minimum opening deposit $25 counts toward balance. Account must stay open/unrestricted through payout within 30 days after qualification.

Primary live campaign supersedes old September expiry. Latest funding day 30 through day 60 inclusive models 31 days; withdrawal day 61, not permission to close. Explicit non-interest-bearing disclosure; rate effective date not stated, checked 2026-09-09. Gross return before fees.

### wells-fargo-initiate-business-825

Source: https://accountoffers.wellsfargo.com/businesscheckinga

Rate: 0 percent; https://www.wellsfargo.com/assets/pdf/about/community-reinvestment/il-business-disclosures.pdf; as of 2026-09-09. DD model remains zero regardless of product rate.

Open Initiate Business Checking with offer code by November 10, 2026. Deposit $25,000 by day 30 and maintain that account balance through day 60. Complete five qualifying posted transactions by day 60 (debit purchases, Bill Pay or qualifying deposits). Minimum opening deposit $25 counts toward balance. Account must stay open/unrestricted through payout within 30 days after qualification.

Primary live campaign supersedes old September expiry. Latest funding day 30 through day 60 inclusive models 31 days; withdrawal day 61, not permission to close. Explicit non-interest-bearing disclosure; rate effective date not stated, checked 2026-09-09. Gross return before fees.

### bank-of-america-business-400

Source: https://promotions.bankofamerica.com/smallbusiness/biz2toffer

Rate: 0 percent; https://www.bankofamerica.com/salesservices/smallbusiness/resources/business-schedule-fees/; as of 2026-09-09. DD model remains zero regardless of product rate.

Open an eligible Business Advantage account through the matching online offer. Deposit $5,000 in new money within 30 days; maintain the qualifying balance from day 31 through day 90. No direct deposit required. Use the appropriate tier link: links and terms vary.

Primary offer and non-interest-bearing account disclosure checked. Explicit modeled interval days 31–90 is 60 days; funding deadline day 30 is separate. Withdrawal day 91 is not account closure; remain open/good standing through payout.

### bank-of-america-business-750

Source: https://promotions.bankofamerica.com/smallbusiness/biz2toffer

Rate: 0 percent; https://www.bankofamerica.com/salesservices/smallbusiness/resources/business-schedule-fees/; as of 2026-09-09. DD model remains zero regardless of product rate.

Open an eligible Business Advantage account through the matching online offer. Deposit $15,000 in new money within 30 days; maintain the qualifying balance from day 31 through day 90. No direct deposit required. Use the appropriate tier link: links and terms vary.

Primary offer and non-interest-bearing account disclosure checked. Explicit modeled interval days 31–90 is 60 days; funding deadline day 30 is separate. Withdrawal day 91 is not account closure; remain open/good standing through payout.

### bank-of-america-business-1000

Source: https://www.doctorofcredit.com/bank-of-america-400-750-business-checking-bonus/

Rate: 0 percent; https://www.bankofamerica.com/salesservices/smallbusiness/resources/business-schedule-fees/; as of 2026-09-09. DD model remains zero regardless of product rate.

Open an eligible Business Advantage account through the matching online offer. Deposit $50,000 in new money within 30 days; maintain the qualifying balance from day 31 through day 90. No direct deposit required. Use the appropriate tier link: links and terms vary.

Conditional calculation from explicitly stated original offer/user tier, NOT verified current enrollment availability. Funding is known from those stated requirements, not unknown. Official account disclosure establishes 0% APY. Current high-tier URL unavailable; alternate extracted targeted campaign expired July 31, 2026, uses enrollment hold anchor and $200,000+ for $2,500, conflicting with the preserved top $250,000 tier. Matching current campaign, expiry and targeting unresolved; do not apply using low-tier link. Modeled days 31–90 = 60 as directed.

### bank-of-america-business-1500

Source: https://www.doctorofcredit.com/bank-of-america-400-750-business-checking-bonus/

Rate: 0 percent; https://www.bankofamerica.com/salesservices/smallbusiness/resources/business-schedule-fees/; as of 2026-09-09. DD model remains zero regardless of product rate.

Open an eligible Business Advantage account through the matching online offer. Deposit $100,000 in new money within 30 days; maintain the qualifying balance from day 31 through day 90. No direct deposit required. Use the appropriate tier link: links and terms vary.

Conditional calculation from explicitly stated original offer/user tier, NOT verified current enrollment availability. Funding is known from those stated requirements, not unknown. Official account disclosure establishes 0% APY. Current high-tier URL unavailable; alternate extracted targeted campaign expired July 31, 2026, uses enrollment hold anchor and $200,000+ for $2,500, conflicting with the preserved top $250,000 tier. Matching current campaign, expiry and targeting unresolved; do not apply using low-tier link. Modeled days 31–90 = 60 as directed.

### bank-of-america-business-2500

Source: https://www.doctorofcredit.com/bank-of-america-400-750-business-checking-bonus/

Rate: 0 percent; https://www.bankofamerica.com/salesservices/smallbusiness/resources/business-schedule-fees/; as of 2026-09-09. DD model remains zero regardless of product rate.

Open an eligible Business Advantage account through the matching online offer. Deposit $250,000 in new money within 30 days; maintain the qualifying balance from day 31 through day 90. No direct deposit required. Use the appropriate tier link: links and terms vary.

Conditional calculation from explicitly stated original offer/user tier, NOT verified current enrollment availability. Funding is known from those stated requirements, not unknown. Official account disclosure establishes 0% APY. Current high-tier URL unavailable; alternate extracted targeted campaign expired July 31, 2026, uses enrollment hold anchor and $200,000+ for $2,500, conflicting with this preserved $250,000 tier. Matching current campaign, expiry and targeting unresolved; do not apply using low-tier link. Modeled days 31–90 = 60 as directed.

### trustone-checking-250

Source: https://trustonefinancial.org/checking-and-savings/checking-accounts/

Rate: 0 percent; https://trustonefinancial.org/checking-and-savings/checking-accounts/; as of 2026-09-09. DD model remains zero regardless of product rate.

Within 60 days, receive $1,000 in direct deposits and enroll in e-statements. Advertised total includes the $50 e-statement reward. A $20 early-closure fee is reported for closure within six months.

Verified cumulative promotional DD amount plus $50 e-statement bonus. Issuer specifies no fixed count or per-deposit minimum. Value Checking official APY None: zero; DD user model seven days each. $25 opening deposit may be separate; overlap with qualifying DD not established, excluded from promotional denominator. Reported $20 early closure fee not confirmed in accessed issuer pages; verify fee schedule.

### trustone-checking-350

Source: https://trustonefinancial.org/checking-and-savings/checking-accounts/

Rate: 0 percent; https://trustonefinancial.org/checking-and-savings/checking-accounts/; as of 2026-09-09. DD model remains zero regardless of product rate.

Within 60 days, receive $2,500 in direct deposits and enroll in e-statements. Advertised total includes the $50 e-statement reward. A $20 early-closure fee is reported for closure within six months.

Verified cumulative promotional DD amount plus $50 e-statement bonus. Issuer specifies no fixed count or per-deposit minimum. Value Checking official APY None: zero; DD user model seven days each. $25 opening deposit may be separate; overlap with qualifying DD not established, excluded from promotional denominator. Reported $20 early closure fee not confirmed in accessed issuer pages; verify fee schedule.

### trustone-checking-500

Source: https://trustonefinancial.org/checking-and-savings/checking-accounts/

Rate: 0 percent; https://trustonefinancial.org/checking-and-savings/checking-accounts/; as of 2026-09-09. DD model remains zero regardless of product rate.

Within 60 days, receive $5,000 in direct deposits and enroll in e-statements. Advertised total includes the $50 e-statement reward. A $20 early-closure fee is reported for closure within six months.

Verified cumulative promotional DD amount plus $50 e-statement bonus. Issuer specifies no fixed count or per-deposit minimum. Value Checking official APY None: zero; DD user model seven days each. $25 opening deposit may be separate; overlap with qualifying DD not established, excluded from promotional denominator. Reported $20 early closure fee not confirmed in accessed issuer pages; verify fee schedule.

### percapita-checking-300

Source: https://www.percapita.com/en/earn300

Rate: 0 percent; https://www.percapita.com/demanddepositaccount/PercapitaDemandDepositAgreement.pdf; as of 2026-03-06. DD model remains zero regardless of product rate.

First 7,000 new customers opening April 16, 2025–December 31, 2026. Earn $25 each calendar month with $300 qualifying debit purchases during first year, up to $300 for 12 months ($3,600 cumulative purchases). Balance must stay positive and account open/active throughout incentive month. Cash substitutes, ATM withdrawals and money movement excluded. Detailed terms allow payout within 10 business days of month end.

Precise unresolved ROI denominator: spend-based promotion, not DD or held-principal funding. $3,600 cumulative purchases consumes capital, plus positive balance required; do not invent a $3,600 balance hold or call purchases DD. Official account rate 0%. First/last partial-month handling and remaining enrollment capacity not stated; full evidence in audit artifact.

## Explicit regression

Bank of America Business Advantage $250,000 tier: 25,000,000 capital cents; 250,000 bonus cents; balance funding by day 30; explicit modeled days 31–90 inclusive = 60; first withdrawal day 91. Official Fundamentals and Relationship account disclosures both state non-interest-bearing: 0% APY. Modeled interest is zero and gross ROI is 1%. Higher-tier current campaign remains unavailable/conflicting, clearly distinguished from this user-stated calculation.

## Deployment compatibility

Publish the expanded live CSV only after the new application ships. Older parsers reject the new mixed/debit_spend enums and retain their previous cache atomically; users must update the app before refresh. No personal database was used for verification.
