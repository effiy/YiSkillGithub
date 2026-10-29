---
name: YiSkillGithub
title: "y-github — GitHub 平台操作：Issue/PR/Release 全生命周期"
description: >
  GitHub 平台操作——通过 GitHub MCP Server 执行 Issue/PR 全生命周期管理、
  代码搜索、仓库文件操作（CRUD/Fork）、分支/提交/标签管理（远程侧）、
  Release 检查、用户/团队查找、Secret 凭证扫描、GitHub Trending 趋势报告生成、
  以及 Bug→PR 复合工作流。

  **触发（即使不说"GitHub"）：**
  创建/搜索/更新/关闭 Issue；创建/审查/合并 Pull Request；在 GitHub 上搜索代码；
  查看 Release/Tag；管理远程分支；Fork 仓库；创建仓库；团队协作操作；
  Secret 扫描检查凭证泄露；Copilot PR review；需要将 Bug 报告转为 Issue 再提 PR；
  生成 GitHub Trending 趋势报告、查看今日/本周/本月热门仓库。

  触发短语：「创建 Issue」「提 PR」「Pull Request」「搜代码」
  「查看 Release」「GitHub 仓库」「GitHub Actions」「管理 Issue」
  「搜索 GitHub」「查看提交」「分支管理」「PR review」「merge 一下」
  「secret scanning」「代码搜索」「GitHub search」「Copilot review」
  「fork 这个」「建个 issue」「看下最近的 release」「谁提交的」
  「create issue」「create PR」「review PR」「merge PR」「close issue」
  「GitHub Trending」「今日热门」「趋势报告」「trending」「本周热门仓库」
  「trending report」「生成趋势报告」。

  **路由：** 本地 Git 操作（commit/stash/rebase）→ 使用 Git 命令；
  GitHub 平台操作（Issue/PR/仓库/Release/团队）→ 本技能。

user_invocable: true
updated: 2026-10-19
lifecycle: active
auto-trigger-rules:
  - 用户需要创建、搜索、更新 GitHub Issue
  - 用户需要创建、审查、合并 Pull Request
  - 涉及 GitHub 仓库操作（文件 CRUD、Fork、创建仓库、代码搜索）
  - 用户需要查看 Release 或管理远程分支/标签
  - 用户提到 Secret 扫描或凭证泄露检查
  - 用户需要生成 GitHub Trending 趋势报告或查看热门仓库
priority: medium
tags: [skill, github, collaboration, issue, pr, repository, code-search, secret-scanning]
status: stable
type: skill
source: internal
created: 2026-08-10
category: aier/skills/y-github
review_cycle: quarterly
roles: [aier]
chip: ai-methodology
---

# GitHub 平台操作

> **GitHub 平台操作的权威工作流。** 通过 GitHub MCP Server 执行 Issue、PR、Release、仓库管理、代码搜索、安全扫描及趋势分析——涵盖从简单查询到多步骤复合工作流的全生命周期。本技能仅操作 GitHub 远程平台，不涉及本地 Git 仓库。

## 决策指南：选择正确的 GitHub 操作

在开始操作前，先确定正确的交互类型：

```
收到 GitHub 相关请求
    │
    ├── 报告 Bug / 提功能请求 / 讨论设计？
    │   ├── 具体可执行的 Bug / 功能 → Issue
    │   ├── 开放讨论 / 设计决策 → Discussion（Discussions 标签页）
    │   └── 有代码修复方案 → 直接创建 PR（关联 Issue）
    │
    ├── 有代码变更需要合并？
    │   ├── 变更 < 500 行，单一目的 → PR
    │   ├── 变更 > 500 行 → 拆分为多个小 PR
    │   └── 变更未完成 → Draft PR
    │
    ├── 准备发布版本？
    │   ├── 代码已合并到 main → Release + Tag
    │   ├── 需要 Release Notes → 检查合并的 PR 摘要
    │   └── 需要预发布 → Pre-release
    │
    └── 查找/搜索信息？
        ├── 代码片段搜索 → search_code（跨仓库全文搜索）
        ├── Issue/PR 搜索 → search_issues（关键词+标签+状态）
        └── 趋势/热门项目 → Trending 报告（web_fetch+分析）
```

