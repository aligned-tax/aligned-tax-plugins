# aligned.tax for Claude

For UK sole traders and landlords on Making Tax Digital for Income Tax, and the accountants and bookkeepers who look after them. Know where you stand, in plain English: what is due, what was sent and what still needs doing. aligned.tax is HMRC-recognised bridging software, and this plugin connects Claude to your aligned.tax account and teaches it to help with your MTD records, safely.

## What it does

- Tells you what is due and when: your businesses, your HMRC connection and every quarterly update, with its dates.
- Shows what was sent to HMRC and when.
- Tells you how fresh your saved HMRC data is, with a one-tap link to refresh it in aligned.tax.
- Turns a folder of receipts, invoices, letting agent statements and bank statements into draft records. Each letting agent statement is checked to add up to the penny before anything is saved, and nothing changes until you say yes to the exact list.
- Works for accountants and bookkeepers too: your client list, each client's records and quarterly updates, with every team member's role respected, from bookkeeper to main agent.
- Answers questions about MTD rules from HMRC guidance, with the page to check, and about how aligned.tax works.
- Filing stays in aligned.tax, where you see a preview and confirm before anything goes to HMRC.

Individuals filing their own MTD Income Tax quarterly updates use aligned.tax free.

## What it never does

- It never contacts HMRC from the chat. Claude reads what aligned.tax last saved from HMRC. Refreshing HMRC data, sending a quarterly update and connecting HMRC all happen in the aligned.tax app in your browser, and Claude gives you the exact link.
- It never invents, estimates or changes your figures. Amounts always come from your own records.
- It never shows your full National Insurance number or UTR. They stay partly hidden.

Year-end Final Declarations, VAT, payroll and company tax are not part of this plugin.

## What is in the plugin

- One skill, `aligned-tax-mtd`, with guides for people filing their own records, for accounting practices, and for turning a folder of paperwork into draft records.
- One connector: the aligned.tax MCP server at `https://mcp.aligned.tax/mcp`. You sign in to it with your aligned.tax account (OAuth) and choose what it may do on the consent page. You can disconnect any time in Settings in aligned.tax.

The plugin runs no code on your computer: no hooks, scripts or local servers. The only service it talks to is the aligned.tax connector above. What you ask about your records goes to aligned.tax over that connection, and aligned.tax handles it under its privacy policy.

## Getting started

1. In Claude on the web or the desktop app, open **Customize**, then **Plugins**, search for **aligned.tax** and choose **Add**.
2. Open the plugin's **Connectors** tab and choose **Connect** next to **aligned.tax**.
3. Sign in with your aligned.tax account, check what Claude may do on the consent page and choose **Authorise access**.
4. Ask something like "Which MTD updates do I have due?"

You need an aligned.tax account. Connect HMRC in aligned.tax so Claude can see your deadlines and what was sent.

**Using Claude Code?** Run `/plugin install aligned-tax --marketplace aligned-tax/aligned-tax-plugins`, then `/mcp` to connect and sign in.

## Support

- Help: https://aligned.tax/help
- Support: https://aligned.tax/support or hello@aligned.tax
- Privacy policy: https://aligned.tax/privacy
- Terms: https://aligned.tax/terms

Published by Aligned Logic Ltd. See LICENSE.
