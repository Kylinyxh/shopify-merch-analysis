# Metrics and Formulas

## Sales share

Sales Share = Category Net Sales / Total Net Sales

## Actual discount rate

Actual Discount Rate = - Discounts / Gross Sales

Do not derive this from Compare-at Price.

## Estimated / model contribution profit

When a SKU margin model exists:

Estimated Contribution Profit = Net Sales × Model Contribution Margin

This is not real contribution profit unless all variable costs are actually included and sourced.

## Weighted category margin

Category Model Margin =
Sum(Estimated Contribution Profit) /
Sum(Net Sales with valid margin model)

Never average SKU percentages arithmetically.

## Season index

Season Index = Monthly Category Sales / Category Average Monthly Sales

Prefer year-normalized indices before cross-year averaging.

## Forward inventory coverage

Forward Inventory Months =
Current Shared Available /
Expected Average Monthly Units for the next 3 months

For seasonal items, expected demand should use same-calendar-month history when possible.

## Real contribution profit

Only when all required costs exist:

Real Contribution Profit =
Net Sales
- Product Cost
- Advertising Spend
- Shipping Cost
- Payment Fees
- Affiliate Commission
- Other variable order-level costs