## Quick-Start

```
get_me（确认身份）→ search（避免重复）→ operate（执行操作）→ verify（验证结果）
```

**首步准则：** 任何会话中首次 GitHub 操作前，必须调用 `get_me` 确认当前认证用户身份和权限范围。身份未确认的情况下，Issue 指派、PR 审查者分配、权限边界判断都可能产生静默错误。

## 能力范围

### Issue 管理
创建、搜索（关键词/标签/状态）、读取详情、更新内容/状态/指派人、关闭、管理子 Issue 和自定义字段。

### Pull Request 管理
创建 PR（含描述/关联 Issue/审查者）、读取 diff/评论、三步审查工作流（查看变更→逐文件评论→提交审查结论）、合并（merge/squash/rebase）、更新分支、请求 Copilot review。

### 仓库与文件操作
文件 CRUD（创建/读取/更新/删除）、批量推送、Fork 仓库、创建新仓库、代码搜索（跨仓库全文搜索）。

### 分支/提交/标签管理（远程侧）
列出/创建/删除分支、查看提交历史/diff、创建/删除标签。

### Release 检查
列出 Release、获取最新 Release、按标签获取指定 Release。

### 用户与团队
`get_me`（当前用户信息）、列出团队、列出团队成员、用户搜索。

### Secret 扫描
检测 diff 和文件内容中的：API 密钥、数据库密码、JWT Secret、Token、私钥。主动扫描——在提交/合并前自动检查。

### Trending 趋势报告

从 GitHub Trending 页面（https://github.com/trending）生成专业趋势分析报告——支持 daily/weekly/monthly 周期、按语言过滤、多语言对比。报告包含：Top 25 热门仓库、Top 5 深度分析（项目概述/为什么火/核心特性/社区反应/适合人群）、语言分布、趋势洞察（热门主题/新兴技术栈/新星项目）、行动建议。

数据获取策略：优先使用 `web_fetch` 直接抓取 Trending 页面解析；备选 `web_search`。对 Top 仓库自动获取 README 摘要进行深度分析。详细报告模板和分析框架参见 [references/trending-report.md](references/trending-report.md)。

#### 趋势报告产出规格

| 报告元素 | 最低要求 | 完整模式 |
|----------|----------|----------|
| 仓库排名 | Top 10 | Top 25 |
| 深度分析 | Top 3（简要） | Top 5（五维度完整分析） |
| 语言分布 | 文本列表 | 文本柱状图 + 百分比 |
| 趋势洞察 | 至少 3 条 | 至少 5 条 + 主题聚类 |
| 新星标注 | — | 标注一周内创建即上榜的项目 |
| 行动建议 | 至少 3 条 | 至少 5 条 + 学习路径 |

#### 热度综合评分公式

```
热度分 = Star增量(40%) + 总星数(20%) + Star/Fork比(15%) + 项目年龄(15%) + 社区活跃度(10%)
```

- **Star 增量**：今日/本周新增 Star 数（归一化到当日最高值）
- **总星数**：历史累计 Star 数的对数归一化
- **Star/Fork 比**：高质量项目通常 Star/Fork > 5
- **项目年龄**：新项目加权更高（3 个月内 ×1.5，1 年内 ×1.0，3 年以上 ×0.5）
- **社区活跃度**：最近 7 天 commits 数 + Issues/PRs 活跃数

