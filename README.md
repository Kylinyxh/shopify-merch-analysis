# Shopify Merchandise Analysis Skill

A reusable Skill for Shopify merchandise and assortment analysis.

## What it covers

- Category structure
- Price-band analysis
- Product lifecycle
- Seasonality adjustment
- SKU four-quadrant analysis
- Profit and contribution modeling
- Inventory-risk and forward-coverage analysis
- Clearance, bundling, price-increase, discount-reduction, restock, and exposure decisions
- Excel and PPT output standards
- Auditable SKU category mapping

## Install

The package follows the Agent Skills folder pattern: `SKILL.md` plus optional supporting resources.

### ChatGPT

If Skills are enabled for your account/workspace:
1. Open **Plugins**
2. Open the **Skills** tab
3. Create or upload a skill
4. Upload this ZIP (or the unpacked folder)
5. Review and install

### Codex / compatible Agent Skills runtimes

Install the folder so the runtime can discover its `SKILL.md`.

## Repository structure

```text
shopify-merchandise-analysis-skill/
├── SKILL.md
├── README.md
├── VERSION
├── LICENSE
├── CHANGELOG.md
├── CONTRIBUTING.md
├── references/
├── templates/
├── examples/
└── .github/
```

## Publishing to GitHub

Recommended repository name:

`shopify-merchandise-analysis-skill`

After creating an empty GitHub repository:

```bash
git init
git add .
git commit -m "Initial release: Shopify merchandise analysis skill"
git branch -M main
git remote add origin https://github.com/<YOUR_ACCOUNT>/shopify-merchandise-analysis-skill.git
git push -u origin main
```
