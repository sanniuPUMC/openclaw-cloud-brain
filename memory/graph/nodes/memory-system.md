# 记忆体系 — 当前架构

## 层
1. **热内存**: SESSION-STATE.md (WAL)
2. **向量存储**: memory-lancedb (LanceDB + dashscope text-embedding-v4)
3. **语义搜索**: memory_search (dashscope embedding)
4. **无损存档**: lossless-claw DAG
5. **反思**: reflection-compiler (playbooks)
6. **图记忆**: memory/graph/ (typed edges) ← 新增
7. **旁路检索**: MemPalace (全文)

## Provider 与 API
- **所有 embedding**: dashscope text-embedding-v4（阿里云，1024维）
- **不使用**: OpenAI API（全部用国内中转）
- **LanceDB**: 本地嵌入式向量数据库
