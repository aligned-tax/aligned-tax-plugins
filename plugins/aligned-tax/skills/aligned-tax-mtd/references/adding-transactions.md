# Adding income and expenses as transactions

Use this when the person describes or pastes income and expenses for one business. Every step follows the seven rules in SKILL.md. Nothing here sends anything to HMRC.

## Before adding

0. Call `getTransactionWorkspace` with the `taxpayerId` (it takes no tax year) and pick the business with the person. If the business is unclear (for example rent when there is more than one property business), ask which one; never choose for them. Use its id as `businessIncomeSourceId`, and the same `taxpayerId`, `businessIncomeSourceId` and `taxYear` in every later call.
1. Call `getTransactionIngestionCapability` for that business and tax year. It does not refuse: when transactions are not available it says so, with a reason, an explanation and a next step. Give the explanation and next step in plain words and do not offer to add transactions; offer the summary spreadsheet instead. If the reason is VAT, use the VAT wording in [refusals.md](refusals.md). If the year is filed from a summary spreadsheet, say so; offer the filing-method check in [filing-a-quarter.md](filing-a-quarter.md) only if the person asks to change.
2. Call `getTransactionCategories` for the business and pick a category for each row from that list. Each transaction takes a detailed category: consolidated expenses are a single total used only on the summary spreadsheet, so never offer them for a transaction. Mortgage interest on a rental property is `ukProperty.expenses.residentialFinancialCost`: it is kept out of profit and gets a tax reduction instead.

## Adding the rows

3. One business and one tax year per call. Each row needs its date as `YYYY-MM-DD`, the amount exactly as the person gave it, money in or out, and a description.
4. Check the records for a duplicate before adding ([splitting-and-duplicates.md](splitting-and-duplicates.md)). A payout or payment that does not match the rows: [matching-payments.md](matching-payments.md). If two rows look identical (same date, amount and description), ask whether they are separate payments before sending. Never drop one yourself. Identical rows in one call are not compared with each other in the review, so both count in the totals; when confirming, aligned.tax holds the second as a possible duplicate, and the person chooses keep both or duplicate.
5. First show the exact rows (date, money in or out, amount, description and business) and wait for a yes: rule 2 applies to drafts too. Then call `addTransactionBatch` with the rows: up to 200 in a call, though about 50 return fastest. One call at a time: wait for each result before the next.
6. Give each call a new `requestId`: letters, digits, hyphens or underscores, 8 to 120 characters. Row n is stored under that id and its position, so:
   - if a call fails or its outcome is unknown, send exactly the same rows, in the same order, with the same `requestId`; it replays safely;
   - never reuse a `requestId` with a changed, reordered or shortened list: every row whose position now holds different facts is refused;
   - to correct one row after it was added, send just that row with a new `requestId`, and tell the person the earlier version is still a draft and not in the new review. With their approval, leave the earlier draft out with `leaveOutCaptureDraft`, or correct it.
7. Read `failed` after every call: those rows, numbered from 1, were not added. A result can succeed and still list refused rows. Tell the person about each one; fix a refused row with them, then send only that row under a new `requestId`.
8. If the first refusal is RECORD_CAPTURE_UNAVAILABLE, stop: none of its reasons is retryable and every row would be refused the same way. Show the message and offer the spreadsheet route it names.

## Repeating entries

Use this when the request repeats: rent every month, a weekly payment, a quarterly bill, "paid up to now and the rest not paid". This connector has no repeat option, so you work out the dates, but only from the person's words, and you show every one of them before adding anything. Send them as ordinary rows.

- **Dates come from the person's words.** "From the start of the tax year" is 6 April of the selected tax year and "to the end of the tax year" is 5 April of the next calendar year. A date they write is used as written. Never use a date from today's date or a guess.
- **Ask for the day of the month.** If they gave no day and no written start date, ask which day each entry falls on ("Which day of the month does the rent come in?"). Never assume one, even when an earlier single entry suggests it.
- **A day that does not exist in a month** (the 30th in February, the 31st in April) is held to the last day of that month. Say so when it applies.
- **Paid or not paid.** Ask whether each entry has been paid or received if they did not say. "Paid up to now, the rest not paid" means entries dated up to today are paid and later ones are not. The chat cannot mark a row unpaid: follow "Existing transactions and money not yet received" below instead of sending a payment status.
- **Show before adding, add on one press.** Say the count, the amount, the first and last date and how many are paid and not paid. Add the rows only after their clear yes to that list. Then show the review as usual.
- **Limits.** A series stays inside one tax year, ends no earlier than it starts and is no longer than a weekly series for a whole tax year. If it is too long or crosses the year end, ask the person to split it.

## Existing transactions and money not yet received

To see the transactions already recorded for a business, call `getTransactionUploadReview` with scope `business`. It only reads, despite the group it sits in.

On the cash basis, income that is due but not yet received is not income yet. The chat cannot mark a row unpaid, so say so and point to invoices, or to recording it in aligned.tax when it is paid.

## To check and Confirmed

On the Transactions tabs, To check means saved but not yet reviewed, so it is not counted in the quarterly update. Confirmed means the person has checked the date, amount and category, and it counts towards the quarterly update. Remind them to confirm before filing: aligned.tax never files rows that are not confirmed. Explain it this way whenever they ask what the tabs mean.

## Review and confirm

9. Show the review: every row with its category and, where the business has a claim to choose, its claim and any amount that is not allowable (a business with no claim choice, such as property, has every expense claimed in full: never mention a claim for it); then the totals for income, expenses, profit, anything not allowable and anything not deducted from profit, exactly as the result gives them (see "Cards and text" in SKILL.md). Rows marked as possible duplicates of transactions already in the records are held when confirming; the person chooses keep both or duplicate.
10. Ask for a clear yes to that review. Then call `confirmTransactionBatch` with the same draft ids, the `reviewToken` and `confirmed` set to true. Only rows that are ready are confirmed. If it says the batch changed, call `getTransactionBatchReview`, show the new review and ask again. More than 200 rows for one business and year need several reviews, each shown and approved.
11. End with the standard ending in SKILL.md ("Evidence and endings"). Report from the result: how many were confirmed, how many were held as possible duplicates, and which still need details. Rows that need details stay as drafts; give the link from `getAppLink` (target `transactions`) to finish them in aligned.tax.

## Files

Transaction files are XLSX only; if a result lists other file types, still say XLSX only, and ask for other formats to be saved as `.xlsx` first. Pass a file through yourself only when your host gives you its raw bytes. A bank or bookkeeping export goes through `createTransactionUpload`: run it with `previewOnly` first and show what it found, then run it for real. Otherwise give the Transactions link from `getAppLink`.
