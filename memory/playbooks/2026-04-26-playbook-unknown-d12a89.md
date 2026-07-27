# Tool failure: exec
- **Trigger**: tool:exec | ALL_PROXY | HTTP_PROXY | HTTPS_PROXY
- **Protocol**:
  - `systemctl --user status openclaw-gateway.service --no-pager -l`
  - `journalctl --user -u openclaw-gateway.service -n 120 --no-pager`
  - 若为配置变更：重启网关后再验证（并记录变更点）。
- **Success**: 错误不再出现 | 关键命令/步骤可复现
- **Source**: tool=exec
> [reflection-compiler:auto]
