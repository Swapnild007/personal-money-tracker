# Product Specification

## 1. Product goal

Create one personal web application that uses detailed bank statements as the source of truth for two connected jobs:

- **Money tracking:** understand where money comes from, where it goes, and how spending changes over time.
- **Tax readiness:** prepare a structured view of potentially relevant income, tax-related transactions and missing tax records so the user can estimate and prepare for ITR filing.

The app is for personal use. Do not add generic social, marketplace or multi-user product features unless explicitly requested.

## 2. Statement import

The import screen must support:

- PDF selection
- PDF password input
- Password visibility toggle
- Password-protected and unprotected PDFs
- Clear incorrect-password error
- Bank/date-range detection
- Original statement preservation where practical
- Import history

The password must not be stored as a financial record.

## 3. Canonical transaction record

Each transaction should retain:

- transaction_date
- narration_raw
- reference_raw
- value_date
- withdrawal_amount
- deposit_amount
- closing_balance
- debit_credit_type
- payment_mode
- merchant_normalized
- category
- subcategory
- confidence
- recurring_flag
- transfer_flag
- refund_flag
- source_statement_id
- source_row/page where available
- notes

### Critical rule

`withdrawal_amount` and `payment_mode` are different fields.

A withdrawal is a bank statement amount field. UPI is a payment method detected from transaction evidence. Never classify every withdrawal as UPI.

## 4. Financial categories

At minimum:

- Salary / Income
- Food & Dining
- Groceries
- Shopping
- Transport
- Rent
- Bills & Utilities
- Loan Payment
- Credit Card Payment
- UPI / Personal Transfer
- Cash Withdrawal
- Entertainment
- Healthcare
- Insurance
- Investments / SIP
- Education
- Tax / Government Payment
- Own Account Transfer
- Refund / Cashback
- Fees / Charges
- Other

Categories can be extended without changing the transaction schema.

## 5. Double-counting rules

### Credit card payment

A credit-card bill payment is a liability settlement, not automatically new consumption spending. Underlying card purchases must be tracked separately when available.

### Loan payment

Loan payment is tracked separately from ordinary spending. Tax relevance depends on loan type and applicable tax rules; the app must not assume the full EMI is deductible.

### Own-account transfer

Transfers between accounts owned by the user are not spending or income.

### Refund / cashback

Refunds and cashback are credits and should be linked to the underlying merchant transaction where possible.

## 6. Monthly analytics

For any uploaded date range, automatically generate calendar-month views.

Each month should show:

- Income
- Genuine spending
- UPI spending
- Cash withdrawals
- Loan payments
- Credit-card payments
- Transfers
- Refunds
- Investments
- Net cash flow
- Average daily spend
- Top categories
- Top merchants

Provide month-over-month change and historical comparisons.

## 7. Merchant intelligence

Normalize variants of the same merchant while retaining the original narration.

Example:

`UPI/SWIGGY/...`, `SWIGGY*...`, `PHONEPE-SWIGGY` → `Swiggy`

Merchant normalization must be evidence-based and reviewable. Never silently merge unrelated merchants.

## 8. Tax / ITR readiness

The tax module should consume the same canonical ledger.

It should identify, where supported:

- Salary credits
- Interest income
- Other identifiable income
- TDS-related information if present
- Home-loan related transactions
- Insurance/investment payments that may be relevant
- Donations or government payments where identifiable
- Capital-gain related transaction candidates where identifiable

It should show:

- Estimated taxable-income inputs
- New vs old regime comparison when applicable
- Estimated tax
- Cess
- TDS/advance tax already known
- Estimated balance payable/refund
- Missing documents/data required for final ITR

Every tax result must be labelled as an **estimate/readiness result** unless supported by authoritative tax records. Do not present a bank-derived estimate as a final tax liability.

## 9. Dashboard

Prefer a clean, light, modern financial UI.

Dashboard sections:

1. Financial health snapshot
2. Monthly spending trend
3. Income vs spending
4. Category distribution
5. Merchant ranking
6. Payment-mode mix
7. Recurring payments
8. Loan and credit-card obligations
9. Tax readiness
10. Data quality / reconciliation alerts

Charts must answer useful questions; avoid decorative charts.

## 10. Quality gates

Before treating an import as valid:

- Verify debit/credit direction from the bank columns.
- Do not use the last numeric value in a line as a generic transaction amount.
- Validate transaction dates.
- Validate amount fields.
- Reconcile closing balances when the statement provides enough information.
- Detect duplicates on overlapping statement imports.
- Preserve ambiguous transactions for review instead of guessing.
- Show import quality metrics.

## 11. Long-term extensibility

The core ledger must not depend on one bank's exact column names. Use a bank adapter/parser layer so additional Indian banks and formats can be supported later.

Keep UI, parsing, financial rules, analytics and tax logic separated.

## 12. Versioning rule

Do not create `v1`, `v2`, `v3`, etc. for normal fixes. Maintain one application and make controlled commits. Only introduce a major version when the product architecture genuinely changes.