### 复合工作流
- **Bug→PR 流水线：** 从 Bug 报告创建 Issue → 创建修复分支 → 提交代码 → 创建 PR → 关联 Issue → 审查 → 合并
- **安全审计：** 全仓库 Secret 扫描 → 发现→轮换密钥 → 验证无残留
- **Release 发布：** 合并 PR → 打 Tag → 创建 Release → 生成 Release Notes
- **Trending 趋势报告：** 抓取 Trending 页面 → 解析结构化数据 → 深度分析 Top 仓库 → 主题聚类 → 生成专业报告

## 工作流详解

### get_me — 确认身份（会话首次必做）

在会话中首次执行 GitHub 操作前，必须确认当前认证用户的身份和权限范围。身份不明会导致 Issue 指派错误、PR 审查者无效、以及超出权限范围的操作静默失败。

**命令：** 调用 GitHub MCP 的 `get_me` 工具。  
**输出：** 用户名、权限范围（repo / user / org 级别）。  
**后续决策：** 如果用户请求的操作超出当前权限范围，不尝试绕过——提示用户检查 GitHub Token 的 scope 配置。

### search — 避免重复（创建操作前必做）

Issue 和 PR 的重复创建是开源协作中最大的效率损耗之一。如果已有讨论或修复在进行中，你的新 Issue/PR 不仅是噪音，还会分散维护者的注意力。即使内容不完全相同，已有讨论也可能提供关键的决策上下文。

**搜索 Issue：** `search_issues` — 按关键词 + 标签 + 状态组合过滤。搜索模式：先用宽泛关键词扫描，再用精确短语精确定位。  
**搜索代码：** `search_code` — 跨仓库全文搜索。使用 `repo:` 限定符和语言过滤缩小范围。  
**搜索 PR：** `search_issues` + `type:pr` 过滤 — 确认是否已有类似的修复分支在进行。

**搜索策略：**

| 搜索目标 | 首次尝试 | 无结果时的备选 |
|----------|----------|----------------|
| Bug Issue | 错误消息关键字 + label:bug | 栈跟踪关键词 + state:all（含已关闭） |
| Feature Request | 功能名称 + label:enhancement | 功能描述关键词（不限于标题） |
| 已存在的 PR | 文件名 + type:pr | 关联 Issue 号 + type:pr |

### Issue 模板

创建 Issue 时，根据类型使用对应模板。以下是两种核心模板：

#### Bug Report 模板

```markdown
## 问题描述
[清晰描述 Bug 的表现——实际发生了什么，而非预期发生什么]

## 复现步骤
1. [第一步]
2. [第二步]
3. [第三步]

## 预期行为
[在正常情况下系统应该怎样响应]

## 实际行为
[当前系统实际发生了什么——含错误消息、状态码、UI 异常]

## 环境信息
- 版本/提交: [commit hash 或版本号]
- 浏览器/运行时: [Chrome 120 / Node.js 20 / Python 3.12]
- 操作系统: [macOS 14 / Ubuntu 22.04]

## 附加上下文
[日志片段、截图链接、最小复现仓库]
```

#### Feature Request 模板

```markdown
## 动机
[描述当前面临的问题或限制——为什么需要这个功能]

## 提案
[描述期望的功能行为——具体而明确的功能描述]

## 备选方案
[已考虑过的替代方案及其不足——证明提案的必要性]

## 影响范围
[此功能会影响哪些模块/用户——评估变更风险]
- 影响模块: 
- 向后兼容: 是 / 否
- 需要数据迁移: 是 / 否

## 附加上下文
[设计稿链接、竞品参考、相关讨论 Issue 号]
```

### PR 描述模板

