# GitHub Trending 报告生成方法论

> 从 GitHub Trending 页面（https://github.com/trending）抓取数据并生成专业、详细、实用的趋势分析报告。

---

## 1. 报告结构模板

```markdown
# 🔥 GitHub Trending 报告

**生成时间：** {YYYY-MM-DD HH:mm} (Asia/Shanghai)
**数据来源：** https://github.com/trending?since={daily|weekly|monthly}
**统计周期：** {今日/本周/本月}

---

## 📊 执行摘要

| 指标 | 数值 |
|------|------|
| Trending 仓库总数 | {N} |
| 最热门语言 | {language}（{count} 个仓库） |
| 今日最热仓库 | [{owner}/{repo}](https://github.com/{owner}/{repo}) — ⭐ +{stars} stars |
| 新星项目 | {count} 个仓库首次进入 Trending |
| 报告周期 | {daily / weekly / monthly} |

---

## 🔥 Top 25 Trending 仓库

| # | 仓库 | ⭐ Stars | 📈 今日新增 | 🍴 Forks | 💻 语言 | 描述 |
|:--:|------|--------:|----------:|--------:|--------|------|
| 1 | [owner/repo](url) | 12.3k | +1,234 | 567 | Python | A brief description |
| 2 | ... | ... | ... | ... | ... | ... |

---

## 🔍 Top 5 深度分析

### 1. [owner/repo](https://github.com/owner/repo)

| 属性 | 值 |
|------|-----|
| **总 Star 数** | X |
| **今日新增** | +Y |
| **语言** | Python / TypeScript / ... |
| **许可证** | MIT / Apache-2.0 / ... |
| **首次提交** | YYYY-MM-DD |
| **贡献者数** | N |

**项目概述：** {一句话描述项目做什么}

**为什么火了：**
1. {原因 1：解决了什么痛点/提供了什么能力}
2. {原因 2：与大趋势的关联——AI/云原生/安全等}
3. {原因 3：社区/公司/事件驱动}

**核心特性：**
- {特性 1}
- {特性 2}
- {特性 3}

**社区反应：**
- Issues 活跃度：{活跃/一般/低}
- PR 合并速度：{快/正常/慢}
- 近期关键讨论：{话题}

**适合人群：** {前端开发者 / 后端 / 数据科学家 / DevOps / 所有人}

---

### 2-5. {同上格式}

---

## 📈 语言分布

| 语言 | 仓库数 | 占比 | 代表项目 |
|------|:------:|:----:|---------|
| Python | 6 | 24% | owner/repo |
| TypeScript | 5 | 20% | owner/repo |
| Rust | 3 | 12% | owner/repo |
| Go | 2 | 8% | owner/repo |
| ... | ... | ... | ... |

```
语言分布图（文本柱状）：
Python     ████████████████████████ 24%
TypeScript ████████████████████     20%
Rust       ████████████             12%
Go         ████████                  8%
...
```

---

## 🧑‍💻 Trending 开发者（可选）

> 注：GitHub Trending 的 Developers 标签页展示当日热门开发者。

| 开发者 | 仓库 | 今日新增 Stars | 主要语言 |
|--------|------|:------------:|:------:|
| [username](url) | repo-name | +X | Python |
| ... | ... | ... | ... |

---

## 🔬 趋势洞察

### 本周热门主题
1. **{主题名}** — {简要说明及代表项目数}
2. **{主题名}** — {简要说明及代表项目数}
3. **{主题名}** — {简要说明及代表项目数}

### 新兴技术栈
- {技术 1}：{为什么值得关注}
- {技术 2}：{为什么值得关注}

### 值得关注的新项目
| 仓库 | 创建时间 | 当前 Stars | 亮点 |
|------|:------:|:--------:|------|
| owner/repo | 3 天前 | 2.5k | {一句话亮点} |
| owner/repo | 1 周前 | 4.8k | {一句话亮点} |

### 与上期对比
- 📈 上升主题：{topic}
- 📉 下降主题：{topic}
- 🆕 新出现：{topic}

---

## 💡 行动建议

### 🧠 值得学习的技术
1. **{技术/项目}** — {学习价值说明}
2. **{技术/项目}** — {学习价值说明}

### 🔧 推荐试用的项目
1. **{project}** — {场景：个人项目 / 企业使用 / 学习参考}
2. **{project}** — {场景}

### ⚠️ 需要注意的风险
- {项目}：{风险——如：API 不稳定、文档不全、社区太小}
- {项目}：{风险}

---

## 📎 附录

### 数据获取时间
- 抓取时间：{时间戳}
- 页面来源：{URL}

### 免责声明
- 数据来自 GitHub Trending 页面，仅反映特定时间点的趋势
- Star 数等指标为近似值，精确数据请以 GitHub API 或仓库页面为准
- 本报告为 AI 自动生成，分析建议仅供参考

---
*报告由 y-github 技能自动生成*
```

