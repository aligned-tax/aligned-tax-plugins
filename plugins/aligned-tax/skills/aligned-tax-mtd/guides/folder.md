# Turning a folder of paperwork into records

Load this after the audience guide when the person gives you several documents (a folder, a set of attachments, "here are my receipts", "sort these out") and wants them turned into records. It is not for one spreadsheet of quarterly totals (that is the upload route in the audience guide) and not for year-end items. If `getGuide` returned a guide for this topic, follow its steps, questions and limits; use this file for how to read documents and for anything it does not cover. The seven rules in SKILL.md always apply, and nothing here sends anything to HMRC.

The order never changes: check, read everything, show one summary, then add drafts in batches, then ask for a yes to each review. Never add anything before the summary has been shown and the person has said yes to adding.

## 1. Before reading anything: five checks

Do these first, so you never read forty documents and then find that nothing can be added.

1. **Whose records.** `getAccessibleTaxpayerContexts`. One run covers one person's records: if the audience guide says to confirm whose records these are, do it now, by name.
2. **Which businesses.** `getTransactionWorkspace` with the `taxpayerId`. Note each business, its type and its id. Foreign property cannot take transactions.
3. **Can records be added.** `getTransactionIngestionCapability` for each business, for the tax year the person is working on now. If it says transactions are not available, give its explanation and next step in plain words and stop for that business; if no business can take them, stop the run here. Never read on and send anyway.
4. **Which years.** Check 3 is per tax year: once the files are read, and before the summary, ask it again for any other tax year the documents' dates fall in. `getUserObligations` gives the quarters and their period dates, for the summary only; an obligation being open or sent does not decide whether a row can be added.
5. **Categories.** `getTransactionCategories` once per business. Use the keys exactly as returned.

Then say what will happen, in your own words: "I'll read everything, show you one summary, and only then add draft transactions to aligned.tax. Nothing is confirmed until you say so, and nothing goes to HMRC from here."

## 2. Read and classify every file

Read every file you have been given. If you cannot open one, list it under "Could not read" with the reason; never guess what it says. If the person attaches files in groups, keep a running list so later files are checked against earlier ones.

Give every file exactly one kind from the table in [references/reading-documents.md](../references/reading-documents.md), then read it by that kind's rules. Fill one line per file as you go, and keep it: file, kind, lines found, rows proposed, statement check result, payment group, state. That table becomes the summary and the run record.

Rules for every row:

