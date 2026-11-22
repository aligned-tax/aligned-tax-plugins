# aligned.tax plugins for Claude

This repository is the plugin marketplace for aligned.tax, HMRC-recognised bridging software for Making Tax Digital for Income Tax in the UK. It is published by Aligned Logic Ltd.

> **Limited release.** The aligned.tax connector is not open to everyone yet. You can install the plugin, but it may not connect to your aligned.tax account until access opens more widely. If it does not connect, keep using the aligned.tax app as normal.

## Plugins

| Plugin | What it does |
|---|---|
| [aligned-tax](plugins/aligned-tax/README.md) | Lets Claude check which quarterly updates are due, what was sent and how old your saved HMRC data is, using the aligned.tax connector. It never contacts HMRC: you send updates in the aligned.tax app. |

## Install

In Claude Code:

```
/plugin install aligned-tax --marketplace aligned-tax/aligned-tax-plugins
```

Or add the marketplace first, then install:

```
claude plugin marketplace add aligned-tax/aligned-tax-plugins
claude plugin install aligned-tax@aligned-tax-plugins
```

On claude.ai, open **Customize > Plugins**, add this repository as a marketplace and install **aligned.tax**.

## Updates

In Claude Code, run `claude plugin update aligned-tax@aligned-tax-plugins`, or turn on auto-update for this marketplace in `/plugin`. Each release is tagged `aligned-tax--v<version>`.

## Support

Help: https://aligned.tax/help. Support: https://aligned.tax/support or hello@aligned.tax. Privacy: https://aligned.tax/privacy.

This repository is built from the aligned.tax source and published by a release script. Changes made here directly are overwritten at the next release.
