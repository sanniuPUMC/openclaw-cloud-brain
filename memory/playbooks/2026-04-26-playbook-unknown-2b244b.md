# Tool failure: browser
- **Trigger**: tool:browser | NOT
- **Protocol**:
  - 先确认服务状态/队列是否卡住，再重试（避免空转）。
  - 重取快照/截图，确认 URL 与目标控件仍存在。
  - 若 ref 失效：重新 snapshot 并用可见文本重新定位。
  - 连续两次无变化：换策略（滚动/等待/改选择器/直跳 URL）。
- **Success**: service active (running) | 日志无持续 error
- **Source**: tool=browser
> [reflection-compiler:auto]