- **Date** is the date money moved. An invoice with no sign of payment anywhere is not sent: list it under "Not sent: no payment seen". aligned.tax treats every row it is sent as paid.
- **Amount** is copied exactly, to the penny, as printed. Never calculated, converted, rounded or added up (the one exception, a letting agent's fee and its VAT, is in the splitting reference). A check that printed figures agree, such as the letting statement check, is not a figure you send or quote. If you cannot read an amount exactly, the row is not sent.
- **Money in or out.** `MONEY_IN` or `MONEY_OUT`. If unsure, do not send the row.
- **Description**: what it was, in plain words, starting with the document's own wording, at most 500 characters. No file names.
- **Supplier or customer** (vendor): the other party's name as printed, at most 160 characters.
- **Reference**: the document's own number (invoice, statement or payment reference), at most 200 characters. If it has none, a short tag such as `receipt IMG_0412.jpg`. Never a folder path, never the person's name.
- **Not VAT registered**: the row is the amount paid, including any VAT. A VAT-registered business cannot take transactions yet.
- **Identifiers.** Never copy a full National Insurance number, UTR, tax reference or bank account number from a document into a row, the summary, a question or the run record. An account may be named by its last four digits ("the account ending 1234").

## 3. Split, then remove duplicates

Split documents into rows with the tables in [references/splitting-and-duplicates.md](../references/splitting-and-duplicates.md): a letting agent statement becomes several rows and its net payment never becomes one; an invoice is split only when its lines need different categories. Then build payment groups: one real payment is sent once, however many documents mention it. Using the payment date for each group matters, because aligned.tax's own check for duplicates against existing records looks for the same date.

## 4. Business, tax year and quarter

- **Business.** Property documents go to the UK property business; trading documents to the matching self-employment. With two self-employments, decide by the name or trade on the document; if it could be either, ask once per supplier, not once per document. Foreign property documents are listed as "cannot be added as transactions; that business files from a spreadsheet".
- **Dates** are printed day/month/year in UK documents (05/04/2026 is 5 April 2026). If both numbers could be the month and the other reading would change the tax year, use another date on the same document or ask.
- **Tax year** comes from the row's date: 6 April to 5 April, written 2026-27. A payment on 5 April 2026 is 2025-26; on 6 April 2026 it is 2026-27. Never from today's date. A date printed without a year: look for the year elsewhere in the same document; if there is none, ask.
- **Years aligned.tax will not take.** If check 3 says a tax year cannot take transactions, its rows go under "Not sent" with that reason, before anything is sent. After adding, a row the review says is dated before the years aligned.tax keeps records for, or in another year, is not confirmed: report it with the review's reason.
- **Quarter.** Do not assign quarters: aligned.tax places each row by its date. Updates are cumulative from the start of the year, so a late document for an earlier quarter is simply added and counts in the next update. Never say a quarter must be filed first.

## 5. Every item ends in one of five states

| State | Meaning | Sent? |
|---|---|---|
| Ready | Date, amount, direction, business and category all read from the document with no doubt | Yes |
| Check category | Facts certain, category a best match | Yes, flagged in the summary; the person can change it on the review |
| Needs you | A fact is missing, or the decision is the person's (a share of a joint property or a home bill, a possible capital item, a mixed account, a missing statement, no payment seen, a foreign currency) | No, until answered |
| Not sent | A transaction that cannot be added now: no payment seen, a balance still owed, or a tax year aligned.tax does not keep records for | No, listed with the reason |
| Left out | Not a transaction (payslip, HMRC letter, personal, a duplicate within the folder, a net payment) | No, listed with the reason |

Every "Needs you" item is put to the person as a question, ending in a question mark, not left as a statement: for example "Is the account ending 1234 used for the business, personal, or both?" Ask them in one numbered list after the summary, at most five at a time, then the rest once those are answered. Most valuable first: how an account is used, then statements that do not add up or are missing, then possible capital items, foreign currency and missing years, then home bills. A question that decides several rows is asked once. If the person says "use your judgement", only categories may be chosen for them; dates, amounts and shares still come from the documents or from them (rule 1). Never fill a gap from another document unless it is in the same payment group. Never work out a share, a business proportion or a sterling amount yourself.

## 6. The summary, shown once before anything is added

```
I read 43 files. Here is what I found for your records.

By business and tax year
| Business    | Tax year | Money in rows | Money out rows | Check category |
| UK property | 2026-27  | 6             | 19             | 3              |

What I did with each file
| File        | Kind                    | Result |
| ABC-May.pdf | Letting agent statement | 3 rows; net £1,005.00 matched to the bank line on 29 May, not counted |

Counted once: 12 payments appeared in more than one document
Needs you: 7, listed below
Not sent: 3, being 1 invoice with no payment seen and 2 receipts from earlier tax years
Left out: 9, being 2 payslips, 1 P60, 3 HMRC letters and 3 personal
Could not read: 1, scan0012.jpg, blurred

Nothing has been added to aligned.tax yet. Shall I add these 25 as drafts for you to review?
```

If the audience guide says to name whose records these are, name them in the first line and in the question. Show counts, never money totals: never add up the rows yourself (rule 1). Each row's own amount is shown exactly as printed in the row-level detail, and the totals come from aligned.tax's review once the drafts are added. Offer row-level detail ("ask to see any business in full"), and show it in full when no card will render. For a very long run, show the tables, then only the rows to check.

Before showing it, check: every file has a kind; every letting statement passed or failed its check; no net payment is a row; no payslip, P60 or HMRC letter is a row; every row has a date whose year came from the document.

## 7. Add the drafts in batches

Only after the person says yes to adding.

1. Group rows by business and tax year: one call is one business and one tax year.
2. Freeze the chunks before sending: about 50 rows each (up to 200 is allowed, but large calls are slow and a call that times out leaves an unknown outcome). Same rows, same order, same content.
3. Choose a run id once: `fld-` and eight random letters and digits. Each chunk's `requestId` is the run id and the chunk number: `fld-k3m9x2ab-01`, `fld-k3m9x2ab-02`.
4. Call `addTransactionBatch` for one chunk at a time and wait for each result before the next. Never send calls in parallel.
5. If a call errors or times out, send the identical chunk again with the same `requestId`; it replays safely. Never rebuild a chunk and send it under an id already used. A row corrected later goes in a new chunk with a new `requestId`.
6. Read `failed` after every call (row numbers count from 1 within that chunk) and carry those rows into "Not added", with the reason.
7. The first RECORD_CAPTURE_UNAVAILABLE refusal stops the whole run: none of its reasons is retryable, and every row would be refused the same way.

Before each call, check: chunk frozen; `requestId` new, or an exact retry; one business; one tax year.

## 8. Review and confirm, one business and tax year at a time

1. `getTransactionBatchReview` with every draft id for one business and tax year (up to 200; more than that need several reviews, each shown and approved).
2. Show it as "Cards and text" in SKILL.md says: aligned.tax's totals, the count that need details, and the count that may duplicate transactions already in the records. Ask, naming the business and tax year (and whose records, where the audience guide says to): "Shall I confirm the 22 ready transactions for UK property, 2026-27?"
3. After a clear yes, `confirmTransactionBatch` with the same draft ids, that `reviewToken` and `confirmed` true. If it says the batch changed, show the new review and ask again. If confirming is refused because of the person's access, stop and say the drafts are waiting for someone who can confirm them.
4. Rows held as possible duplicates of existing records are the person's choice, keep both or duplicate, on the card or in aligned.tax. Never decide it for them.
5. Then the next business and tax year. Never one "confirm everything" across reviews the person has not seen.

## 9. Finishing

End with the standard ending in SKILL.md ("Evidence and endings"): "What changed", "Assumptions I made", "Still needs you" and one next step. "Still needs you" is this run's "Needs you" list, not a second one. A bank payment that does not match its documents: [references/matching-payments.md](../references/matching-payments.md). Report exactly from the results: how many were confirmed, how many were held as possible duplicates, how many are still drafts needing details (give the link from `getAppLink`, target `transactions`, to finish them), and everything not sent, with the reason. If you can write files where the documents are, save the run record there as `aligned-tax-run-<run id>.md`: one line per row with its `requestId`, source files, page and line, payment group and state; otherwise show it as a table. Then: "Keep the original documents: HMRC expects records to be kept. aligned.tax cannot store them from this chat yet."

## Turns to avoid

- **Net counted as rent.** The bank shows "ABC LETTINGS £1,005.00" and there is no statement. Sending £1,005.00 as rent is wrong: net is not rent. List it under "Needs you: agent statement missing for this payment".
- **One payment sent twice.** An invoice for £84.00, its receipt and the card line are one payment: one row, dated when paid.
- **A guessed year.** A receipt says "14 May" and nothing else. Do not assume the current year; ask.