```markdown
## 变更摘要
[一段话概括本次 PR 做了什么变更，以及为什么要做这个变更]

## 关联 Issue
Fixes #[Issue号]

## 变更详情
- [关键变更点 1 —— 说明为什么这样做而非怎么做]
- [关键变更点 2]
- [关键变更点 3]

## 测试
- [ ] 单元测试通过（新增 [N] 个测试用例）
- [ ] 集成测试通过
- [ ] 手动验证：复现步骤已通过
- [ ] 回归测试：相关功能无退行

## 自查清单
- [ ] PR 变更行数 ≤ 500 行（超过则已拆分为多个小 PR）
- [ ] 无 Secret / Token / 密钥硬编码（secret_scanning 已通过）
- [ ] 分支命名遵循规范（feature/fix/release 前缀）
- [ ] 代码已通过本地 lint 和 format

## 审查重点
- [建议审查者特别关注的文件或逻辑——标注高风险变更点]
```

### operate — 执行操作
按需调用对应的 MCP 工具。关键原则：
- **分页查询：** `page`/`perPage`，建议 5-10 条/页——避免一次拉取过多数据
- **最小化输出：** 不需要完整详情时使用 `minimal_output: true`——节省 Token
- **文件更新需 SHA：** 更新文件前必须先用 `get_file_contents` 获取当前 SHA——防止覆盖他人修改
- **主动 Secret 扫描：** 在提交/合并前自动扫描变更内容的凭证泄露

### verify — 验证结果
写操作后立即验证：
- 创建 Issue → 搜索确认 Issue 存在且内容正确
- 合并 PR → 检查目标分支是否包含合并的提交
- 更新文件 → 读取文件确认内容已更新

### trending-report — 生成趋势报告
从 GitHub Trending 页面生成专业趋势分析报告。五步流程：

**步骤 1：确定报告范围**
- 周期：daily（默认）/ weekly / monthly
- 语言：全语言（默认）/ 指定语言（如 python、rust、typescript）
- 类型：仓库排名 / 开发者排名 / 多语言对比

**步骤 2：数据采集**
- 主方案：`web_fetch("https://github.com/trending[?since=...][/language]")`
- 备选方案：`web_search("github trending repositories")` 获取趋势汇总
- 采集后解析 HTML 提取结构化数据（仓库名、描述、Star 数、语言、Fork 数、今日新增）

**步骤 3：深度分析**
- 对 Top 5 仓库逐个 `web_fetch("https://github.com/{owner}/{repo}")` 获取 README 摘要
- 分析维度：项目概述、为什么火、核心特性、社区反应、适合人群
- 对全部仓库做主题聚类（AI/LLM、云原生、前端框架、后端框架、DevOps 等 10 类）

**步骤 4：洞察生成**
- 语言分布统计 + 文本柱状图
- 热门主题识别（本周最火的 3 个技术方向）
- 新兴技术栈发现（增长最快的技术/框架）
- 新星项目标注（一周内创建即进入 Trending）
- 热度综合评分（Star 增量 40% + 总星数 20% + Star/Fork 比 15% + 项目年龄 15% + 社区活跃度 10%）

**步骤 5：报告输出**
- 按模板生成 Markdown 报告（模板详见 [references/trending-report.md](references/trending-report.md)）
- 报告包含：执行摘要 → Top 25 → Top 5 深度分析 → 语言分布 → 趋势洞察 → 行动建议
- 支持轻量模式（仅 Top 10 + 快速洞察）和完整模式（全量报告）

## 命令速查

| 操作 | 工具 | 关键参数 |
|------|---------|---------|
| 查看身份 | `get_me` | — |
| 搜索 Issue | `search_issues` | `q`, `labels`, `state`, `page`, `perPage` |
| 创建 Issue | `create_issue` | `title`, `body`, `labels`, `assignees` |
| 读取 Issue | `get_issue` | `issue_number` |
| 更新 Issue | `update_issue` | `issue_number`, `title`/`body`/`state`/`labels` |
| 搜索代码 | `search_code` | `q`（含 repo: 限定符） |
| 创建 PR | `create_pull_request` | `title`, `body`, `head`, `base` |
| 审查 PR | `create_review` | `event`（APPROVE/REQUEST_CHANGES/COMMENT）, `body` |
| 合并 PR | `merge_pull_request` | `merge_method`（merge/squash/rebase） |
| 获取 Release | `get_latest_release` | — |
| Secret 扫描 | `secret_scanning` | 自动扫描 diff 和文件内容 |
| 获取文件 | `get_file_contents` | `path`, `ref`（返回 SHA） |
| 更新文件 | `create_or_update_file` | `path`, `content`, `sha`, `message` |
| Trending 报告 | `web_fetch` + 解析 | `url`, 周期(since), 语言(language), 报告模式(light/full) |
| Trending 搜索（备选） | `web_search` | 关键词 "github trending {language} {period}" |

