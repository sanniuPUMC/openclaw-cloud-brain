# 领星 ERP 自动化与浏览器配置指南

## 核心环境配置 (PVE + Ubuntu 24.04)

### 1. 桌面自动登录与显示绑定
为了确保 Agent 重启后能直接看到图形界面并接管浏览器，必须配置 GDM 自动登录：
- **文件路径**: `/etc/gdm3/custom.conf`
- **配置项**:
  ```ini
  AutomaticLoginEnable = true
  AutomaticLogin = sanniukin
  ```
- **环境变量**: 所有的浏览器自动化必须绑定到 `DISPLAY=:0`。

### 2. OpenClaw 浏览器插件配置
为了绕过滑块验证、维持长期登录态并避免端口冲突，必须使用以下持久化配置：
- **Headless**: `false` (始终开启有头模式，以便在 VNC 中可见和手动干预)
- **Profile**: `openclaw` (专属代理模式)
- **User Data Directory**: `/home/sanniukin/.openclaw/browser-data`
- **Remote Debugging Port**: `18800`

### 3. x11vnc 工具 (可选)
如果需要远程观察物理桌面 :0，可运行：
- **命令**: `x11vnc -forever -display :0 -auth /run/user/1000/gdm/Xauthority -rfbport 5900`
- **密码**: `004869`

## 领星 ERP (erp.lingxing.com) 操作规程

### 登录与验证
- **登录方式**: 首次使用或登录失效时，Agent 自动调起 `DISPLAY=:0` 的 Chrome。
- **滑块验证**: 需要人工在 PVE 的 VNC 界面手动完成一次滑块。
- **持久化**: 由于配置了固定的 `userDataDir`，登录后的 Session 会永久保留在服务器，下次无需重复登录。

### 数据提取流程
1. 启动命令示例:
   `DISPLAY=:0 google-chrome-stable --remote-debugging-port=18800 --user-data-dir=/home/sanniukin/.openclaw/browser-data --no-sandbox "https://erp.lingxing.com/" &`
2. 使用 `browser(action="snapshot")` 直接读取看板数据。
3. 核心关注指标: 销量、销售额、订单量、ACOS、ACoAS、库存在库/在途、异常事项。

---
*上次更新: 2026-03-28 (sanniukin-pc)*
