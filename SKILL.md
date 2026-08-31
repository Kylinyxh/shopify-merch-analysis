---
name: shopify-sku-decision-tool-stateless
description: Rebuild a Shopify SKU merchandise decision system from fresh product, sales, inventory, and profit/cost uploads. Generates HTML, Excel, and PPT. No historical business data is bundled or reused.
---

# Shopify SKU Decision Tool — Stateless Version

## Core rule

This Skill is completely stateless.

It must never bundle, retain, infer, or reuse any business data from previous runs, including:
- product files
- SKU master data
- sales history
- inventory
- profit or cost data
- category mappings
- seasonality indexes
- price-band tables
- warehouse names
- brand-specific rules
- previously generated HTML / Excel / PPT outputs

Every run must rebuild the analysis from the files uploaded in that run.

## Required inputs for every run

### 1. Product file
Recommended fields:
- Product ID
- Product Title
- Variant SKU / SKU
- Product Type
- Product Category
- Current Price
- Compare-at Price
- Image URL
- Created At
- Published At
- Status
- Goods No. if available

### 2. Sales file
Prefer 18–24 months or more of monthly SKU history.

Recommended fields:
- Month
- SKU
- Product Title
- Net Sales
- Net Quantity
- Gross Sales
- Discounts
- Returns

### 3. Inventory file
Required fields:
- SKU
- Warehouse / Location
- Available Inventory / 可用库存

Warehouse inclusion must come from the user's current-run instruction or current-run files/config. Never reuse an old warehouse rule silently.

### 4. Profit / Cost file
Required for margin, contribution profit, and suggested pricing.

Recommended fields:
- SKU
- Unit Cost
- Model Contribution Margin
- optional shipping / fees / commissions

If profit/cost data is missing:
- do not fabricate margin
- do not fabricate contribution profit
- do not issue a margin-based suggested price
- clearly mark unavailable analyses

## Rebuild every run

From current uploads only:

1. Validate current files and fields.
2. Rebuild product master.
3. Rebuild Category / Subcategory mapping from current product information.
4. Mark insufficiently supported classifications as 待分类 / 待补资料.
5. Deduplicate sales rows duplicated by dimensions such as Product Tag.
6. Recalculate trailing 12-month sales.
7. Recalculate prior-year trailing 12-month sales.
8. Recalculate recent 3-month sales and same-period prior-year comparison.
9. Recalculate actual discount rate.
10. Recalculate category seasonality from current sales history.
11. Recalculate category price-band concentration from current sales + price.
12. Recalculate inventory coverage from current inventory.
13. Recalculate profitability from current cost/profit inputs.
14. Recalculate lifecycle.
15. Recalculate SKU four-quadrant.
16. Recalculate final SKU decisions.
17. Generate HTML + Excel + PPT.

## Four-month season judgment

The decision system must always show:
- current month
- next month
- month +2
- month +3

For each month show:
- standardized seasonality index
- 旺季 / 偏旺 / 平季 / 偏淡 / 淡季

Thresholds:
- 旺季 >= 1.20
- 偏旺 1.05–1.19
- 平季 0.90–1.04
- 偏淡 0.75–0.89
- 淡季 < 0.75

Use the current system/browser date dynamically.

A seasonal product must not be cleared solely because the current month is weak.

## Time-based decision logic

Combine:
- current month season phase
- next 3 months direction
- inventory coverage
- recent trend
- profitability
- discount rate
- category price-band position

Typical actions:
- 补货 / 保证供应
- 旺季前准备
- 增加曝光
- 维持 / 观察
- 测试提价 / 降低成本
- 减少实际折扣 / 提高利润
- 捆绑销售 / 降低库存
- 清仓 / 停止补货
- 待分类 / 待补资料

Do not make a final decision from one metric alone.

## Discount rule

Do not use Compare-at Price as proof of actual discounting.

Use:
Actual Discount Rate = - Discounts / Gross Sales

## Price-band rule

Recalculate on every run.

For each category show:
- primary price band
- primary price-band sales share
- second price band
- second price-band sales share

Never reuse an old price-band table.

## Suggested price

Only output **建议测试价** when the current run contains valid:
- current price
- current cost/margin
- sales performance
- actual discount rate
- inventory coverage
- four-month seasonal direction

Use a conservative test range, generally 3%–12%, adjusted by the data.

Do not present the suggestion as a guaranteed final price.

Recommend a 7–14 day test.

## HTML output

Generate one offline single-file HTML tool.

It must support:
- SKU search
- Goods No. search if available
- Product Title search

Each SKU result should show:
- product identity
- category / subcategory
- current price
- combined recommendation
- current month + next 3 months seasonality
- standardized indexes
- future-season advice
- primary category price band and share
- second price band and share
- suggested test price when supported
- trailing 12-month sales
- recent trend / YoY
- model contribution margin if supported
- contribution profit if supported
- actual discount rate
- available inventory
- inventory coverage
- lifecycle
- quadrant
- reasoning

## Excel output

Generate a complete workbook with at least:
- 总览
- 测算时间与定义
- 产品与品类映射
- 品类分析
- 品类价格带
- 四个月季节判断
- 生命周期
- SKU完整分析
- SKU四象限
- 库存风险
- 建议定价
- 决策清单
- 待分类SKU

Every analytical table must show its measurement period.

## PPT output

Do not force a fixed slide count.

Recommended sections:
- 执行摘要
- 完整品类结论表
- 品类销售 × 利润四象限
- 价格带分析
- 四个月季节趋势
- 生命周期分析
- 库存风险
- 建议定价
- SKU决策分布
- 行动清单

Every analytical slide must include:
- measurement period
- clear labels
- written judgment
- conclusion
- action recommendation

Avoid repeating the same conclusion on multiple slides.

## Missing data rule

If any input is missing:
- state exactly what is missing
- continue only with supported analyses
- never silently reuse previous data
- never fill missing business facts from bundled files

## Privacy / package rule

This Skill package may contain only:
- analysis logic
- schemas
- templates
- configuration
- documentation
- generic code

It must contain no real user business data.