---

## 2. 数据获取策略

### 2.1 主数据源

| 优先级 | 方法 | URL | 说明 |
|:------:|------|-----|------|
| 1 | `web_fetch` | `https://github.com/trending` | 今日 Trending 仓库页 |
| 2 | `web_fetch` | `https://github.com/trending?since=weekly` | 本周 Trending |
| 3 | `web_fetch` | `https://github.com/trending?since=monthly` | 本月 Trending |
| 4 | `web_fetch` | `https://github.com/trending/developers` | Trending 开发者页 |

### 2.2 语言过滤

GitHub Trending 支持按语言过滤：
- `https://github.com/trending/python?since=daily`
- `https://github.com/trending/typescript?since=weekly`
- `https://github.com/trending/rust?since=daily`
- 等

当用户指定语言时，使用对应的过滤 URL。

### 2.3 备选数据源（当 web_fetch 效果不佳时）

| 方法 | 说明 |
|------|------|
| `web_search("github trending repositories 2026")` | 搜索引擎获取趋势汇总 |
| `web_search("site:github.com trending this week")` | 搜索相关的趋势汇总博客/文章 |
| GitHub Explore API（如可用） | `https://api.github.com/search/repositories?q=created:>YYYY-MM-DD&sort=stars` |

### 2.4 内容解析策略

从 HTML 中提取的关键选择器（GitHub Trending 页面结构）：

```
仓库卡片：article.Box-row
├── h2.h3.lh-condensed → 仓库名 (owner / repo)
├── p.col-9.color-fg-muted → 描述
├── 语言信息：span[itemprop="programmingLanguage"] → 语言
├── Star 数：第一个 Link--muted → 总 Star 数
├── Fork 数：第二个 Link--muted → 总 Fork 数
└── 今日新增：span.d-inline-block.float-sm-right → 今日新增 Star 数
```

**解析步骤：**
1. 从 `web_fetch` 返回的文本中按 `owner / repo` 模式提取仓库名
2. 匹配 Star 数模式（如 `12,345` 或 `1.2k`）
3. 匹配语言关键词列表
4. 提取描述文本
5. 对提取的数据进行交叉验证

---

## 3. 分析框架

### 3.1 热度评分（综合加权）

| 因子 | 权重 | 计算方式 |
|------|:---:|---------|
| 今日 Star 增量 | 40% | 归一化到 0-1 |
| 总 Star 数（相对值） | 20% | log(总星数) 归一化 |
| Star/Fork 比 | 15% | Star/Fork 比值反映质量 |
| 项目年龄（新项目加分） | 15% | 越新分数越高 |
| 社区活跃度 | 10% | Issues/PR 活跃度 |

### 3.2 趋势分类

| 类别 | 特征 | 示例 |
|------|------|------|
| 🚀 爆发型 | 新项目，Star 单日增长 > 总星数的 50% | 新开源的项目突然走红 |
| 📈 稳步增长型 | 持续多天在 Trending，Star 增长稳定 | 成熟项目持续获得关注 |
| 🔥 事件驱动型 | 因某事件/发布/新闻突然冲榜 | 大版本发布、安全漏洞披露等 |
| 💎 新星 | 一周内创建，已进入 Trending | 新项目的强力开局 |

### 3.3 主题聚类

对仓库描述和 README 进行关键词提取，聚类常见主题：

| 主题 | 关键词 |
|------|--------|
| AI/LLM | llm, gpt, ai, agent, rag, embedding, transformer, copilot, chatgpt, openai, claude |
| 云原生 | kubernetes, docker, container, serverless, cloud, k8s, helm, terraform |
| 前端框架 | react, vue, angular, svelte, nextjs, nuxt, astro, component |
| 后端框架 | django, fastapi, spring, gin, echo, express, nest, laravel |
| DevOps | cicd, pipeline, monitoring, observability, logging, alerting |
| 安全 | security, vulnerability, scanner, firewall, pentest, exploit |
| 数据库 | postgres, mysql, mongodb, redis, sqlite, vector, surrealdb |
| Rust 生态 | rust, cargo, wasm, systems, embedded |
| 开发者工具 | cli, editor, terminal, vscode, neovim, ide, plugin |
| 数据科学 | pandas, numpy, pytorch, tensorflow, ml, data, analytics |

