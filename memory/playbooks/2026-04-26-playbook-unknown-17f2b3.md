# Stuck loop: tighten & request evidence
- **Trigger**: tool:agent_end
- **Protocol**:
  - 置顶：`状态：失败/部分完成` + 一句卡点结论。
  - 列出 1 条缺失证据（日志/报错/快照/命令输出）作为继续推进条件。
  - 给 2 条路径（最快/最稳），每条写明成功信号；两轮无增量则收口。
- **Success**: 拿到关键证据后能继续推进 | 主人不需要反复追问进度
- **Source**: agent_end: unsuccessful run
> [reflection-compiler:auto]
