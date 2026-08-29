---
name: Shopify-shopify-merch-analysis
description: Analyze Shopify Shopify merchandising performance from sales, product, cost, publication-date, and warehouse inventory files. Use for category structure, price bands, product lifecycle, seasonality, SKU four-quadrant analysis, inventory risk, and actions such as clearance, bundles, price increases, discount reduction, restocking, and exposure.
---

# Shopify Shopify Merchandise Analysis

## Purpose

Use this skill when the user wants a repeatable merchandise-analysis workflow for Shopify US Shopify data.

The skill must answer four questions in order:

1. **定标准 / Structure** — What does the assortment structure look like?
2. **筛商品 / Select SKUs** — Which SKUs are stars, cash cows/problems, potentials, and low-efficiency items?
3. **算贡献 / Contribution** — Which products truly contribute profit, rotate inventory efficiently, or tie up cash?
4. **定策略 / Action** — What should be done next: restock, increase exposure, raise price, reduce discounting, bundle, clearance, or stop restocking?

Do not stop at charts. Every important chart must have a written interpretation and a decision-oriented conclusion.

## Required Inputs

Prefer these files:

1. Shopify monthly SKU sales export
   - Month
   - Product ID
   - Product Title
   - Variant SKU
   - Variant Title
   - Net Quantity
   - Gross Sales
   - Discounts
   - Returns
   - Net Sales

2. Shopify product master export
   - Product ID
   - Product Title
   - SKU
   - Product Type
   - Shopify Product Category
   - Current Price
   - Compare-at Price
   - Image URL
   - Status

3. Product lifecycle export
   - Product ID
   - Product Title
   - SKU
   - Created At
   - Published At
   - Status

4. SKU profit/cost model
   - SKU
   - Unit Cost or current pricing cost model
   - Model profit / model contribution margin when available

5. Warehouse-by-SKU inventory
   - SKU
   - Warehouse
   - 可用库存 / Available Inventory

6. Optional
   - Goods Number / 货品编号
   - Actual shipping cost
   - Advertising spend
   - Payment fees
   - Affiliate commission

## Non-Negotiable Data Rules

### 1. Never double count Shopify sales

Shopify exports may repeat the same sales fact because of multi-value dimensions such as Product Tag.

Before aggregation, check whether the same:
- Month
- SKU
- Product
- Variant
- sales metrics

appear multiple times only because of tag/category expansion.

Deduplicate before calculating totals.

### 2. Always keep a Goods No. + SKU master key when available

The final SKU detail table must begin with:
- Goods No. / 货品编号
- SKU
- Product Title

If Goods No. is absent, preserve the column as blank and document the missing mapping.



### Inventory configuration rule

Inventory must be configurable by store.

Before analysis, define:
- which warehouse(s) belong in the sellable inventory pool
- which inventory field represents true available-to-sell units
- whether the warehouse pool is shared across multiple sales channels

If inventory is shared across channels:
- use it for relative inventory risk, stockout risk, and cash tied up
- do not present it as channel-exclusive inventory
- label inventory coverage as an estimate

### Discount interpretation rule

Do not infer real discounting from Compare-at Price alone.

Historical discount rate should be calculated from transaction data whenever possible:

**Actual Discount Rate = - Discounts / Gross Sales**

If Compare-at Price is known to be unreliable or used only as a merchandising anchor, exclude it from discount analysis and document that choice.


## Category Classification Rules

Category assignment must be auditable.

Priority:
1. Product title and clear product identity
2. Shopify Product Category
3. Product Type
4. Website category structure
5. Product image as a validation layer

If an internal Shopify category conflicts with an unmistakable title, prefer the actual product identity.

Never combine products merely because they are cycling-related.

### Bikes

**Bikes contains complete bicycles only.**

Do not classify these as Bikes:
- Bike Lights
- Inner Tubes
- Tool Accessories
- Bike Trainer
- Cycling Mask
- Bicycle Saddles
- Handlebar Tape
- Bike Wheels / Wheelsets
- Tires
- Pedals
- Racks
- Pumps
- Locks
- Bags

### Required standalone or defensible categories

Prefer explicit categories when identifiable:

- Bikes
- Bike Bags
- Bike Lights
- Tail Lights
- Bike Pumps
- Bike Trainers
- Bike Wheels / Wheelsets
- Tires
- Inner Tubes
- Bicycle Saddles
- Handlebar Tape
- Pedals
- Helmets
- Eyewear
- Gloves
- Cycling Apparel
- Cycling Masks / Face Covers
- Headwear / Balaclavas
- Bike Mirrors
- Bike Locks
- Bike Tools
- Tool Accessories
- Racks & Cargo
- Bottles & Cages
- Fenders
- Bells & Horns
- Cycling Electronics
- Phone Mounts
- Other Accessories only as a last-resort bucket

