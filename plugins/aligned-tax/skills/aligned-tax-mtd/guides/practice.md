# Working for clients

Load this when practice tools such as `listClients` are listed, including for a practice with no clients yet. If `getGuide` returned a guide for this topic, follow its steps, questions and limits; use this file for anything it does not cover. The seven rules in SKILL.md always apply.

## Which client

- If no tax year was named, ask once, then use it throughout.
- `listClients` with the tax year, then confirm the client by name before any change: "I'll work on Jane Smith (sole trader, plumbing). Is that right?" Repeat the client's name in every approval question.
- `getClientDetails` with the `taxpayerId` and tax year shows one client, including their HMRC connection.
- Never use an identifier from another client or from an earlier conversation.

## Roles

A practice member's role limits what they can do, from lowest to highest: Reviewer, Bookkeeper, Preparer, Supporting agent, Main agent, Client owner. Reviewers read; Bookkeepers add records; Preparers confirm and delete; Supporting agents and above send quarterly updates. A Bookkeeper can add transactions but not confirm them: end with "the drafts are waiting for a preparer to confirm". If a tool refuses because of the role, say which action needs a higher role; the practice owner changes roles in the app.

## Jobs

**Who needs me.** `getAccountantRecordsOverview` with the tax year shows each client's quarters and records-request state; `listClients` the client list; `getPracticeAgentWork` current preparation work.

**One client's position.** `getClientDetails`, then `getClientContext` for what aligned.tax has noticed about the client, and `getClientRecordsFiles` for the files already held.

**Add a client's transactions.** Follow [references/adding-transactions.md](../references/adding-transactions.md), with the client named in the review and in the approval question. Adding needs Bookkeeper, confirming Preparer.

**Rent every month, or any entry that repeats.** Follow "Repeating entries" in [references/adding-transactions.md](../references/adding-transactions.md): ask for the day of the month and whether each entry is paid, show the entries with the client's name, and add them only after a clear yes.

**A client's bag of paperwork.** Follow [guides/folder.md](folder.md) for one client per run: confirm the client by name first, and name them in the summary and in every approval question. A Bookkeeper can add the drafts but not confirm them: end with "the drafts are waiting for a preparer to confirm".

**File a client's quarter.** First check the quarter's status with `getClientDetails`, passing the `taxpayerId`, the `selectedTaxYear` and `includeAllObligations` true (without it, sent quarters are left out). `getAppLink` builds an upload link for any quarter, so this check is the only guard. If the quarter is not in the result, stop and ask; never infer its status:
- **Already sent:** never call `upload` and never offer an upload link. Use "Correcting a quarter already sent" in [references/filing-a-quarter.md](../references/filing-a-quarter.md); the place is the client's page, `getAppLink` with target `client`, the `taxpayerId` and tab `obligations`. A Reviewer can see filings but cannot correct them.
- **Open:** `getClientRecordsMode` tells you how the client's business files (use it where [references/filing-a-quarter.md](../references/filing-a-quarter.md) names `getIndividualRecordsMode`). Follow [references/filing-a-quarter.md](../references/filing-a-quarter.md), or give the upload screen link from `getAppLink` (target `upload`, with the `taxpayerId` and `obligationId`). The update is sent on the filing screen in aligned.tax, never from the chat.

**Client authorisation email.** Call `previewClientInvitationEmail` (with `isFirstSend` true for a first invitation; otherwise it is a reminder) and show the email exactly. It is sent only when the person presses Send on the email card for that version: never call `sendClientInvitationEmail` because of a yes typed in the chat. If no card is shown, give the client's page link (`getAppLink`, target `client`). To follow the HMRC invitation afterwards, give the client's page link: the invitation is checked with HMRC there.

**A client's books.** Where Books is on for the client, the same tools as "Your books" in [guides/individual.md](individual.md) answer from the client's saved books, with the client named: `getBooksReports` or `getBooksHome` for profit so far, `listInvoices` for who owes them money, `getBankStatements` and `getBankStatementReview` for statements and what is left to sort, `listContacts`, `listMileageJourneys` and `listBooksDocuments`. `saveInvoiceDraft` and `saveMileageJourney` save drafts only (Bookkeeper or above) and return the app link. Bank numbers arrive masked: never ask for them. Sorting statement lines, issuing and sending invoices and recording payments are done in aligned.tax: give the link from `getAppLink` (targets `books`, `invoices`, `invoiceDraft`, `mileage`, `statements`, `statement`, `reports`, `vatSettings`). Sending VAT returns is not open to the public yet.

**Keep a note.** `recordClientObservation` records something about the client for the practice. Say what you will record first.

**A client's spreadsheet.** `uploadClientRecordsFile` stores an XLSX against one exact client obligation; it does not file it. It needs the file's raw bytes (`fileData`, base64) and exact `fileSize`, and a new `submissionKey` per file.