## GitHub 操作反模式

| 反模式 | 为什么错误 | 正确做法 | 已发生的修复步骤 |
|--------|----------|---------|-----------------|
| 创建 Issue 前不搜索 | 重复 Issue 消耗审查者带宽，降低仓库信噪比 | 先 `search_issues` 按关键词+标签搜索，确认无重复后创建 | 1. `search_issues` 确认重复 2. 在新 Issue 中引用已有 Issue 并关闭 3. 将信息追加到已有 Issue |
| PR 合并前不审查 | 未经审查的代码直接合并→线上缺陷逃逸→回滚成本远高于审查成本 | 触发 code-review 技能，P0 问题修复后再合并 | 1. 立即触发审查 2. 如已合并，创建回滚 PR 3. 事后补充审查+补丁 PR |
| 提交含密钥/Token 的代码 | 密钥进入 Git 历史→泄露且难以彻底清除（git reflog/git stash 中仍有残留） | 提交前运行 `secret_scanning`；已泄露→立即轮换密钥+BFG 清理历史 | 1. 立即在平台端轮换密钥 2. `git filter-branch` 或 BFG 清理 3. 强制推送前通知所有协作者 4. 审查不再使用的 secret |
| 单次 PR >500 行 | 审查者认知超载→走马观花式审查→漏掉严重问题 | 拆分为多个 ≤500 行的独立 PR，每个 PR 聚焦一个变更主题 | 1. `git reset --soft HEAD~1` 回到未提交状态 2. 按主题拆分为多个分支 3. 逐一提交 PR |
| PR 描述为空 | 审查者被迫从 diff 逆向推断意图→效率低、易误解变更目标 | 至少填写：变更摘要 + 关联 Issue + 测试状态 + 审查重点 | 1. `update_pull_request` 补充描述 2. 通知审查者重新审查 |
| PR 未完成但不标记 Draft | 审查者浪费时间审查半成品代码→评论可能不适用于最终版本 | 未完成→转为 Draft PR；完成→`Ready for review` | 1. `update_pull_request(draft=true)` 2. 完成后标记 ready |
| 不先 `get_me` 就操作 | 权限范围未知→Issue 指派到不存在的人→PR 审查者无效 | 会话首次操作前确认身份和权限 | 1. 立即 `get_me` 2. 用正确身份修正已操作的内容 |

### Merge 策略决策矩阵

| 场景 | 推荐 merge_method | 效果 | 不推荐的方法 |
|------|:-----------------:|------|:-----------:|
| 单次 feature 开发（多 commits） | `squash` | 合并为单个干净 commit，保持 main 线性历史 | merge commit（噪音大） |
| Bug fix（1-2 commits） | `merge` | 保留修复过程的原子性，便于 cherry-pick 到 release 分支 | squash（丢失修复细节） |
| 长期 feature 分支合并 | `merge` | 保留完整开发历史，便于审计和回滚 | squash（丢失历史上下文） |
| Release 分支合并到 main | `rebase` | 确保线性历史，Release Notes 自动生成清晰 | merge commit（引入不必要的合并节点） |
| 紧急热修复 | `merge` | 快速合并，保留完整证据链供事后分析 | squash（模糊修复过程） |

## 容错策略

