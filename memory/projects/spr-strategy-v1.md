# 汐灵 (Xi Ling) - SPR 选品黄金法则 (SPR V1.0)

## 🎯 核心逻辑定义 (Definitions)
1. **SPR (SellerSprite Purchase Rate)**: 代表产品在 8 天内获得该关键词流量所需的最小销量。
2. **逻辑核算 (Assumptions)**:
   - 假定**点击转化率 (CVR) = 8%**。
   - **潜在成本计算**: 
     - 每日关键词获取成本 = (预计流量) * CPC。
     - 8 天流量获取成本 = (SPR / 8%) * CPC。

## 📥 核心筛选参数 (Filtering Criteria)
- **价格 (Price)**: > $20 (排除无意义低客单件)。
- **CPC 竞价 (CPC Bid)**: < $3 (确保流量成本可控)。
- **月购买量 (Monthly Purchases)**: 必须有真实市场支撑。
- **转化率核对**: 需结合 8% CVR 进行利润回测。
- **SPR 数据类型**: ⚠️ SPR 字段仅接受**整数**！计算公式：`SPR_max = floor(1 / CVR)`。
  - 例：`8% CVR → 1/0.08=12.5 → floor(12.5)=12`
  - 例：`10% CVR → 1/0.10=10 → 10`
  - 填入小数会被系统静默忽略，导致筛选无结果。

## 🚫 风险与排除项 (Exclusions)
- **类目限制**: 
  - ❌ **儿童相关 (Kids/Baby)**: 排除，监管过严。
  - ❌ **玩具相关 (Toys)**: 排除，合规风险高。
- **物流限制**: 
  - ❌ **家具/大件 (Furniture/Oversized)**: 排除，体积重量过大，头程及仓储成本过高。

## 🛠️ 执行分工 (Implementation)
- **视觉大脑**: Qwen-VL (精准定位参数框)。
- **执行逻辑**: Gemma 4 (填写参数并核算成本)。
- **异常预警**: MiniMax 2.7 (拦截类目违规产品)。

---
*Created: 2026-04-06*
*Updated: 2026-04-27 — 补充 SPR 整数约束*
*Status: Active - 黄金法则*
