[RLM_AssetPortfolio_LWC.README.md](https://github.com/user-attachments/files/30841076/RLM_AssetPortfolio_LWC.README.md)
# RLM Account Asset Portfolio (LWC)

A Salesforce Revenue Cloud (RLM) Lightning Web Component for the **Account record
page** that lists the account's **Asset portfolio** — active assets, renewal
dates, and lifecycle status — so a rep can see the installed base at a glance.

> **Target org:** an existing **sandbox / Developer Edition / dev** org with
> Revenue Cloud (RLM) enabled. **Never deploy to production** from this flow.

---

## What's included

```
RLM_AssetPortfolio_LWC/                     # ← project root is NESTED one level
├── sfdx-project.json                       # API 59
└── force-app/main/default/
    ├── classes/RLM_AccountAssetPortfolioService.cls   # @AuraEnabled Asset query surface
    └── lwc/rlmAccountAssetPortfolio/                  # lightning__RecordPage (Account)
```

Deploy mechanism: **source-format SFDX project** → `sf project deploy start`.

> **Layout note:** the SFDX project is **not at the repo root** — it lives in the
> `RLM_AssetPortfolio_LWC/` subfolder. You must `cd` into it before deploying.

## Prerequisites

| Requirement | Check |
|---|---|
| Salesforce CLI (`sf`) 2.x | `sf --version` |
| Target org authenticated + aliased | `sf org display --target-org <alias>` |
| Revenue Cloud (RLM) enabled (Asset object with RLM fields) | — |

## Deploy

```bash
git clone https://github.com/aaronlong78/RLM_AssetPortfolio_LWC.git
cd RLM_AssetPortfolio_LWC/RLM_AssetPortfolio_LWC     # note: nested project folder

sf project deploy start --source-dir force-app --target-org <alias> --dry-run
sf project deploy start --source-dir force-app --target-org <alias>
```

## Post-deploy steps

1. **Add the component to the Account record page.** Lightning App Builder →
   open the Account record page → drag **rlmAccountAssetPortfolio** onto the page
   → Save & Activate.

## Verify

```bash
sf project deploy report --target-org <alias>
```
Open an Account that has Assets → confirm the portfolio component renders rows.

## Known gotchas & limitations

- **No permission set** is included — Apex class access must be granted manually
  (see Post-deploy step 1), or the component fails silently for non-admins.
- **No Apex test class** ships — do not deploy to production (coverage gate).
- Read-only display component; it does not modify Asset data.

## Safety

- Metadata-only deploy — no data is loaded or deleted.
- Review the `--dry-run` output before deploying.
