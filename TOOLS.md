# TOOLS.md - Local Notes

### Network & Proxy
- **Default Proxy**: http://192.168.100.183:7890
- **Policy**: ALL external financial API calls (yfinance, Alpha Vantage, Polygon) MUST use this proxy to avoid IP blocking.
- **Status**: Verified 2026-04-04.

### Core Data Priority & API Keys
1. **Polygon.io** (Primary US)
   - Key: `zToCOusQu3vScKzZWjx95F_Gdw0s4XtD`
2. **Alpha Vantage** (Secondary / US Indicators)
   - Key: `09SEPG1E9S9HUZ08`
3. **AKShare** (Primary HK/CN)
   - Method: Python `import akshare as ak`
4. **yfinance** (Tertiary / Fallback)

### 🔐 核心账户凭证 (Sensitive Credentials)
- **领星 ERP (erp.lingxing.com)**: `17754926259` / `Shengri1996426`
- **卖家精灵 (sellersprite.com) 唯一可用账号**: `rhui1987`（VIP会员）
  - ⚠️ 其他账号（如 888JQJL）均为试用账号，容易被限制/弹窗「超过免费试用次数」
  - 后续所有卖家精灵操作**必须**使用 `rhui1987` 登录

---
### MemPalace 旁路检索（只读）
- **仓库路径**: `~/.openclaw/workspace/cloned-repos/mempalace`
- **Palace 路径**: `~/.mempalace/palace`
- **搜索命令**: `cd <仓库路径> && source venv/bin/activate && python3 -m mempalace search "<query>" [--wing <wing>] [--room <room>] [--results <n>]`
- **版本**: 3.3.3
- **策略**: SOUL.md §记忆分层 5) 先 memory_search，再 mempalace_search

### 科普文案模板
- **模板路径**: `templates/medical-copy/钻石精雕-科普模板.docx`
- **用途**: 医美科普文章模板，用于生成类似结构的文章

### Skills 安装状态 (2026-04-27)

| Skill | 安装 | 配置 | 说明 |
|-------|------|------|------|
| **multi-search-engine** | ✅ 已有 | ✅ 无密钥 | 开箱即用，17个搜索引擎 |
| **notion** | ✅ 已有 | ❌ 需 Notion API Key | 需创建 integration 并配置 `~/.config/notion/api_key` |
| **auto-updater** | ✅ 已安装 | ⚠️ 需设置 cron | 需要配置 cron job 开启自动更新 |
| **desktop-control** | ✅ 已有 | ✅ 依赖已安装 | PyAutoGUI + Pillow 已就绪，OpenCV 可选 |
| **elite-longterm-memory** | ✅ 已有 | ❌ 需 OpenAI API Key + memory-lancedb 插件 | 需要 `OPENAI_API_KEY` 环境变量，需启用 `memory-lancedb` 插件 |
| **moltspeak** | ✅ 已安装 | ✅ 纯协议文档 | 信息型 skill，无需配置 |
| **tencent-docs** | ✅ 已安装 | ❌ 需腾讯文档授权 | 需运行 `bash setup.sh tdoc_check_and_start_auth` 完成 OAuth |
| **tushare-finance** | ✅ 已安装 | ❌ 需 Tushare Token | 需设置环境变量 `TUSHARE_TOKEN` |

### File Operation & Memory Guidelines
- **Memory Promotion**: DO NOT leave config (API Keys, Proxy) in `working-buffer.md`. Promote IMMEDIATELY to `TOOLS.md`.
- **Reliability**: Use `read` before `edit` for exact match.