Do not force a fixed small category count. It is better to add a defensible category than to create a misleading combined category.

For every SKU, retain:
- Category
- Subcategory
- Classification Basis
- Image URL

## Measurement Windows

Every table and chart must show the period used.

Default:
- Core performance period: latest complete 12 months
- Prior comparison: preceding complete 12 months
- Recent trend: latest complete 3 months
- Recent YoY trend: same 3 months one year earlier
- Inventory snapshot: file/report date
- Lifecycle age: as of analysis date
- Seasonality: at least 18 months, preferably 24+ months

Avoid using an incomplete current month in YoY comparisons unless explicitly labeled.

## Stage 1 — Structure / 定标准

### Category structure

For each category calculate:
- Net Sales
- Sales Share
- Net Quantity
- SKU Count
- Estimated Contribution Profit
- Weighted Model Contribution Margin
- Profit Contribution Share
- Available Inventory
- Estimated Inventory Value
- YoY Growth
- Actual Discount Rate

Do not average SKU margins arithmetically.

Correct category margin:

**Category Margin = Sum(Estimated Contribution Profit) / Sum(Net Sales with valid margin model)**

### Price-band structure

Create price bands appropriate to the category.

Default cross-category bands:
- < $20
- $20–29.99
- $30–49.99
- $50–79.99
- $80–119.99
- $120–199.99
- $200+

For each Category × Price Band calculate:
- SKU Count
- Net Sales
- Share within Category
- Net Quantity
- Estimated Contribution Profit
- Weighted Model Margin

Identify:
- dominant price band
- high-profit price band
- overcrowded low-sales price band
- price gaps

### Lifecycle structure

Use Published At first.
If Published At is missing, use first-sale month only as an explicitly labeled estimate.

Suggested stages:
- 0–90 days: New / introduction
- 91–180 days: Validation
- 181–365 days: Growth
- 1–2 years: Mature
- 2+ years: Legacy

Do not label old products as declining based on age alone.

## Seasonality Treatment

Seasonality must be explicitly modeled for seasonal products such as:
- Cycling Apparel
- Gloves
- Headwear
- Balaclavas
- Cycling Masks
- thermal/winter accessories
- weather-dependent items where supported by data

### Season index

For each category and calendar month:

**Season Index = Category Sales in Month / Category Average Monthly Sales**

Normalize separately by year when strong overall growth would distort seasonality, then average the monthly indices across years.

Interpretation:
- 1.00 = normal month
- >1 = above-normal seasonal demand
- <1 = below-normal seasonal demand

### Season-adjusted trend

For seasonal SKUs, do not compare adjacent months as evidence of decline.

Prefer:
- same-month YoY
- same-period YoY
- SKU trend versus category seasonal baseline

A seasonal SKU cannot be sent to clearance solely because the latest 3 months are weak during its normal off-season.

## Stage 2 — SKU Selection / 筛商品

### Four-quadrant analysis

Default SKU scatter:
- X-axis: Core Net Sales
- Y-axis: Model Contribution Margin

Use robust thresholds:
- high sales = P75 of active SKU sales, or another explicitly documented threshold
- high margin = 10% default unless the user changes it

Quadrants:
- High Sales + High Margin = Star / 明星
- High Sales + Low Margin = Cash Cow / Margin Issue / 现金牛或利润问题
- Low Sales + High Margin = Potential / 潜力
- Low Sales + Low Margin = Low Efficiency / 低效

For category-level scatter:
- show every meaningful category
- label all points or at minimum all top categories and all outliers
- show both axis definitions
- show threshold lines
- include units and percent ticks

Never publish an unlabeled scatter that the reader cannot interpret.

## Stage 3 — Contribution / 算贡献

Distinguish levels:

1. Gross profit / 毛利润
2. Model contribution profit / 模型贡献利润
3. Real contribution profit / 真实贡献利润
4. Company net profit / 公司净利润

Do not call model contribution profit “real contribution profit”.

### Real contribution profit

Only calculate when required expense data exists:

**Real Contribution Profit =
Net Sales
- Product Cost
- Ad Spend
- Shipping Cost
- Payment Fees
- Affiliate Commission
- Other order-level variable costs**

If those fields are unavailable, explicitly label the result as:
**Estimated / Model Contribution Profit**

### Inventory coverage

For non-seasonal products:
- recent average monthly units may be used as a simple coverage baseline

