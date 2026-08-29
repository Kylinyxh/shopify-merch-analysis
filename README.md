# Shopify SKU Decision Tool + Report Updater Skill

以后你最简单的操作只有一句话：

> 上传 **最新销量表 + 最新库存表**，然后说：**更新决策工具**

Skill 会一次性输出三个文件：

1. **HTML 决策查询工具** — 输入 SKU 直接看建议
2. **Excel 完整分析表** — 全量 SKU / 品类数据、四个月季节判断、库存、价格带、建议定价
3. **PPT 汇报版** — 品类分析、季节变化、四象限、库存风险、价格带、SKU行动清单和明确结论

## 四个月季节判断

每次更新都会按当前日期自动计算：

- M0：当前月
- M1：下月
- M2：下下月
- M3：第4个月

例如当前经营窗口是 9 月，就显示：

**9月 → 10月 → 11月 → 12月**

每个月同时显示：
- 标准化季节指数
- 淡季 / 偏淡 / 平季 / 偏旺 / 旺季

这样冬季品能提前看到旺季，夏季品也能提前看到转淡。

## 每次只需要提供

### 1. 最新销量表
建议包含：
- Month
- SKU / Product variant SKU
- Product title
- Net sales
- Net quantity
- Gross sales
- Discounts

最好保留滚动 15–24 个月历史数据，保证同比与季节判断稳定。

### 2. 最新库存表
至少包含：
- SKU
- Warehouse / 仓库
- Available Inventory / 可用库存

可以上传多个库存文件。

## Skill 内置、不需要每次重复提供

- 商品标题
- 品类 / 子品类
- 货品编号
- 当前售价
- 利润模型
- 上架时间
- 生命周期
- 历史季节指数
- 价格带结构

## Excel 默认包含

- 总览
- 测算时间与定义
- 品类分析与结论
- 品类四个月季节判断
- 品类价格带
- SKU完整决策表
- SKU四象限
- 库存风险
- 建议定价
- 决策行动清单
- 待分类SKU

## PPT 默认包含

不固定页数，以“一个问题一页”为原则：
- 管理层总结
- 品类销售 / 占比 / 利润 / YoY / 库存 + 结论
- 品类四象限 + 结论
- 价格带结构 + 结论
- 四个月季节判断 / 旺季准备 + 结论
- 生命周期 + 结论
- 库存覆盖 + 结论
- SKU四象限 + 结论
- 建议提价 SKU + 测试价
- 最终行动清单

## 新 SKU

如果最新销量表出现 baseline 没有的 SKU：
- 不会乱归类
- 标记为 **待分类 / 待补资料**
- 仍进入 HTML 和 Excel
- PPT 不把它计入正式品类结论

## 目录结构

```text
shopify-sku-decision-tool-updater-skill/
├── SKILL.md
├── README.md
├── VERSION
├── CHANGELOG.md
├── baseline/
├── config/
├── scripts/
├── references/
│   ├── input_rules.md
│   ├── excel_output_spec.md
│   └── ppt_output_spec.md
├── templates/
└── examples/
```
