# aligned.tax plugins for Claude

This repository is the plugin marketplace for aligned.tax, HMRC-recognised bridging software for Making Tax Digital for Income Tax in the UK. It is published by Aligned Logic Ltd.

## Plugins

| Plugin | What it does |
|---|---|
| [aligned-tax](plugins/aligned-tax/README.md) | For sole traders, landlords, accountants and bookkeepers: know what is due and when, what was sent to HMRC and how fresh your HMRC data is, and turn receipts and statements into checked draft records. Filing stays in the aligned.tax app, with a preview and your confirmation. |

## Install

In Claude on the web or the desktop app, open **Customize**, then **Plugins**, search for **aligned.tax** and choose **Add**. Then connect **aligned.tax** from the plugin's **Connectors** tab and sign in with your aligned.tax account.

**Using Claude Code?**

```
/plugin install aligned-tax --marketplace aligned-tax/aligned-tax-plugins
```

Or add the marketplace first, then install:

```
claude plugin marketplace add aligned-tax/aligned-tax-plugins
claude plugin install aligned-tax@aligned-tax-plugins
```

## Updates

In Claude Code, run `claude plugin update aligned-tax@aligned-tax-plugins`, or turn on auto-update for this marketplace in `/plugin`. Each release is tagged `aligned-tax--v<version>`.

## Support

Help: https://aligned.tax/help. Support: https://aligned.tax/support or hello@aligned.tax. Privacy: https://aligned.tax/privacy.

This repository is built from the aligned.tax source and published by a release script. Changes made here directly are overwritten at the next release.