**Spreadsheets received by email, and preparation work.** `getPracticeEmailIntakeWorkspace` lists held spreadsheets; `resolveSummaryBusinessEvidence` confirms the business and `resolveBusinessEmailIntakeSource` the period, each with the person's explicit decision. `startFindRecords` searches the connected mailbox for XLSX files. `resolvePreparationWork` picks worksheets, `confirmPreparation` confirms matches, `retryPreparation` and `cancelPreparation` deal with stuck work.

**History.** `getPracticeActivity` shows what happened and who did it; `getAuditEvidence` opens the evidence behind one event.

**VAT status.** Ask about it as in [references/refusals.md](../references/refusals.md), about the client by name.

## Walking a client through a quarter

When the person wants help getting a client's quarter in, or is not sure what to add for them, lead with questions rather than waiting to be told. Do this only where transactions are open for the business (step 1 of [references/adding-transactions.md](../references/adding-transactions.md)); otherwise offer the summary template.

1. **See what came before.** `getIncomeSourceUploads` with the client's `taxpayerId` lists earlier updates; `getUploadSummary` on the latest one already sent for the same business gives its figures by category. Updates are year-to-date, so it shows every category used so far this tax year; for the first quarter, use the previous year's last update. If nothing has been sent yet, skip this, walk the common categories instead and say so.
2. **Income first.** "Did Jane Smith's business have any income this quarter?"
3. **Then expenses, a few at a time, starting with what came before.** "Earlier this year Jane Smith had phone, software and insurance. Any of those this quarter?" Then the other common ones for that kind of business, in the plain labels `getTransactionCategories` gives: for a sole trader, usually phone and internet, software and subscriptions, insurance, travel, vehicle costs, materials or stock, accountancy fees and bank charges; for a rental property, repairs, letting agent fees, insurance, mortgage interest, utilities, and ground rent or service charges. An expense from before may not recur: ask, never assume.
4. **Amounts only from the person or the client's records** (rule 1). Never carry an earlier amount forward or estimate one, and never quote an earlier figure as last quarter's spend: it is a year-to-date total.
5. **Add as you go.** Add the answers as rows following [references/adding-transactions.md](../references/adding-transactions.md), then show the review.

**To check and Confirmed.** On the Transactions tabs, To check means saved but not yet reviewed, so it is not counted in the quarterly update. Confirmed means the person has checked the date, amount and category, and it counts towards the quarterly update. Remind them to confirm before filing: aligned.tax never files rows that are not confirmed. Explain it this way whenever they ask what the tabs mean.

## Agents and HMRC

| Agent type | Quarterly updates | Final Declaration |
|---|---|---|
| Main agent | Yes | Yes |
| Supporting agent | Yes | No |

## Add a client

Always give the Add a client link (`getAppLink`, target `addClient`), so the client's National Insurance number is typed in aligned.tax, never in the chat.

## Done in the app because it needs HMRC

The connector never contacts HMRC, on live or test. Client reads show what aligned.tax last saved; say how old it is, as in "How old the HMRC data is" in SKILL.md. Refreshing a client's HMRC data (`getAppLink`, target `refreshHmrc`, with that client's `taxpayerId`), adding a client, following an HMRC invitation, MTD sign-up status, tax calculations, choosing how a business files and sending an update are done in aligned.tax. Do not look for another tool: give the app link.

## Done in the app with Ask Aligned

Forms, questions to a client, the email to a client after filing and the work queue are done in aligned.tax with Ask Aligned. Give the client's page link.

## Practice refusals

| Message or symptom | What to do |
|---|---|
| "This tool is for individual accounts. Use the practice and client tools instead." | Do not retry. Use the practice tools that are listed. |
| "This email is not the version the person approved. Show the new preview and ask again before sending." (`INVITATION_EMAIL_CHANGED`) | Call `previewClientInvitationEmail` again and show the new email; the person sends from the new card. |
| A refusal that mentions a role or permission | Say which action needs a higher role. |
| A client you expected is missing | Use `getAccessibleTaxpayerContexts` to see which clients they can reach. Never use an identifier from somewhere else. |

Everything else: [references/refusals.md](../references/refusals.md).

## Examples

**An ambiguous name.** "Add these for Smith." `listClients` shows two clients called Smith: ask which one, by name and business, before adding anything.

**The authorisation email.** "Send Tom his invitation." Confirm which Tom, call `previewClientInvitationEmail` with `isFirstSend` true, and say the email is on the card for them to check and send. Never say it was sent unless a later result shows it.

**An unknown outcome.** The person pressed Send in aligned.tax and is not sure the update went. Call `getRecentActivity` for that client and report only what it shows; never say it was sent unless a result shows it.
