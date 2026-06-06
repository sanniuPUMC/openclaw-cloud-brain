# LingXing-ERP 自动化销售总结技能

本技能专门用于在 Ubuntu 24.04 (PVE VM) 图形界面环境下，自动化操作领星 ERP 系统 (`erp.lingxing.com`)，提取实时销售数据并生成报告。

## 1. 核心运行环境 (系统层级)

### 桌面绑定与自动登录
- **物理桌面**: 必须绑定到 `DISPLAY=:0`。
- **自动登录**: 已根据 `/etc/gdm3/custom.conf` 配置 `sanniukin` 自动登录，确保重启后直接进入 GUI 桌面。
- **显示监控**: 用户可通过 PVE 虚拟机的 VNC 控制台实时监控操作窗口。

### 自动化权限 (sudo)
- **用户**: `sanniukin`
- **Root 权限密码**: `004869` (仅在必须清理旧进程、修改网络/系统配置时使用 `echo "004869" | sudo -S ...`)

## 2. 浏览器自动化方案 (Agent 层级)

### 核心参数
- **类型**: 有头模式 Chrome (`headless: false`)。
- **控制方式**: CDP (Remote Debugging)。
- **调试端口**: `18800` (如果此端口被无头后台进程占用，必须先清理)。
- **数据持久化**: 使用专属路径 `/home/sanniukin/.openclaw/browser-data`（配置为 `openclaw` Profile）。
- **启动指令示例**: 
  `DISPLAY=:0 google-chrome-stable --remote-debugging-port=18800 --user-data-dir=/home/sanniukin/.openclaw/browser-data --no-sandbox "https://erp.lingxing.com/" &`

## 3. 操作流程 (标准化任务)

### 任务 A: 提取销售概况 (已登录状态)
1.  检查端口 `18800` 是否活跃。
2.  若不活跃，通过 `exec` 后台静默启动带调试端口的 Chrome。
3.  使用 `browser(action="snapshot")` 直接获取 `https://erp.lingxing.com/` 的 DOM 快照。
4.  解析核心数据:
    - 今日/昨日: 销量、销售额、订单量。
    - 近 30 天: 销售额走势、毛利润、毛利率、ACOS、退款。
    - 风险项: 库存不足数 (通常为 Top 统计)。

### 任务 B: 登录与滑块异常处理
1.  如果 snapshot 显示在登录页或滑块验证码页。
2.  通过消息通知用户: "请在 VNC 中完成领星登录/滑块验证"。
3.  等待用户确认 "已登录" 后，再次执行【任务 A】。
4.  登录态将持久化保存，后续通常无需重复。

## 4. 输出规范

### 报告模板
> ### [日期] 领星 ERP 销售经营摘要
> - **实时销售**: 今日销 [X] 笔, 销额 $[Y], ACoAS [Z]%
> - **近30天走势**: 总销 [A], 同比 [B], 环比 [C], ACOS [D]%
> - **资产健康**: 库存监控 ([E] 项缺货), 结算毛利 $[F], 利率 [G]%
> - **动作建议**: [基于数据给出的运营操作建议]

### 任务 C: 复杂日期组件的全自动控制 (New ✨)
1.  **打开日期面板**: 找到 `input[placeholder="请选择时间"]` 并点击。
2.  **定位快捷按钮**: 领星的日期面板包含快捷项。必须使用 `evaluate` 模式执行精准的文字匹配点击，而非依赖 `ref` 索引。
    - **逻辑**: `Array.from(document.querySelectorAll('li, button, span')).find(el => el.innerText.trim() === '最近30天').click()`
3.  **多重验证机制**: 
    - 点击后必须等待至少 3 秒等待异步数据加载。
    - **强制核对**: 读取数据前，必须通过 `snapshot` 检查输入框内的日期文字是否与目标一致（如检查是否显示为 `2026-xx-xx - 2026-xx-xx`），防止因 UI 遮挡导致的点击失效。
4.  **常见坑点**: 
    - 领星的下拉列表有时会因为右侧日历浮层的 z-index 干扰导致模型通过视觉看到的 `ref` 发生位移。
    - 优先使用 JS 注入点击 (`evaluate`)，备选方案才是视觉点击。

---
*Created by OpenClaw Agent Creator for sanniukin-pc | Last Updated: 2026-03-28*
