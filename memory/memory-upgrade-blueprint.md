# Agent 记忆系统升级蓝图

> 日期: 2026-04-27
> 数据来源: GitHub API 搜索 (🔍) + web_fetch 抓取 (🌐) + 先前的 memory-landscape-research.md (📋)
> 关键词: agent-memory, knowledge-graph, mcp-memory-service, cognee, mem0

## 一、GitHub 开源项目全景图

| 项目 | Stars | 语言 | 部署 | 记忆模型 | MCP/OpenClaw | 可集成度 |
|------|-------|------|------|---------|-------------|---------|
| 🔍 **cognee** (topoteretes) | ~8k | Python | 本地 | Graph+Vector 混合 | ✅ OpenClaw 插件 `@cognee/cognee-openclaw` | 🔥🔥🔥🔥🔥 |
| 🔍 **mem0** (mem0ai) | ~25k | Python | 本地/托管 | 自动提取+多信号检索 | 无官方插件但可 SDK | 🔥🔥🔥🔥 |
| 🔍 **mcp-memory-service** (doobidoo) | ~300 | Python | Docker | 知识图谱+因果边 | ✅ MCP 协议 | 🔥🔥🔥🔥 |
| 📋 **Supermemory** | ~8k | TypeScript | 托管/本地 | RAG+混合搜索 | ✅ OpenClaw 插件 | 🔥🔥🔥🔥 |
| 📋 **EverOS/HyperMem** | ~1k | Python | 本地 | 超图层次记忆 | 无插件 | 🔥🔥🔥 |
| 📋 **MemSearch** (Zilliz) | ~500 | Python | 本地 | Markdown-first+Milvus | ✅ OpenClaw 插件 | 🔥🔥🔥🔥 |
| 📋 **Letta (MemGPT)** | ~15k | Python | 托管/本地 | Memory Blocks 层级 | 无官方插件 | 🔥🔥🔥 |
| 📋 **M-Flow** (FlowElement) | ~200 | Python | 本地 | 图评分引擎 | 无插件 | 🔥🔥 |
| 📋 **GraphRAG** (微软) | ~25k | Python | 本地 | 知识图谱+社区摘要 | 无插件，成本高 | 🔥🔥 |
| 🔍 **Awesome-AI-Memory** | list | — | — | 知识库 | 调研用 | — |

## 二、Top 3 高价值项目深度分析

### 🥇 Cognee — 最具实操价值

**GitHub**: `topoteretes/cognee`, Stars ~8k, Language: Python
**描述**: "Knowledge Engine for AI Agent Memory in 6 lines of code"

**核心能力**:
1. 向量搜索 + 图数据库 + 认知科学方法的混合
2. 自动构建知识图谱（关系自动提取、本体映射）
3. 因果推理支持（文档间的关系链）
4. 跨 agent 知识共享
5. 本地运行，模型无关
6. **已有 `@cognee/cognee-openclaw` npm 包**

**集成方案**:
```bash
# 直接安装 OpenClaw 插件
npm install @cognee/cognee-openclaw
# 或 pip 安装 core
pip install cognee
```

**对 OpenClaw 的价值**:
- 补全知识图谱缺口：自动提取关系
- 补全因果推理：文档间的关系链可回答 "为什么 A 导致了 B"
- 补全跨会话关联：持久化知识不再孤立
- **最关键**：已有 OpenClaw 插件，集成成本极低

**风险评估**: 中型项目，依赖 Python 3.10-3.13，需要 LLM API（可复用 dashscope）

### 🥈 MCP Memory Service — 因果知识图谱

**GitHub**: `doobidoo/mcp-memory-service`, Stars ~300, Language: Python
**描述**: "REST API + knowledge graph + autonomous consolidation"

**核心能力**:
1. 类型化因果边（causes/fixes/contradicts/depends_on）
2. MCP 协议原生支持
3. 多 agent 知识共享（X-Agent-ID 隔离）
4. 知识图谱自动合并

**集成方案**: 作为 MCP server 运行，任何 MCP 兼容客户端可直接调用

**对 OpenClaw 的价值**:
- 这个方案与我们已经设计的 `memory/graph/edges.jsonl` 理念完全一致，但它更完善
- 如果我们不想自己造轮子，可以直接部署这个