| 情况 | 行为 | 恢复方式 |
|------|------|---------|
| API 速率限制（429） | 显示限制信息；建议等待或减少 `perPage` | 等待 rate limit 窗口重置 |
| 资源权限拒绝（403/404） | 显示错误码；建议检查权限范围 | 确认 token scope 或请求授权 |
| 文件更新无 SHA | 拒绝操作；提示先调用 `get_file_contents` | 先获取最新 SHA，再更新 |
| PR 模板未找到 | 降级到最小描述模板；警告缺失模板 | 手动编写描述，建议补充模板 |
| Copilot review 不可用 | 显示错误；建议手动审查 | 切换到标准三步审查流程 |
| 分页结果为空 | 显示空结果提示；检查过滤参数是否过严 | 放宽过滤条件或确认搜索关键词 |
| 非英语用户请求 | 以用户语言回复；资源标题保持原文 | — |
| 大文件变更（>1MB） | 提示文件过大，建议拆分或使用 Git LFS | 拆分文件或配置 LFS |
| Trending 页面抓取不完整 | 重试 1 次；仍不完整则降级为 `web_search` 方案 | 使用搜索引擎获取趋势汇总 |
| Trending 页面结构变化 | 尝试备选解析模式；标注"部分数据可能不完整" | 手动检查页面结构 |
| 用户指定语言无 Trending 结果 | 提示该语言可能今日无热门项目；建议扩大范围 | 回退到全语言 trending 或换周期 |
| 仓库详情抓取触发限流 | 仅对 Top 3 做深度分析，其余仅基础信息 | 减少并发请求数 |

## 输入→输出示例

### 示例 1：从 Bug 报告创建 Issue 并提 PR

**输入：** 用户报告 "YiVad 项目列表翻到第 2 页后列头丢失，每次必现。昨天 ProTable 重构了分页逻辑。"

**流程：**
```
1. search_issues("ProTable 分页 列头") → 无重复 Issue
2. create_issue(title="[Bug] ProTable 分页后列头丢失", labels=["bug","YiVad"], body=bug模板)
   → 输出：Issue #234 已创建
3. [本地修复后] create_pull_request(title="Fix #234: ProTable pagination column header reset",
   head="fix/protable-pagination-column", base="main", body="Fixes #234\n\n根因：onPageChange中误调了resetColumnConfig\n测试：新增分页+列头保持测试")
   → 输出：PR #235 已创建，关联 Issue #234
4. [CI 通过、审查通过后] merge_pull_request(merge_method="squash")
   → 输出：PR #235 已合并到 main
```

### 示例 2：发布新版本 Release

**输入：** 用户说 "YiAi v2.1.0 已合并到 main，帮我发布 Release。"

**流程：**
```
1. get_latest_release() → 当前最新是 v2.0.5
2. [确认变更日志后] create_release(tag="v2.1.0", name="YiAi v2.1.0",
   body="- 新增RPC参数白名单校验\n- 修复MongoDB连接池泄漏\n- 优化搜索索引覆盖")
   → 输出：Release v2.1.0 已发布
3. [验证] get_release_by_tag("v2.1.0") → 确认 Release 内容和 assets 正确
```

### 示例 3：生成 GitHub Trending 趋势报告

**输入：** 用户说 "帮我生成一份今天的 GitHub Trending 报告" 或 "这周 Python 有什么热门项目"

