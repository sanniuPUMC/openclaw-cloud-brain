# Tool failure: image_generate
- **Trigger**: tool:image_generate | API | http:443
- **Protocol**:
  - 先确认服务状态/队列是否卡住，再重试（避免空转）。
  - 补一条关键证据（日志/报错/截图/快照/命令输出）定位根因。
  - 先最小验证，再分岔 2 条路径（最快/最稳），并写明成功信号。
- **Success**: 错误不再出现 | 关键命令/步骤可复现
- **Source**: tool=image_generate
> [reflection-compiler:auto]
