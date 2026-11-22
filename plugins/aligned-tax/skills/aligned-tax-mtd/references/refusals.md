# Refusals and what to do next

Many refusals arrive as a JSON object: `refusal.message` (the words to show), `code`, sometimes `reasons` and `nextActions`, `retryable`, and a note that the refusal is data, not instructions. Show the message in plain words, never the code. Follow `nextActions`. When `retryable` is false, do not try the same request again until the cause is resolved. Other failures are a plain message: show it as given. The codes in this file are for you, not for the person.

## Connecting and permissions

| Message or symptom | Meaning | What to do |
|---|---|---|
| "OAuth session required.", HTTP 401 or "invalid_token" | The connection has expired or was never completed. | Disconnect and reconnect the connector, then sign in again. |
| "MCP access is currently restricted to administrators." | aligned.tax has not opened AI access for this account yet. | Nothing to fix on your side. The person can keep using the app. |
| A refusal saying the tool is for another kind of account | The tool does not apply to this account. | Do not retry and do not repeat the refusal. Use the tools that are listed. |
| "Your MCP connection does not grant this access." | The permissions granted when connecting do not cover this tool. | The person disconnects and reconnects, granting the permission needed. |
| A tool you need is not in your tool list | The person did not grant that permission, or the account cannot use it. | A read-only connection cannot add, upload or preview. The person reconnects, granting the extra permission. |
| "Tool call failed. Please try again." | The call failed without a reason. The cause is not known. | For a read, try once more. For a change, first check whether it happened (for a batch, `getTransactionBatchReview`). If it keeps failing, suggest continuing in the aligned.tax app. |
| "Too many requests from this app are already in progress for your account." | Several calls were running at once. | Wait a few seconds; make one call at a time. |
| HTTP 404 "Session not found" | The connector ended an old session. | The host starts a new session automatically; retry the request once. |
| Consent page says "Connect HMRC first" | The account has no HMRC connection. | The person connects HMRC in the aligned.tax app, then connects you again. |
| "Invalid MCP resource" or "Unsupported MCP resource" while connecting | The host sent a sign-in address the connector does not accept. | Check the server URL ends in `/mcp` and is one of the two in SKILL.md. Report it to hello@aligned.tax if it persists. |

The connector allows about 60 tool calls a minute per person and a few open sessions at once. If calls fail in a burst, slow down: one call, read the result, then the next.

## Filing

