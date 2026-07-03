# Tool failure: browser
- **Trigger**: tool:browser | NOT | /home/sanniukin/.local/node-v24.14.1-linux-x64/lib/node_modules/openclaw/dist/local-dispatch.runtime-D0nZQ4fb.js | /home/sanniukin/.local/node-v24.14.1-linux-x64/lib/node_modules/openclaw/dist/session-tab-registry-CJx9utct.js
- **Protocol**:
  - 重取快照/截图，确认 URL 与目标控件仍存在。
  - 若 ref 失效：重新 snapshot 并用可见文本重新定位。
  - 连续两次无变化：换策略（滚动/等待/改选择器/直跳 URL）。
- **Success**: service active (running) | 日志无持续 error
- **Source**: tool=browser
> [reflection-compiler:auto]