---

## 4. 报告生成工作流

```
1. 确定报告范围
   ├── 用户指定：语言 / 周期(daily/weekly/monthly) / 是否含开发者
   ├── 默认：全语言 daily
   └── 高级：多语言对比报告
   
2. 数据采集
   ├── web_fetch("https://github.com/trending[?since=...][/language]")
   ├── 解析 HTML → 提取结构化数据
   ├── 验证数据完整性（至少获取到 20+ 仓库）
   └── [可选] web_fetch("https://github.com/trending/developers[?since=...]")
   
3. 深度分析（Top 5 仓库）
   ├── web_fetch("https://github.com/{owner}/{repo}") → 获取 README 摘要
   ├── 提取：许可证、创建时间、Issue 数、PR 数
   ├── 分析：项目成熟度、社区活跃度
   └── 综合：热度评分 + 主题归类
   
4. 报告生成
   ├── 按模板填充结构化数据
   ├── 生成趋势洞察（主题聚类、对比分析）
   ├── 生成行动建议
   └── 输出 Markdown 格式报告
   
5. 质量控制
   ├── 数据一致性检查（总数 vs 语言分布一致）
   ├── 至少 5 条深度分析
   ├── 至少 3 条趋势洞察
   └── 至少 3 条行动建议
```

---

## 5. 输出规格

### 5.1 最小可行报告（轻量模式）

当时间/Token 受限时，输出精简版：

```markdown
# 🔥 GitHub Trending 速报 — {日期}

## Top 10
{10 行表格：排名 + 仓库名 + 描述 + Star + 语言}

## 快速洞察
- 热门主题：{1-3 个主题}
- 推荐关注：{1-3 个项目}

## 报告全文
→ 请求 "详细报告" 获取完整分析
```

### 5.2 完整报告（标准模式）

按 §1 模板输出，包含所有章节。

### 5.3 多语言对比报告（高级模式）

当用户请求对比多个语言的 Trending 时：

```markdown
# 🔥 GitHub Trending 多语言对比 — {日期}

## Python vs TypeScript vs Rust

| 维度 | Python | TypeScript | Rust |
|------|--------|-----------|------|
| Trending 仓库数 | X | Y | Z |
| 平均 Star 增量 | +X | +Y | +Z |
| Top 1 仓库 | repo | repo | repo |
| 主导主题 | AI/LLM | Web/工具 | 系统/工具 |

## 各语言 Top 5
...
```

---

## 6. 错误处理

| 场景 | 处理方式 |
|------|---------|
| `web_fetch` 返回不完整 | 重试 1 次；仍不完整则降级为 web_search 方案 |
| 页面结构变化 / 解析失败 | 尝试备选解析模式；报告"部分数据可能不完整" |
| 无法获取开发者 Tab | 仅生成仓库报告，标注"开发者数据不可用" |
| 用户指定语言无结果 | 提示该语言可能今日无 Trending 项目，建议扩大范围 |
| 网络超时 | 提示稍后重试，使用 web_search 获取替代数据 |
| 获取仓库详情时限流 | 仅对 Top 3 做深度分析，其余仅基础信息 |

---

## 7. 使用示例

### 示例 1：今日全语言 Trending 报告

```
用户：帮我生成一份 GitHub Trending 报告

执行：
1. web_fetch("https://github.com/trending") → 解析得到 25 个仓库
2. 对 Top 5 逐一下载 README 摘要
3. 按模板生成完整报告
4. 输出 Markdown 报告 + 保存为 trending-report-2026-10-20.md
```

### 示例 2：Python 本周 Trending

```
用户：看看这周 Python 有什么热门项目

执行：
1. web_fetch("https://github.com/trending/python?since=weekly") → 解析
2. 识别主题：AI Agent 框架 ×3、数据处理 ×2、Web 框架 ×1...
3. 输出精简报告 + 趋势洞察
```

### 示例 3：多语言对比

```
用户：对比 Python 和 Rust 今天的热门趋势

执行：
1. 并行 web_fetch（Python trending + Rust trending）
2. 解析两份数据
3. 生成对比报告——分析两个生态的关注点差异
```

---

> **参考文件版本**：v1.0.0 | **作者**：YrY Team | **创建日期**：2026-10-20
>
> *本文件是 y-github 技能 Trending 报告功能的技术参考，定义了报告结构模板、数据获取策略、分析框架和输出规格。*