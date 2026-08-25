# Personal Money Tracker

A private, long-term financial intelligence project built for one user.

## Purpose

Combine **expense tracking** and **tax/ITR readiness** from the same financial ledger.

### Core flow

Bank statement PDF + password
→ statement extraction
→ transaction validation/reconciliation
→ transaction classification
→ merchant normalization
→ monthly ledger
→ expense intelligence + tax readiness

## Principles

1. **Accuracy before presentation.** Never guess a financial amount.
2. **Preserve source data.** Keep the original bank row and derived interpretation separately.
3. **Withdrawal is not UPI.** Withdrawal/Deposit are bank amount fields; payment mode is derived separately.
4. **Avoid double counting.** Credit-card payments, loan payments and own-account transfers must not automatically become consumption spending.
5. **Long-term ready.** The data model must support additional banks, cards, UPI exports, CSVs, OCR/scanned statements and future financial sources without redesigning the ledger.
6. **Personal first.** This is not intended to be a generic finance product.
7. **Tax results are estimates unless supported by authoritative tax records.** Bank data can identify useful inputs, but final ITR filing must reconcile with Form 16, AIS, 26AS and other applicable records.

## Planned modules

- Statement Import
- Transaction Ledger
- Monthly Financial View
- Merchant Intelligence
- Category Intelligence
- Recurring Payments
- Loans
- Credit Cards
- Income
- Investments
- Tax / ITR Readiness
- Reconciliation & Data Quality
- Historical comparison

## Current project rule

Do not create endless numbered versions. Build one coherent application and improve it in-place after testing.
