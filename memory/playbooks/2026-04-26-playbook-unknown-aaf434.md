# Tool failure: gateway
- **Trigger**: tool:gateway
- **Protocol**:
  - 补一条关键证据（日志/报错/截图/快照/命令输出）定位根因。
  - 先最小验证，再分岔 2 条路径（最快/最稳），并写明成功信号。
- **Success**: service active (running) | 日志无持续 error
- **Source**: tool=gateway
> [reflection-compiler:auto]
