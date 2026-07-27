# 卖家精灵选品体系

## 基本信息
- **平台**: sellersprite.com
- **核心功能**: 关键词选品、市场分析、竞品监控
- **唯一可用账号**: rhui1987（VIP会员）
  - **不要用**: 888JQJL（试用账号，会被限制弹窗）

## 关键配置
- **SPR 黄金法则**: 见 projects/spr-strategy-v1.md
- **类目排除**: Baby（index 4）、Toys & Games（index 18）
- **筛选条件**: Price > $20, PPC < $3, SPR < 12.5（整数取 floor）

## 自动化工具
- **SKILL.md**: skills/SellerSprite-Automation/SKILL.md v1.2.0
- **DOM索引**: checkbox 2-20, price[62,63], PPC[86,87], SPR[12,13]

## 相关坑位
- SPR 输入框不支持小数 → 用 floor(1/CVR)
- 弹窗干扰时 → 退出登录清除缓存
- URL 参数可验证筛选条件提交状态