**流程：** (以全语言 daily 为例)
```
1. 确定范围 → daily，全语言，完整模式
2. web_fetch("https://github.com/trending") → 解析得到 25 个仓库的结构化数据
3. 深度分析 Top 5：
   - web_fetch("https://github.com/{owner}/{repo1}") → 提取 README、许可证、社区指标
   - web_fetch("https://github.com/{owner}/{repo2}") → ...
   - 综合评分排名，识别热度驱动因素
4. 主题聚类 → 发现本周热门：AI Agent 框架 ×4、Rust 工具链 ×3、前端框架 ×2 ...
5. 生成报告 → 按模板输出
   # 🔥 GitHub Trending 报告 — 2026-10-20
   - 执行摘要：25 个 Trending 仓库，Python 居首（6 个）
   - Top 25 排名表
   - Top 5 深度分析（每仓库含：项目概述/为什么火/核心特性/社区反应/适合人群）
   - 语言分布 + 文本柱状图
   - 趋势洞察：AI Agent 持续火热，Rust 工具链异军突起
   - 行动建议：推荐 3 个值得深入学习的项目
   
6. [验证] 抽查 Top 3 仓库的 Star 数与实际仓库页一致性
   报告保存为 trending-report-2026-10-20.md
```

**语言过滤变体：**
```
用户："这周 Python 有什么热门项目"
→ web_fetch("https://github.com/trending/python?since=weekly")
→ 解析 + 深度分析 → 输出 Python 专属趋势报告
```

**多语言对比变体：**
```
用户："对比 Python 和 Rust 今天的趋势"
→ 并行 web_fetch(python trending + rust trending)
→ 生成对比报告：Top 5 ×2、主题差异、生态洞察
```

## 与 YrY 工作流的集成

```
github（平台操作）
    │
    ├── 创建 Issue ──→ y-plan（需求分析）→ y-arch（架构决策）
    ├── 创建 PR ──→ y-dev（六维审查）→ 反馈修改 → 重新审查
    ├── 安全审查 ──→ secret_scanning（凭证扫描）→ security（威胁建模+攻击路径追踪）
    ├── 合并后 ──→ y-test（测试验证）→ CI/CD（五道门禁）
    └── 发布 Release ──→ 触发 CI/CD → 部署 → 监控验证
```

### YrY 项目约定

**分支命名规范：**

| 分支类型 | 命名格式 | 示例 |
|----------|----------|------|
| 新功能 | `feature/<描述>` | `feature/user-auth-module` |
| Bug 修复 | `fix/<Issue号>-<简述>` | `fix/234-protable-column-reset` |
| 发布分支 | `release/<版本>` | `release/v2.1.0` |
| 热修复 | `hotfix/<Issue号>-<简述>` | `hotfix/301-payment-timeout` |

**PR 标题前缀：**

| 前缀 | 影响项目 | 使用场景 |
|------|----------|----------|
| `[YiAi]` | 后端 AI 服务 | AI 模型推理、API 网关、RPC 服务 |
| `[YiVad]` | 前端管理平台 | 组件库、ProTable、状态管理 |
| `[YiPet]` | 宠物社交应用 | 移动端 UI、实时通信、多模态交互 |
| `[Infra]` | 基础设施 | CI/CD、监控、Docker、部署脚本 |

**合并策略：**

| PR 类型 | merge_method | commit 消息格式 | 原因 |
|--------|:-----------:|----------------|------|
| feature | `squash` | `feat(scope): 简短描述 (#PR)` | 保持 main 线性历史 |
| bugfix | `merge` | 保留原始 commits | 便于 cherry-pick 到 release |
| release | `rebase` | 线性历史 | Release Notes 自动生成 |
| hotfix | `merge` | 保留完整证据链 | 事后分析需完整上下文 |

**PR 关联规则：**
- 每个 PR 必须关联至少一个 Issue（在描述中使用 `Fixes #N` 或 `Closes #N`）
- 无关联 Issue 的 PR 需在描述中解释原因（如：重构、文档修正）
- 一个 PR 可以 `Fixes` 多个 Issue（但建议一个 PR 只解决一个问题）

### 安全优先工作流

涉及敏感操作时，遵循以下增强工作流：

