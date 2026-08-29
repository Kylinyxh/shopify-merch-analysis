---
name: shopify-sku-decision-tool-updater
description: Update a Shopify SKU decision system from only the latest sales file and inventory file(s). Reuse bundled baseline product/category/seasonality/price-band/profit-model data, recalculate sales and inventory risk, apply a dynamic four-month season window, and output an offline HTML decision tool plus a full Excel workbook and an executive PowerPoint.
---

# Shopify SKU Decision Tool + Report Updater

## Trigger

Use this skill when the user says any equivalent of:
- “更新决策工具”
- “用最新销量和库存更新”
- “重新生成SKU决策工具”
- “更新HTML、Excel和PPT”
- or uploads a sales table plus inventory table(s) and requests refreshed SKU decisions.

## User Input — Lightweight Update Mode

Normally require only:
1. **最新销量表**
2. **最新库存表**（one or multiple files）

Do not ask the user to resend stable product master data unless the baseline is missing fields required for a new SKU or the user explicitly wants to change the baseline.

## Mandatory Outputs

Every successful update must generate **all three**:

1. `SKU_决策查询工具_更新版.html`
2. `SKU_商品经营分析_更新版.xlsx`
3. `SKU_商品经营分析_更新版.pptx`

The HTML is the interactive SKU query tool. The Excel is the complete editable analysis. The PPT is the management/executive summary.

## Baseline Files

Read and reuse:
- `baseline/product_master.csv`
- `baseline/seasonality.csv`
- `baseline/price_bands.csv`
- `config/default_config.json`

The baseline preserves relatively static fields:
- Goods No.
- SKU
- Product Title
- Category
- Subcategory
- Classification Basis
- Product Type
- Shopify Product Category
- Image URL
- Current Price
- Compare-at Price
- Seasonality
- Published At
- Lifecycle
- Model Contribution Margin
- Status

## Update Workflow

1. Detect the newest sales file and inventory file(s) from the current conversation.
2. Read baseline files.
3. Normalize SKU strings and deduplicate duplicated sales rows created by multi-value dimensions such as Product Tag.
4. Recalculate current sales, YoY, recent trend, actual discounts, inventory coverage, quadrants and SKU actions.
5. Use the current date to build the **four-month season window**.
6. Generate a reusable analysis payload / decision dataset.
7. Generate the HTML query tool.
8. Generate the Excel workbook according to `references/excel_output_spec.md`.
9. Generate the PPT according to `references/ppt_output_spec.md`.
10. Return all three files and briefly report:
   - latest complete sales month
   - trailing 12-month period
   - inventory snapshot date
   - current four-month decision window
   - SKU count
   - unknown/new SKU count

## Sales Rules

Calculate at minimum:
- trailing 12-month Net Sales
- prior-year same 12-month Net Sales
- YoY Growth
- recent 3-month Net Sales
- prior-year same recent 3 months
- season-adjusted recent YoY
- Net Quantity
- Gross Sales
- Discounts
- Actual Discount Rate

**Actual Discount Rate = - Discounts / Gross Sales**

Do not use Compare-at Price as proof of actual discounting.

## Inventory Rules

Use the configured **Available Inventory / 可用库存** field and only the warehouse pool defined in `config/default_config.json`.

If inventory is shared across multiple sales channels:
- label it shared available inventory
- use it for stockout risk, overstock risk and cash-tied-up decisions
- do not call it Shopify-exclusive inventory

## Four-Month Season Window — Mandatory

At update time and HTML runtime, identify:
- **M0 = 当前月**
- **M1 = 下月**
- **M2 = 下下月**
- **M3 = 第4个月**

For each SKU and category, display the standardized seasonal index and phase for all four months.

Season phase thresholds:
- **旺季**: index >= 1.20
- **偏旺**: 1.05 <= index < 1.20
- **平季**: 0.90 <= index < 1.05
- **偏淡**: 0.75 <= index < 0.90
- **淡季**: index < 0.75

The standardized seasonal index is relative to the category's own normal monthly level; **1.00 = category monthly average**.

### Time-Aware Decision Rules

Use all four months, not only the latest 3-month sales result.

Examples:
- Current month is off-season but M1–M3 rise sharply into 偏旺/旺季 → mark **旺季前准备** and assess restocking.
- Winter category with low inventory and M1–M3 rising → **优先补货 / 重点关注**.
- Summer category currently strong but M1–M3 fall into 偏淡/淡季 → **控制补货 / 去库存**.
- Seasonal SKU with weak recent sales but a clear upcoming peak → do **not** automatically clear it.
- High inventory through a declining season → prioritize bundle, reduce restocking or clearance depending on profitability and lifecycle.

## Category Price-Band Rules

For every category calculate/show:
- #1 core price band
- #1 core price-band sales share
- #2 price band
- #2 price-band sales share

Use the latest rolling 12-month sales whenever possible. If the current lightweight update cannot reliably rebuild price bands because transaction price information is absent, reuse the baseline price-band structure and clearly label it as baseline/reference price-band structure.

## Decision Logic

Combine:
- sales scale
- contribution margin
- actual discount rate
- recent YoY
- four-month seasonal direction
- shared inventory coverage
- lifecycle
- price-band position

Possible actions include:
- 补货 / 保证供应
- 旺季前重点关注
- 增加曝光
- 放量 / 维持价格
- 维持 / 观察
- 测试提价 / 降低成本
- 减少实际折扣 / 提高利润
- 捆绑销售 / 降低库存
- 清仓 / 停止补货
- 季节性保留 / 旺季后复盘
- 待分类 / 待补资料

## Price Advice

If a SKU qualifies for a price increase, provide:
- Current Price
- Suggested Test Price
- Suggested Increase %
- Pricing Rationale

Use a conservative test increase, normally **3%–12%**, adjusted by:
- contribution margin
- actual discount rate
- recent trend
- four-month seasonal direction
- inventory coverage
- current price-band position

Do not mechanically apply the same percentage to every SKU.

Label the result **建议测试价**, not guaranteed final pricing. Recommend 7–14 day monitoring of conversion, units sold and contribution profit.

## Unknown/New SKU Rule

If the new sales file contains a SKU not in `baseline/product_master.csv`:
- never invent its category
- Category = `待分类`
- Subcategory = `待分类`
- Classification Basis = `新SKU：baseline中不存在`
- include it in HTML and Excel
- surface it in a dedicated “待分类SKU” list
- exclude it from category conclusions until classified

## Excel Output Requirements

Read `references/excel_output_spec.md` and follow it.

The workbook must be editable and include complete data, not only Top-N tables. Every analytical table must show its measurement period.

## PPT Output Requirements

Read `references/ppt_output_spec.md` and follow it.

Do not force the PPT to a fixed slide count. One page should answer one clear management question. Do not repeat the same conclusion across multiple pages.

## HTML Output Requirements

The self-contained offline HTML must support:
- SKU search
- Goods No. search
- Product Title search
- decision-type filters
- complete four-month season cards (M0–M3)
- standardized seasonal index for all four months
- time-aware combined recommendation
- core and second price bands + shares
- suggested test price + increase + rationale
- sales / margin / trend / discount / inventory coverage / lifecycle / quadrant

The HTML must dynamically read the browser's current date so the four-month window automatically moves forward over time.

## Updating the Baseline

Only update the baseline when the user provides new static product information, category mappings, price master, lifecycle dates, profit model, or corrected seasonality/price-band history.

Do not overwrite baseline just because a monthly sales/inventory refresh was supplied.
