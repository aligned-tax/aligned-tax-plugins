# Filing a quarter

Every step follows the seven rules in SKILL.md. The connector never sends an update to HMRC, on live or test: you can prepare and preview it, and the person checks the figures, ticks the declaration and presses Send on the filing screen in aligned.tax. A yes typed in the chat never sends an update. The person can also do every step in the app: give the upload screen link from `getAppLink` (target `upload`, with the `taxpayerId` and the `obligationId`).

## From a summary spreadsheet

1. Pick the open obligation with the person and note its ID. Check its status first: if the quarter has already been sent, do not upload and do not offer an upload link; see "Correcting a quarter already sent".
2. Through the connector, `upload` reads only the official aligned.tax summary template for that quarter's business. Give the download link from `getAppLink` (target `template`, with the `taxpayerId` and the `obligationId`). For the person's own spreadsheet layout, give the upload screen link instead: the app matches their labels and shows the result before anything is sent. Foreign property can only use its template. If help text mentions matching your own layout, that is the upload screen in the app, not this connector.
3. If your host can open the file, check it before uploading and tell the person what you find: the tax year on the sheet matches the obligation's; the business on the sheet is the one the obligation belongs to; no figure looks like a placeholder (999, 9,999, 12,345 or the template's sample values); expenses are filled either in detail or as one consolidated total, not both; no cell that should be a number holds text. Do not fix anything yourself. If you cannot open the file, say you have not checked it. "Detailed" or "consolidated" expenses on the template is how expenses are listed; the filing method is separate.
4. `upload` with `obligationId`, `fileName` and `fileBase64` (the file's raw bytes, base64-encoded). XLSX only, 10 MB at most; CSV, XLS, Numbers and Google Sheets files must be saved as `.xlsx` first. Only send a file whose raw bytes your host gives you; otherwise give the upload screen link. The result includes an `uploadId`.
5. `reviewMapping` with the `uploadId` (it only looks, though it needs the write permission). Show which template rows became which HMRC fields. Anything wrong is fixed in the spreadsheet or in the app.
6. Optional: `getUploadSummary` for totals and readiness, `getFilingDecreases` for figures lower than the last update.
7. `previewSubmission` with the `uploadId`. Show every business, period and figure it returns, including lines about figures lower than the last update. If something looks unusual (several categories with the same round figure, expenses far above income), say so as a question, never as a correction. If the period on the obligation has not ended yet, say so plainly.
8. Give the filing screen link from the preview (or `getAppLink` with target `filing`, the `taxpayerId` and the `uploadId`). The person checks the figures there, acknowledges any lower figures, ticks the declaration and sends. Never tell the person whether sending from the chat is switched on or off: it is internal configuration. Say only the next step, to check the figures, tick the declaration and send on the filing screen. A reply in the chat does not acknowledge lower figures.
9. Never say the update was sent unless a later result shows it (`getRecentActivity`); do not promise a receipt or correlation number.

Quarterly updates are cumulative: each covers 6 April to the end of that quarter. A later update also covers an earlier quarter that was missed, so never insist on filing quarters in order.

## Correcting a quarter already sent

Only the latest quarter sent for a business in a tax year can be corrected, using Correct & Resubmit in aligned.tax, never through this connector. Do not call `upload` for it and do not offer an upload link for it.

- **Where:** the Quarterly Statuses page in aligned.tax, where Correct & Resubmit sits beside the latest quarter sent for each business.
- **What to say:** "That quarter has already been sent. To change it, use Correct & Resubmit on that quarter in aligned.tax and send a corrected update." If a later quarter is still to come, add that its update will also carry the corrected figures. For the last quarter of the year, or when no later quarter is due, do not say that.
- **An earlier quarter** (a later one has already been sent) cannot be corrected on its own: the year-to-date figures are corrected by resubmitting the latest quarter sent.
- **Year already finalised:** the figures change only by amending that year's return, which is never done through this connector. While an amendment is open in aligned.tax, the latest quarter sent can still be corrected there.

If `upload` says "That obligation is not available" and the obligations list shows that quarter as sent, this is the reason; say so in plain words.

## From transaction records

`getIndividualRecordsMode` tells you which method a business uses.

1. `getTransactionWorkspace` lists the businesses that can keep transactions.
2. Bring records in as in [adding-transactions.md](adding-transactions.md), or one item at a time with `addTransactionDraft` (a new `requestId` per item). `getTransactionImportBatchStatus` shows when an import has finished.
3. `getTransactionUploadReview` shows the transactions. Use `applyTransactionCorrections` only for decisions the person has just made.
4. `getTransactionBookkeepingReadiness` says READY or names the barrier. Explain it in plain words; most are resolved in the app.
5. `prepareTransactionFiling` builds a fixed cumulative update. Use the identifiers it returns unchanged.
6. `previewSubmission`, then the filing screen link, exactly as for a spreadsheet.
7. If the person is not sure an update they sent went, `getTransactionFilingStatus`. If it is still unclear, say so: aligned.tax checks with HMRC on the filing screen. Never resubmit to find out.

To pick up an earlier preparation, use `resumeTransactionCashbookFiling`. To file the quarter from a spreadsheet instead, `useSpreadsheetForPreparedTransactionFiling` releases the prepared update, with the person's explicit approval.

## Changing how a business files

Do not suggest switching to transactions until you have checked that this business can use them:

1. `getTransactionIngestionCapability` for the business and tax year. If it says transactions are not available, stop: give its explanation and keep the current method.
2. `getQuarterFilingMethod` for the quarter (`getBusinessYearFilingMethod` gives the year's method). Each method says whether it is blocked and why, and the result carries the business's VAT status. Do not use its `sameAsLastTime` value to pick the method; name the method yourself.

Offer the switch only if neither blocks it. A switch step being needed, or no starting figures before the first accepted update, describes the switch rather than blocks it: explain those. If any result mentions VAT, or something blocks the switch, do not suggest it. The person checks and saves the switch in aligned.tax, on that quarter's upload screen; give the upload screen link. Even then, a saved switch is not proof that transactions can be added.

Starting figures: `getFilingOpeningCorrections` shows them; `addFilingOpeningCorrection`, `keepFilingOpeningCorrection` and `voidFilingOpeningCorrection` each need the person's approval of the exact amount, note and source. Amounts are in pence.

## HMRC calculations

Tax calculations come from HMRC, so they are not available through this connector: the person sees them in aligned.tax. If they share one, explain it as HMRC's estimate, not a bill.

## History

Describe submission history in plain words. If a period is not shown, say so; do not infer it. If history and the obligations list seem to disagree, say what each shows and how old the obligations are, and offer the refresh link (`getAppLink`, target `refreshHmrc`): HMRC decides what is filed.