```
提交代码
    │
    ├── 1. secret_scanning（扫描 diff 中的凭证泄露）
    │   ├── 通过 → 继续
    │   └── 告警 → 阻止提交，轮换密钥后重新扫描
    │
    ├── 2. 检查 .gitignore（确认敏感文件不会入库）
    │   ├── .env / *.pem / credentials.json / *.key 已忽略？
    │   └── 未忽略 → 追加到 .gitignore → 确认未跟踪状态
    │
    ├── 3. 检查文件权限（chmod 600 对密钥文件）
    │
    └── 4. 提交 → PR → code-review → merge
```

### 操作验证清单

每次 GitHub 操作完成后，对照以下检查项验证：

- [ ] `get_me` 已执行，身份和权限范围已确认
- [ ] 创建操作前已执行搜索，确认无重复
- [ ] Issue/PR 描述使用对应模板，所有必填字段已填写
- [ ] 分支命名符合 YrY 规范
- [ ] PR 已关联至少一个 Issue（`Fixes #N`）
- [ ] 提交前 secret_scanning 已通过
- [ ] 写操作后已执行验证（创建→搜索确认；合并→检查提交；更新→读取确认）
- [ ] 复合工作流中上一步成功后才执行下一步

## 停止条件

操作完成判定标准——以下条件全部满足方可认为操作已完结：

- [ ] **可验证性**：操作结果已验证（创建→可搜索到；合并→目标分支含提交；更新→读取确认内容一致）
- [ ] **写入安全**：写入前已确认范围和影响（Issue 不重复、PR 描述完整、secret_scanning 无告警）
- [ ] **权限处理**：权限错误不重试——提示用户检查 GitHub Token 的 scope 配置，不尝试绕过
- [ ] **复合工作流**：上一步验证通过后才执行下一步——不跳过中间验证直接 sprint 到终点
- [ ] **趋势报告**：至少采集 20+ 仓库数据、至少 5 条深度分析、至少 3 条趋势洞察和行动建议

## 原则

### 安全优先
写入前确认身份（`get_me`）；提交前主动 `secret_scanning`——预防优于事后清理。密钥一旦进入 Git 历史，即使删除提交记录，`git reflog` 和 `git stash` 中仍可能有残留。

### 先搜索再创建
避免重复 Issue/PR——重复内容不仅浪费你的时间，更消耗所有参与者的注意力和审查带宽。一次重复 Issue 可能分散维护者 5-10 分钟——这时间本可以用于审查一个有价值的 PR。

### 最小权限
只在需要时请求所需权限。如果操作被拒绝（403/404），不尝试绕过——检查 Token scope 配置。最小权限原则也适用于 API 调用：分页查询使用合理的 `perPage`（建议 5-10），避免不必要的完整详情读取。

### 操作可验证
每个写操作都有对应的验证步骤，形成闭环：
- 创建 → `search_issues` 确认存在且内容正确
- 合并 → 检查目标分支的 commit log 确认含合并提交
- 更新 → `get_file_contents` 读取确认内容已生效
- 删除 → 读取确认返回 404

### 复合工作流逐步验证
不跳过中间验证直接 sprint 到终点——Bug→PR 流水线中，Issue 创建成功后验证 Issue，PR 创建成功后验证 PR，每一步都是下一步的前提。失败点明确，回滚代价最小。

### 趋势报告深度优先
不堆砌数据——Top 5 必须做五维度深度分析（项目概述/为什么火/核心特性/社区反应/适合人群）；至少 3 条趋势洞察和行动建议；所有数据来源可追溯（标注抓取 URL 和时间戳）。

## 参考文件

- [references/trending-report.md](references/trending-report.md) — Trending 报告模板、数据获取策略、分析框架、输出规格
- [../y-dev/SKILL.md](../y-dev/SKILL.md) — 代码审查与质量门禁（PR 审查、反馈处理、合并前验证）
- [../y-workflow/SKILL.md](../y-workflow/SKILL.md) — 开发工作流（Git 策略、CI/CD、发布流程）
- [../y-test/SKILL.md](../y-test/SKILL.md) — 测试策略（CI 管道、覆盖率标准、质量评分）