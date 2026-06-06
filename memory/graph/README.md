# Memory Graph 使用指南

## 节点类型
- **project**: 项目/系统（如 sellersprite）
- **entity**: 实体/账号/工具（如 dashscope, notion）
- **concept**: 概念/规则（如 SPR 黄金法则）
- **person**: 人物（如 rhui1987）

## 边类型 (typed edges)

| 类型 | 含义 | 例子 |
|------|------|------|
| `depends_on` | A 依赖于 B | sellersprite → spr-strategy |
| `causes` | A 导致 B | memory-lancedb disabled → memory sloted default |
| `fixes` | A 修复 B | --force fix → virus blocking |
| `contradicts` | A 与 B 矛盾 | rhui1987 ≠ 888JQJL (VIP vs 试用) |
| `relates_to` | A 与 B 一般关联 | dashscope ↔ text-embedding-v4 |

## 如何检索
```bash
# 查询某个实体的所有关联
grep '"source":"sellersprite"' memory/graph/edges.jsonl

# 查询因果链
grep '"type":"causes"' memory/graph/edges.jsonl

# 查询所有 "修复" 关系
grep '"fixes"' memory/graph/edges.jsonl
```