For seasonal products:
- use forward-looking seasonal demand
- preferably use average sales for the same upcoming calendar months in prior years
- if history is insufficient, adjust recent velocity with the category season index

Example:

**Forward Inventory Months =
Current Shared Available Inventory /
Expected Average Monthly Units for Next 3 Months**

Always label it as an estimated shared-inventory coverage measure.

## Stage 4 — Decision Rules / 定策略

Every active SKU should receive:
- Primary Decision
- Supporting Flags
- Short reason

Allowed decision flags:
- Restock / 补货
- Increase Exposure / 增加曝光
- Maintain / 维持
- Price Increase Test / 提价测试
- Reduce Actual Discount / 减少折扣
- Bundle / 捆绑销售
- Clearance / 清仓
- Stop Restock / 停止补货
- Cost Review / 成本优化
- Seasonal Hold / 季节等待

### Clearance logic

Never use one single metric.

A clearance candidate usually requires several signals:
- low sales
- low model margin or negative contribution
- high forward inventory coverage
- mature/legacy product
- season-adjusted trend is weak
- no clear strategic reason to retain the SKU

Seasonal off-season alone is not a clearance signal.

### Bundle logic

Bundle candidates may include:
- high inventory + acceptable margin
- low standalone sales but strong compatibility with a bestseller
- complementary products
- products where discounting directly would damage price integrity

Prefer logical cycling use-case bundles rather than arbitrary combinations.

### Price increase logic

Consider a price increase test when:
- sales are strong
- model margin is below target
- actual discounting is low
- product is not in obvious decline
- inventory is not critically excessive
- market positioning allows it

For Bikes, current non-markdown status is not itself a reason to raise price.

### Reduce-discount logic

Use only actual Shopify Discounts.

If:
- sales are strong
- margin is weak
- actual discount rate is high

then consider reducing promotional discounting before raising list price.

### Restock logic

Consider restock when:
- strong sales
- healthy contribution
- forward inventory coverage is low
- no imminent seasonal decline that makes stock unnecessary

### Increase-exposure logic

Potential SKU:
- low sales
- high margin
- good conversion data if available
- low traffic or limited visibility
- inventory available

Recommend exposure tests before price cuts.

## Output Requirements

### Excel

Produce an editable workbook with at least:
1. 总览
2. 测算时间与定义
3. 品类分析
4. 价格带分析
5. 季节性
6. 生命周期
7. SKU完整分析
8. 四象限
9. 库存风险
10. 决策清单
11. 品类映射审计
12. Category Classification Audit

SKU完整分析 must include:
- Goods No.
- SKU
- Product Title
- Category
- Subcategory
- Classification Basis
- Image URL
- Current Price
- Compare-at Price
- Actual Discount Rate
- Published At
- Lifecycle
- Seasonality
- Core Sales
- YoY
- Margin
- Estimated Contribution Profit
- Warehouse Inventory A
- Warehouse Inventory B
- Shared Available
- Forward Inventory Months
- Quadrant
- Decision flags
- Primary Decision

Every sheet must display or reference its measurement period.

### PPT

PPT is not just a screenshot of Excel.

Required qualities:
- clear executive title
- measurement period on every analytical slide
- complete axes
- units
- legends
- direct labels where feasible
- no unlabeled category dots
- no incomplete lines where the reader cannot identify the series
- all major data points represented
- written judgment on every slide
- explicit conclusion / action box on every analytical slide

Suggested slides:
1. Executive summary
2. Methodology and periods
3. Category structure
4. Category sales × margin quadrant
5. Price-band mix
6. Margin by price band
7. Seasonal category index
8. Lifecycle
9. SKU four-quadrant
10. Inventory / forward coverage
11. Decision distribution
12. Priority action list
13. Bikes audit when relevant

## Quality Checks Before Finalizing

1. Confirm Bikes contains only complete bicycles.
2. Search for Bikes containing:
   - light
   - tube
   - trainer
   - saddle
   - handlebar tape
   - wheel
   - mask
   - tool
   and correct any false positives.
3. Confirm no Compare-at based discount logic is used.
4. Confirm all category margin calculations are weighted.
5. Confirm seasonal SKUs are not classified as declining solely because of off-season recent months.
6. Confirm configured sellable warehouse pool inventory uses available inventory.
7. Confirm shared inventory is labeled as shared.
8. Confirm all charts have titles, axes, units, labels/legend, measurement period, and conclusion.
9. Confirm no duplicate sales caused by Product Tag expansion.
10. Confirm category classification can be audited SKU-by-SKU.