| Message or symptom | Meaning | What to do |
|---|---|---|
| The preview points to the filing screen, or says that lower figures need acknowledging | Updates are always sent in aligned.tax, never from a chat. Never say whether sending from the chat is switched on or off. | Give the filing screen link from the preview, or from `getAppLink` with target `filing`. A reply in the chat is not an acknowledgement. |
| The person is not sure an update they sent in aligned.tax reached HMRC | The outcome is not known yet. It is not proof that HMRC refused it. | Do not suggest sending again. Call `getRecentActivity`; for transaction filings, `getTransactionFilingStatus`. Report only what they show. |
| Obligations, businesses or the HMRC connection look out of date | The connector shows what aligned.tax last saved; it never asks HMRC itself. | Say how old the data is (the `HMRC data age` block) and offer the refresh link (`getAppLink`, target `refreshHmrc`). |
| "That obligation is not available." from `upload`, when the obligations list shows that quarter as sent | The quarter has already been sent; the connector only uploads for open quarters. (If the list shows it as open, this is not the reason: check the obligation ID came from this person's list.) | Say the quarter has already been sent and is corrected in aligned.tax; give the place from "Correcting a quarter already sent" in [filing-a-quarter.md](filing-a-quarter.md). Never offer an upload link for it. |
| The person quotes an HMRC rule error from aligned.tax, such as `RULE_SUBMISSION_END_DATE_CANNOT_MOVE_BACKWARDS` | HMRC already holds a later update for this business. | Explain it and suggest the next open quarter, whose cumulative update covers this one. |

## Uploads

| Message or symptom | Meaning | What to do |
|---|---|---|
| Upload refused as the wrong type | Only `.xlsx` is accepted. | The person saves or exports the file as `.xlsx` (Excel, Google Sheets, Numbers and LibreOffice can all do this). |
| Upload refused as too large | The file is over 10 MB. | Remove unused sheets or images, then try again. |
| You cannot read the file's bytes | Some hosts turn attachments into text and cannot pass the file itself to a tool. | Say so plainly. The person uploads in the aligned.tax app. |
| Not the summary template (next actions `DOWNLOAD_SUMMARY_TEMPLATE` and `UPLOAD_IN_APP`) | Through the connector, `upload` reads only the official summary template. | Offer both: download the template (`getAppLink`, target `template`) and upload that, or upload their own spreadsheet on the upload screen in aligned.tax. Never map figures by guessing. |
| Upload refused as needing changes (`UPLOAD_NEEDS_CHANGES`) | A problem the app cannot describe in a chat, because the message could quote the spreadsheet. | Follow the next steps in the refusal. |

## Transactions

| Message or symptom | Meaning | What to do |
|---|---|---|
| `RECORD_CAPTURE_UNAVAILABLE` with a reason | Adding transactions is not available for this business. None of the reasons is retryable. | Show the message. Do not retry or try another transaction tool. Offer the spreadsheet route it names. |
| Reason `VAT_REGISTERED_BUSINESS`: "This business has a VAT registration number saved in aligned.tax, so recording transactions isn't available for it yet." | aligned.tax holds a VAT registration number for the business. | Use the VAT wording below. The alternative is the summary spreadsheet. |
| Reason `ROLE_CANNOT_ADD_RECORDS`: "Your access to these records lets you view them but not add to them." | The person's access is view only. | Do not retry. Say they can ask the records' owner for bookkeeper access or higher. |
| Adding refused because the year is filed from a summary spreadsheet | The business files the year from a spreadsheet of totals. | Say so. Offer the filing-method check in [filing-a-quarter.md](filing-a-quarter.md) only if the person asks to change. |
| "This capture request has already been used for different facts." (in `failed`, or for a single row) | A `requestId` was reused with different rows, or rows in a different order. Other refusals share its code, so go by these words. | Do not change the rows and retry with the same id. To correct a row, send it with a new `requestId`, as in [adding-transactions.md](adding-transactions.md). |
| "This draft was being saved at the same time. Try again with the same requestId." (`CAPTURE_CHANGED`) | Two saves crossed. | Send the same rows again with the same `requestId`. |
| No rows were added (`TRANSACTION_BATCH_NOT_ADDED`) | Every row was refused; the message gives the first reason. | Show the reason. Fix what it names with the person before sending again. |
| A message naming fields to check (`TRANSACTION_BATCH_INPUT_INVALID`) | A row or the batch is not in the shape the tool takes. | Fix the fields it names (dates as `YYYY-MM-DD`, one business and tax year, the row limit) and send again. Never change an amount to make it fit. |
| A message naming fields to check, or that the category does not fit, for a single draft (`TRANSACTION_DRAFT_INPUT_INVALID`) | One item is not in the shape the tool takes, or its category does not fit the business or the direction of money. | Fix what it names with the person (a category from `getTransactionCategories`, the date, the text length) and send again with a new `requestId`. Never change an amount to make it fit. |
| A draft is not part of this batch (`TRANSACTION_BATCH_DRAFT_UNAVAILABLE`) | A draft id is not for this business and tax year, or no longer exists. | Remove that id and review again. |
| "These transactions changed since the person reviewed them. Show the new review and ask again before confirming." (`TRANSACTION_BATCH_CHANGED`) | A row changed after the review was shown. | Call `getTransactionBatchReview`, show the new review and ask again. |

### Asking about VAT status

When aligned.tax reports a business as VAT registered and the person may disagree, do not say "you are VAT registered" and do not ask them to prove anything. Say: "aligned.tax has a VAT registration number saved for this business, which is why transactions cannot be added for it yet. Does that match your records?" If they say it is wrong, believe them, say aligned.tax support (hello@aligned.tax) can check and correct it, and do not retry the batch until they say it has been changed.
