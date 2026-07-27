# MEMORY.md - Long-Term Memory (Index Only)

> 💡 为了节省 Token，本文件仅保留核心索引和红线规则。具体配置按需读取。

## 🚫 核心红线 (Hard Rules) - 2026/04/21 修订版

1. **绝对拒绝杜撰**: 严禁捏造任何行情数据（股价、行权价、权利金、IV）。若工具链失效，必须如实报告“无法获取数据”，严禁进行“模拟推测”。
2. **强制实时校验**: 凡涉及金融标的，回复前必须先执行 `get_market_snapshot` 获取**当前真实股价**。禁止依赖预训练模型中关于拆股、合股或历史价格的过期记忆。
3. **先数后话流程**: 涉及期权、资产配置时，必须在回复中先展示 Raw Data（原始数据表），再进行逻辑推演。
4. **代理与合规**: 调用金融 API 必须强制注入代理（192.168.100.183:7890）以防 IP 被封叠加上下文污染。
5. **错误必复盘**: 一旦发现数据错误，必须立即更新此文档，分析根因并提出自愈动作。

## 🛠️ 配置索引 (On-Demand Lookup)
- **API Keys & Proxy**: 见 `TOOLS.md`。
- **历史决策库**: 检索 `memory/` 目录。
- **记忆分层与沉淀**: 见 `memory/openclaw-memory-discipline.md`。
- **执行纪律（顾问式下一步 + 执行锐度补充包 + 委派进度）**: 见 workspace `SOUL.md` 对应章节（与 `AGENTS.md` §C.2、§E 对齐）。
- **跨体协作（agentToAgent + Claude Code / Hermes 衔接）**: `~/.openclaw/openclaw.json` 的 `tools.agentToAgent` + `SOUL.md`「跨体协作」+ `AGENTS.md` §B.2。
- **编队编排（Hub、双子互斥、L3 工作流）**: `AGENTS.md`「编排拓扑与分层」+ §B.4；`main.subagents.allowAgents` 见 `openclaw.json`。
- **三体默认模型栈（main / heavy_coder / claw_code）**: 官方 V4 Flash → GLM 5.1 → Kimi Code → V4 Pro → Gemini3（末位）；见 `AGENTS.md` §编排 4) 与 `openclaw.json` `agents.defaults` + 三 agent 的 `model`。
- **压缩/LCM 省钱**: `agents.defaults.compaction.model` + `plugins.entries["lossless-claw"].config`（summary / largeFile / expansion）默认 `deepseek/deepseek-v4-flash`；主对话仍走各 agent `model` 链。见 `AGENTS.md` §编排 5)。
- **claw_code 真实仓库**: workspace = `~/.openclaw/workspace/cloned-repos/claw-code`。
- **反思 Playbook**: 见 `memory/playbooks/2026-04-25-playbook-unknown-6503ce.md`。
- **反思 Playbook**: 见 `memory/playbooks/2026-04-25-playbook-unknown-fdb877.md`。
- **反思 Playbook**: 见 `memory/playbooks/_index.jsonl`。
- **记忆条目**: 见 `memory/2026-04-26-memory-ou-9230f5211fa2d688aba1f97568d28390-d0534c.md`。
- **记忆体系评估**: 见 `memory/facts/memory-architecture-assessment.md`（2026-04-27 方案A 评估）。
- **记忆全景调研**: 见 `memory/facts/memory-landscape-research.md`（2026-04-27 广域调研报告）。
- **热内存 SESSION-STATE.md**: 见 `workspace/SESSION-STATE.md`（WAL 写前日志协议）。
- **记忆目录结构**: `memory/projects/`（项目）、`memory/decisions/`（决策）、`memory/lessons/`（教训）、`memory/facts/`（事实/偏好）。
- **轻量自动事实提取**: 见 `.scripts/extract_facts.py`（对话→结构化记忆自动管道）。
- **子 agent 记忆继承**: 见 `AGENTS.md` §3.5（spawn 时注入记忆上下文模板）。
- **Skills 安装配置**: 见 `TOOLS.md`「Skills 安装状态」表。
- **记忆升级蓝图**: 见 `memory/memory-upgrade-blueprint.md`（2026-04-27 GitHub 搜索+对比分析）。
- **Cognee 知识引擎**: `pip install cognee` v0.5.7 已安装，内置 LanceDB 图+向量混合存储，有 OpenClaw 插件。
- **MCP Memory Service**: GitHub `doobidoo/mcp-memory-service`（因果知识图谱 MCP，待网络恢复后克隆）。
- **记忆条目**: 见 `memory/2026-04-26-memory-ou-9230f5211fa2d688aba1f97568d28390-0f372a.md`。
- **记忆条目**: 见 `memory/2026-04-27-memory-ou-9230f5211fa2d688aba1f97568d28390-7f6f68.md`。

## 进化系统
- **轻量自动进化系统**: `.learnings/` 三层学习沉淀（HOT_FIX → WEEKLY → PERMANENT）
- **管理脚本**: `.scripts/learnings_manager.py`
- **使用指南**: `.learnings/README.md`