**风险评估**: 需要 Docker 或 Python 环境，较年轻的项目

### 🥉 Mem0 — 自动事实提取标杆

**GitHub**: `mem0ai/mem0`, Stars ~25k, Language: Python
**描述**: "Universal memory layer for AI Agents"

**核心能力**:
1. 单一 ADD-only 提取（一次 LLM 调用提取所有新事实）
2. 实体链接（entity linking）
3. 混合检索（语义+BM25+实体匹配）
4. 全平台 SDK（Python/JS）

**集成方案**: 可以通过 Python SDK 或 REST API 集成

**对 OpenClaw 的价值**:
- 直接解决「自动事实提取」缺口
- 但需要托管服务或本地部署

## 三、OpenClaw 现行 vs 业界最佳 — 逐维对比

| 维度 | 当前方案 | 业界最佳 | 差距评分 | 可升级路径 |
|------|---------|---------|---------|-----------|
| **会话记忆** | lossless-claw DAG | lossless-claw DAG | 🟢 9/10 | 已是最佳 |
| **向量存储** | LanceDB (memory-lancedb) + dashscope | Cognee (LanceDB+Neo4j) / Mem0 | 🟡 6/10 | 激活 LanceDB ✅，缺图存储 |
| **知识图谱** | typed edges JSONL (刚建) | Cognee 自动图 / MCP Memory Service | 🔴 2/10 | **集成 cognee** |
| **自动事实提取** | extract_facts.py (启发式) | Mem0 自动 ADD-only | 🔴 2/10 | 集成 Mem0 SDK 或 cognee |
| **因果推理** | 无 | Cognee 图推理 / M-Flow 图评分 | 🔴 1/10 | **集成 cognee** |
| **跨会话关联** | MemPalace 全文 | Cognee 持久化图 | 🟡 5/10 | 升级 |
| **用户画像** | 静态 USER.md | Mem0 自动提取 + 更新 | 🔴 1/10 | 自动提取后更新 |
| **记忆压缩** | lossless-claw 摘要 | lossless-claw 摘要 | 🟢 9/10 | 已是最佳 |
| **多 agent 共享** | 手工 handoff | Cognee cross-agent 共享 | 🔴 2/10 | 升级 |

**总体：6/10 → 目标 8.5/10**（集成 cognee 后）

## 四、推荐升级路线图

### Phase 1：立即可做（今晚，30min）

| # | 行动 | 预期收益 |
|---|------|---------|
| 1 | `pip install cognee` → 验证核心功能 | 验证可行性 |
| 2 | `npm install @cognee/cognee-openclaw` → 测试插件 | OpenClaw 直接集成 |
| 3 | 在 SESSION-STATE.md 记录集成状态 | 可追溯 |

### Phase 2：深度集成（1-2周，2-4h）

| # | 行动 | 说明 |
|---|------|------|
| 1 | 部署 cognee 本地服务（Python + LanceDB/Neo4j） | 作为 OpenClaw 的记忆后端 |
| 2 | 配置 cognee 使用 dashscope 作为 LLM backend | 复用现有 API |
| 3 | 迁移现有的 typed edges JSONL → cognee graph | 保留已有知识 |
| 4 | 测试 "为什么上次选了 X" 类因果查询 | 验收 |

### Phase 3：完善（1月+，4-8h）

| # | 行动 |
|---|------|
| 1 | 自动事实提取 + 自动关系图生成全自动化 |
| 2 | 跨 agent 记忆共享（main ↔ sentinel ↔ quant） |
| 3 | 记忆质量评估（召回率、因果准确率） |
| 4 | USER.md 自动更新（偏好动态化） |

## 五、总结

**最大发现**：**cognee 已有 OpenClaw 插件** (`@cognee/cognee-openclaw`)，这是补全知识图谱缺口的最快路径。它和我们自建的 typed edges JSONL 互补——cognee 做自动图生成和推理，JSONL edges 做手工记录和可审计性。

不引入超重型方案（GraphRAG/Neo4j集群/TrustGraph），而是用：
1. **cognee** → 知识图谱 + 图推理
2. **memory-lancedb**（已有）→ 向量存储  
3. **extract_facts.py** → 轻量手工/启发式补充
4. **typed edges JSONL** → 审计和手工关联
