# OpenClaw 记忆架构评估 & 升级方案

> 评估日期: 2026-04-27

## 一、当前记忆架构（现状评估）

### 已启用层

| 层 | 技术 | 状态 | 说明 |
|----|------|------|------|
| **① 热记忆** | 会话上下文 + lossless-claw | ✅ 完美 | lossless-claw 保全会话历史，支持无损召回 |
| **② 语义搜索** | OpenClaw `memorySearch` | ✅ 已启用 | 使用 dashscope `text-embedding-v4`（非 OpenAI） |
| **③ 知识文件** | MEMORY.md + memory/*.md | ✅ 良好 | 结构化的长期知识库 |
| **④ 反思回放** | reflection-compiler + playbooks | ✅ 已启用 | 自动生成 tool 失败 playbook |
| **⑤ 记忆沉淀** | memory-scribe | ✅ 已启用 | Hermes 风格记忆起草 |
| **⑥ 学习日志** | `.learnings/` + self-improvement | ✅ 有但可优化 | 有 LEARNINGS.md 但使用不够系统 |
| **⑦ 会话索引** | MemPalace | ✅ 旁路可检索 | 只读全文检索 |

### 当前缺失或薄弱层

| 层 | 状态 | 问题 |
|----|------|------|
| **❌ 向量数据库** | 未启用 | lancedb 已安装，但 memory-lancedb 插件未在 config 中激活 |
| **❌ 多级记忆热温冷** | 未实现 | 没有 SESSION-STATE.md 做热内存 |
| **⚠️ memory_search 索引** | 仅限 .md 文件 | 不能索引会话历史（lossless-claw 可以 grep） |
| **⚠️ 自动事实提取** | 无 | 没有 Mem0 或类似自动提取 |
| **⚠️ 子 agent 记忆继承** | 靠手动 | 子 agent 不自动继承父会话记忆 |

## 二、关于 elite-longterm-memory skill 的分析

**elite-longterm-memory** 主要针对 Claude Code / Cursor 等 AI IDE 设计，不是专门为 OpenClaw 优化的。

它要求的 **OPENAI_API_KEY** 其实是用于：

| 组件 | 实际可替换？ | 方案 |
|------|------------|------|
| LanceDB 向量存储 | ✅ **不需要 OpenAI** | LanceDB 本地运行，可以用 sentence-transformers 或任何 embedding |
| semantic search | ✅ 已有替代 | 你已经在用 dashscope `text-embedding-v4` |
| Mem0 自动提取 | ❌ 需要 API Key | 但这不是必须的 |
| SuperMemory 云备份 | ❌ 可选 | 非核心功能 |

**结论：不需要 OpenAI API Key。** elite-longterm-memory 的核心价值在于其**架构设计理念**（分层记忆 + WAL 协议），而不是它需要 OpenAI。我们可以借鉴其分层架构，但全部用你已有的基础设施实现。

## 三、最佳升级方案

### 方案 A：中等升级（推荐，1-2h 工作量）

在现有基础上弥补最大缺口：

```
┌─────────────────────────────────────────────────────────┐
│                  升级后的记忆架构                        │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  热: SESSION-STATE.md  ← 新增：WAL 内存协议             │
│                                                         │
│  温: memory_search (dashscope) + LanceDB 向量          │
│      ↓ 升级：激活 lancedb 插件，换本地 embedding        │
│                                                         │
│  冷: MEMORY.md + memory/ + .learnings/                  │
│      ↓ 升级：重构为结构化目录                           │
│                                                         │
│  存档: lossless-claw (全会话无损) + MemPalace           │
│                                                         │
│  反射: reflection-compiler (playbooks)                   │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

#### Step 1: 添加 SESSION-STATE.md（热内存）

用 WAL（Write-Ahead Log）协议：每次修改前先写状态，再回复。

**文件位置**: `~/.openclaw/workspace/SESSION-STATE.md`
**内容**: 当前任务、关键上下文、待办决策

#### Step 2: 激活 LanceDB 向量插件

在 `openclaw.json` 的 `plugins.entries` 中添加：

```json
"memory-lancedb": {
  "enabled": true,
  "config": {
    "autoCapture": false,
    "autoRecall": true,
    "captureCategories": ["preference", "decision", "fact"],
    "minImportance": 0.7,
    "embeddingsProvider": "openai"  
  }
}
```

⚠️ **关于 embedding 模型的关键发现**：

你当前的 `memorySearch` 已经配置了 daschscope 作为 embedding 提供方：
```json
"memorySearch": {
  "provider": "openai",
  "remote": {
    "baseUrl": "https://dashscope.aliyuncs.com/compatible-mode/v1",
    "apiKey": "sk-..."
  },
  "model": "text-embedding-v4"
}
```

这意味着 `memory_search` 工具已经能正常工作，**使用的是阿里云通义千问的 text-embedding-v4，不需要 OpenAI**。

LanceDB 也可以用同样的 daschscope embedding。

#### Step 3: 重构 memory/ 目录结构

```
memory/
├── playbooks/           ← 已有（tool 失败反思）
├── projects/            ← 新增：项目级记忆
│   └── sellersprite.md
├── decisions/           ← 新增：决策日志
│   └── 2026-04.md
├── lessons/             ← 新增：踩坑教训
│   └── tool-failures.md
├── people/              ← 新增（可选）
│   └── user-profile.md
└── preferences/         ← 新增（可选）
```

#### Step 4: 让子 agent 继承记忆

在 `AGENTS.md` 或 spawn 规则中加入：
> 子 agent 启动时自动 `memory_search` 获取相关上下文

### 方案 B：轻量升级（最快，30min）

只做最小的改动提升记忆质量：

1. 创建 `SESSION-STATE.md` — 热内存协议
2. 把 `.learnings/` 的教训定期沉淀到 `memory/lessons/`
3. 给 MEMORY.md 加结构和目录索引

### 方案 C：完整升级（3-5h，最完善）

包含方案 A 全部，再加上：
4. 安装配置 Mem0 或自建自动提取管道
5. 建立定时记忆整理 cron job
6. 建立跨会话记忆关联图

## 四、我的建议

**当前记忆体系评分：7/10**

| 维度 | 分数 | 说明 |
|------|------|------|
| 会话记忆 | 9/10 | lossless-claw 非常强，无损 DAG 结构优于大部分方案 |
| 长期知识 | 7/10 | MEMORY.md + memory/ 文件结构不错，可更好组织 |
| 语义检索 | 6/10 | memory_search 已启用，但受限于仅索引 .md 文件 |
| 自动反思 | 7/10 | reflection-compiler 生成 playbook 很好用 |
| 热记忆 | 3/10 | 缺少 SESSION-STATE.md 作为工作内存 |
| 跨会话关联 | 5/10 | MemPalace 可搜，但缺少自动关联 |

**最推荐的路径**：方案 A（中等升级），1-2h 内完成，几个关键优化：

1. **SESSION-STATE.md**（30min）— 最大的性价比提升
2. **激活 LanceDB + 目录重构**（30min）— 让向量搜索覆盖更多类型
3. **不需要 OpenAI** — 全部用 dashscope + 本地替代

要我开始实施哪个方案？
