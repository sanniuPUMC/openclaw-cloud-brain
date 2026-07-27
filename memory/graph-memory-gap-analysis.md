# 知识图谱与因果关联记忆 — 实施蓝图

> 日期: 2026-04-27
> 基于: memory-landscape-research.md 广域调研
> 理念来源: MCP Memory Service (doobidoo), HyperMem (EverOS), M-Flow

## 设计哲学：轻量知识图谱

完整部署 Neo4j/TrustGraph/GraphRAG 对于单 agent 场景**过重**。
我们采用**文件层面的类型化边（typed edges）**方案：

- **不引入新数据库** — 用 Markdown 文件内的链接语法 `[[target]]` 表达关联
- **不引入额外服务** — 用现有 LanceDB + grep 做关联检索
- **边类型固定但有意义** — `causes / fixes / contradicts / relates_to / depends_on`

## 实施步骤

### 第一步：知识图谱记忆文件 (`memory/graph/`)

在 `memory/` 下创建 `graph/` 目录，存储三种节点类型：

```
memory/graph/
├── nodes/           # 实体/概念/项目节点
│   ├── sellersprite.md
│   ├── dashscope-embedding.md
│   └── ...
├── edges.jsonl      # 类型化边（一行一条）
└── README.md        # 图结构说明
```

### 第二步：edge schema

每条边是 JSON 一行：
```json
{"source": "sellersprite", "target": "spr-strategy", "type": "depends_on", "reason": "SPR选品依赖卖家精灵数据"}
{"source": "rhui1987", "target": "888JQJL", "type": "contradicts", "reason": "前者是VIP会员，后者是试用账号不应用"}
```

### 第三步：集成到现有检索工作流

- `memory_search` 可以搜索 nodes/ 目录中的 Markdown 文件
- `grep edges.jsonl` 可以按 source 或 target 查询因果链
- `lcm_grep` 可以为关联提供对话证据

### 第四步：自动边生成（轻量版）

在 `.scripts/extract_facts.py` 中添加 `--edges` 功能：
- 当对话中出现 "用 A 替代 B" → 生成 `contradicts` 边
- 当对话中出现 "A 依赖于 B" → 生成 `depends_on` 边
- 当对话中出现 "A 导致了 B" → 生成 `causes` 边
