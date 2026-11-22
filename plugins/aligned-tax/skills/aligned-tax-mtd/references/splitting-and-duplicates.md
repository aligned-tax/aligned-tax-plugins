# Splitting documents and counting each payment once

Used by [guides/folder.md](../guides/folder.md). Tables first: take the first row that fits. Every row follows the seven rules in SKILL.md.

## Letting agent statements

The first case to get right. These rules match aligned.tax's own reader for statements uploaded in the app, so the rows agree with what the app would produce.

| Line on the statement | Row? | Money in or out | Category (a key from `getTransactionCategories`) |
|---|---|---|---|
| Rent received (gross) | Yes | In | Rental income |
| Management fee, letting fee or commission, with its VAT | Yes | Out | The property category for agent or professional fees |
| Repairs, maintenance, contractor | Yes | Out | Repairs and maintenance |
| Insurance, ground rent, service charge, cleaning, gardening, certificates, licence, advertising, legal | Yes, marked "check category" | Out | The best matching category, shown to the person |
| Net payment to the landlord ("paid to you", "balance remitted") | **Never** | | Used only for the check below |
| Totals and subtotals | Never | | |
| Deposit, float, retention, reserve, arrears, "held", "brought forward", "carried forward" | No: "Needs you" | | |
| Rent refunded to a tenant, negative rent | No: "Needs you" | | |

**The check, for every statement before anything is sent.** Gross rent, minus every deduction you turned into a row, must equal the net payment printed, to the penny. If it does not, or the statement has an opening or closing balance, or any line says unpaid or outstanding, do not split it: list the statement under "Needs you", giving the printed gross, deductions and net and saying they do not agree. Do not state a difference you worked out.

**Worked example.**

```
ABC Lettings - Landlord statement, May 2026, 12 Sample Road
28/05/2026  Rent received            1,250.00
28/05/2026  Management fee 10%         125.00
28/05/2026  VAT on fee                  25.00
14/05/2026  Boiler repair, J Smith       95.00
            Net paid to landlord     1,005.00
```

Three rows, not four or five:

| Date | Money in or out | Amount | Description | Supplier | Reference |
|---|---|---|---|---|---|
| 2026-05-28 | In | 1250.00 | Rent received, 12 Sample Road, May 2026 | ABC Lettings | ABC statement May 2026 |
| 2026-05-28 | Out | 150.00 | Management fee 10% including VAT, 12 Sample Road | ABC Lettings | ABC statement May 2026 |
| 2026-05-14 | Out | 95.00 | Boiler repair, J Smith, 12 Sample Road | ABC Lettings | ABC statement May 2026 |

Check: 1,250.00 - 150.00 - 95.00 = 1,005.00. The fee and its VAT are one row because they are one payment for one thing; this is the only place where two printed figures are added, and the summary says so. The bank line "ABC LETTINGS 1,005.00" on 29 May belongs to this statement's payment group and is not a row.

Two rules that prevent the classic errors:

- A bank line from a letting agent **with no statement** is never rent. "Needs you: agent statement missing for this payment". Net is not rent.
- A property a document shows as jointly owned (another owner named): ask once what share the person records. Send nothing for that property until they answer. Never work out a share.

## Invoices and bills with several lines

Take the first row that fits: the rows that hold an invoice back come first.

| Situation | Rows |
|---|---|
| No payment seen anywhere in the folder | Not sent: "no payment seen" |
| A car, land or a building, or an item for use in a rented home (furniture, appliances) | "Needs you: may be a capital item". Never sent as an expense on your own |
| Equipment, tools, a computer or a van for the business | One row: on the cash basis (most sole traders and landlords) it is an expense when paid. If the person says the business uses traditional accounting, "Needs you: may be a capital item" instead |
| Part of the invoice is not for the business | "Needs you": the person says which lines |
| Paid in parts | One row per payment, dated when paid, each with the same invoice number as reference |
| A sales invoice the person issued | Money-in row dated when the money arrived (bank line or "paid" mark), not when issued. If the person says the business uses traditional accounting, ask instead |
| One invoice, paid in one payment, every line the same category and all for the business | One row for the total paid |
| Lines need different categories (materials and tool hire on one merchant's invoice) | One row per printed line, using each line's printed amount (including its VAT when the line shows it); never add lines together into a category total. Check that the rows' amounts agree with the printed total; if they do not, or the lines show VAT only as one total, send one row for the printed total, marked "check category" |

## Payment groups: each payment once

Build the groups before the summary. One real payment is sent once, however many documents mention it.

| Step | Rule |
|---|---|
| 1. Candidates | Two items can be the same payment only if the amount (to the penny), the direction and the business all match. |
| 2. Link | And one of: the same invoice or reference number; or the same other party (allowing for bank abbreviations) with the payment on or after the invoice date and within 45 days; or one is a receipt and the other a card or bank line for the same other party (allowing for bank abbreviations) within 5 days. A link by party and dates alone counts only when neither document carries a different reference and, taking the same date first, there is one closest match on each side. |
| 2a. Not linked | Different invoice or reference numbers, or more than one possible match (two £84.00 invoices from the same supplier and one bank line): do not group. Do not send both of a receipt and a bank or card line that might be one payment: list them under "Needs you" and ask the person which payment went with which. |
| 3. Keep one row | The date from the document that shows money moving (bank or card line first, then the receipt, then a "paid" stamp). Rows split from a letting statement keep the statement's own line dates. The description and reference from the richest document (the invoice first). The supplier's name from the invoice or receipt, not the bank's abbreviation. |
| 4. Record | List every document in the group in the run record, so the person can see why three papers became one row. |
| 5. Doubt | Same amount and party, but nothing links them (two £45.00 fuel receipts in one week): keep both and mark "possible duplicate, kept both". Never drop one on amount alone. |
| 6. Same file twice | Identical files, or two scans of the same receipt, count once. |
| 7. Recurring | Same amount, same party, different months: separate payments. |
| 8. Net agent payment | Always in its statement's group; never a row of its own. |

Why you must do this before sending: aligned.tax's duplicate check compares rows with transactions already in the records, for the same business, and looks for the same date (a few days apart only when the supplier and reference both match). It does not compare rows in the same call with each other. An invoice dated 3 May and its bank payment on 10 May would both be accepted. Using the payment date for each group also lets aligned.tax catch a payment the person already entered by hand.

After adding, the review marks rows that may duplicate transactions already in the records. When confirming, those rows are held for the person to choose keep both or duplicate. Never choose for them.
