# aligned.tax for Claude

aligned.tax is HMRC-recognised bridging software for Making Tax Digital for Income Tax in the UK. This plugin connects Claude to your aligned.tax account and teaches Claude how to help with your MTD records safely.

> **Limited release.** The aligned.tax connector is not open to everyone yet. After you install the plugin it may not connect, and Claude will say the connector is unavailable. If that happens, keep using the aligned.tax app as normal. This page will say when the connector opens more widely.

## What it does

- Shows whether aligned.tax is connected to HMRC, your businesses, and which quarterly updates are due and when.
- Shows what was sent to HMRC and when.
- Tells you how old your saved HMRC data is, with a link to refresh it in the aligned.tax app.
- Adds income and expenses as drafts, where your account allows it, and confirms them only after you say yes to the exact list.
- Answers questions about MTD rules from HMRC guidance, with the page to check, and about how aligned.tax works.

## What it never does

- It never contacts HMRC. Claude reads what aligned.tax last saved from HMRC. Refreshing HMRC data, sending a quarterly update and connecting HMRC all happen in the aligned.tax app in your browser, and Claude gives you the exact link.
- It never invents, estimates or changes your figures. Amounts always come from your own records.
- It never shows your National Insurance number, UTR or other references in full. They stay partly hidden.

Year-end Final Declarations, VAT, payroll and company tax are not part of this plugin.

## What is in the plugin

- One skill, `aligned-tax-mtd`, with guides for people filing their own records, for accounting practices, and for turning a folder of paperwork into draft records.
- One connector: the aligned.tax MCP server at `https://mcp.aligned.tax/mcp`. You sign in to it with your aligned.tax account (OAuth) and choose what it may do on the consent page.

The plugin runs no code on your computer: no hooks, scripts or local servers. The only service it talks to is the aligned.tax connector above. What you ask about your records goes to aligned.tax over that connection, and aligned.tax handles it under its privacy policy.

## Getting started

1. Install the plugin. In Claude Code, run:

   ```
   /plugin install aligned-tax --marketplace aligned-tax/aligned-tax-plugins
   ```

   On claude.ai, add it from **Customize > Plugins**.
2. Open the plugin's **Connectors** tab (on claude.ai or in Cowork), or run `/mcp` in Claude Code, and connect **aligned.tax**.
3. Sign in with your aligned.tax account, check the permissions on the consent page and choose **Authorise access**.
4. Ask something like "Which MTD updates do I have due?"

You need an aligned.tax account with HMRC connected.

## Support

- Help: https://aligned.tax/help
- Support: https://aligned.tax/support or hello@aligned.tax
- Privacy policy: https://aligned.tax/privacy
- Terms: https://aligned.tax/terms

Published by Aligned Logic Ltd. See LICENSE.
