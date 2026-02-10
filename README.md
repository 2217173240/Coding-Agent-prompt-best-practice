# Coding Agent Prompt Best Practice

## 简介

本仓库收录了 AI 编码 Agent 的 Prompt 最佳实践，包括：
- **Claude Code** 与 **Gemini CLI** 的协同工作流模板
- **Codex CLI** 自动闭环开发 Prompt 体系
- 基于 CSV 驱动的长时间自动化任务管理
- 人在回路 (Human-in-the-Loop) 的代码审查流程
- 实战总结的使用心法与避坑指南

核心理念：通过结构化 Prompt、外部状态管理（CSV）和分工协作，规避 LLM 的上下文限制与幻觉问题，实现高质量的自动化开发。

## 仓库内容

### 📋 Prompt 模板（可直接复制使用）

#### 1. Claude Code + Gemini CLI 协同工作流
- **Claude Code 执行型 Agent**：
  - 中文：[`documents/claude-cn.md`](documents/claude-cn.md)
  - 英文：[`documents/claude-en.md`](documents/claude-en.md)
- **Gemini CLI 审查/顾问型**：
  - 中文：[`documents/gemini-cn.md`](documents/gemini-cn.md)
  - 英文：[`documents/gemini-en.md`](documents/gemini-en.md)

#### 2. Codex CLI 自动闭环开发
- **驱动 Prompt**：[`codex-auto-prompt/prompt/prompt.md`](codex-auto-prompt/prompt/prompt.md)
- **配套说明文档**：[`codex-auto-prompt/prompt/doc.md`](codex-auto-prompt/prompt/doc.md)

### 💡 实战经验总结
- **Claude Code 使用心法（三条铁律）**：[`documents/cc的最佳用法.md`](documents/cc的最佳用法.md)

## 环境与软件要求

### 环境
*   macOS & Linux 终端 (包括 WSL)

### 软件
*   `claude-code`
*   `gemini-cli`

## 使用方法

### 🔄 方案一：Claude Code + Gemini CLI 协同工作流

适用于需要**人在回路审查**的复杂项目。

**1. 初始化 Claude Code Agent**
- 使用 `/sub-agent` 命令导入模板：[`documents/claude-cn.md`](documents/claude-cn.md) 或 [`documents/claude-en.md`](documents/claude-en.md)
- 或将模板内容设置为 `CLAUDE.md` 的基础内容

**2. 初始化 Gemini CLI Agent**
- 导入模板：[`documents/gemini-cn.md`](documents/gemini-cn.md) 或 [`documents/gemini-en.md`](documents/gemini-en.md)
- Gemini 被约束为"审查/架构顾问"角色：提出审查意见与行动规划，不直接写代码

**3. 协作流程**
- 使用统一的 `@+文件名` 语法对齐两个 CLI 中 Agent 的上下文颗粒度
- 保持任务规划文档的实时更新
- **人工审查并复制粘贴**两个 Agent 之间的通信，确保符合预期，避免 LLM 自主决策导致的短视效应

**4. 分工原理**
- **Gemini 2.5 Pro**：1M token 长上下文 → 负责代码审查和提出解决方案
- **Claude Sonnet 4.5**：200K 上下文 + Agent 训练优化 → 专注于任务执行

---

### 🤖 方案二：Codex CLI 自动闭环开发

适用于需要**长时间挂机自动开发**的场景。

**核心机制**
- 使用 **`issues.csv`** 作为任务驱动引擎和外部记忆
- 每行任务包含：ID、Title、Content、Acceptance Criteria、Review Requirements、State、Labels
- Agent 严格执行 **"读取 → 开发 → 测试 → 提交"** 循环工作流

**使用步骤**
1. **Plan**：与 AI 聊透需求，生成 `plan.md`
2. **Issue**：让 AI 根据 `plan.md` 生成初步的 `issues.csv`，**人工介入修改**，确保验收标准极其具体
3. **Prompt**：使用驱动 Prompt（[`codex-auto-prompt/prompt/prompt.md`](codex-auto-prompt/prompt/prompt.md)）
4. **Run**：挂机，让 Agent 自动循环执行
5. **Check**：查看 Git Commit 记录验证进度

**为什么能跑 11 个小时？**
1. **外部存储解决记忆问题**：`issues.csv` 记录准确进度，即使上下文刷新，任务状态依然可靠
2. **Git Commit 防止幻觉**：代码变更和进度变更绑定，可通过 Git 记录追溯
3. **验收标准强制约束**：每行任务的 Acceptance Criteria 决定开发边界，防止 AI 跑偏

详细说明：[`codex-auto-prompt/prompt/doc.md`](codex-auto-prompt/prompt/doc.md)

---

## 💡 Claude Code 使用心法（三条铁律）

只记住这三个最简单的点，让你 CC 智力直线上升：

**1. 除非在 Plan Mode，永远不要纠正 CC**
- 如果 CC 做错了：`clear` → `git reset` → 修改原始 Prompt 重新执行
- 否则你会收到典中典的"您说的对"

**2. 永远不要使用 `/compact`**
- 就当没有这个命令
- 如果需求执行到 context 满了还没完成 → 拆成两个需求
- `clear` → `git reset` → 只做第一个需求

**3. 三轮对话还没完成就重来**
- Context 用到 60% 就差不多了
- 想清楚你要干什么：`clear` → `git reset` → 重新编辑 Prompt

**一句话总结**：CC 开发团队说过的，错了不要改，直接重新来。没错，绝对是正解。

详细心法：[`documents/cc的最佳用法.md`](documents/cc的最佳用法.md)

---

## 🎯 核心思想

**1. 规避 LLM 的局限性**

由于 LLM 底层数学规律的缺陷可能导致错误复合效应和上下文窗口限制，本工作流避免使用单一模型来完成完整的"代码审查-代码修改"循环。引入 Gemini 的长上下文能力，以尽量减少因上下文窗口限制导致的信息丢失，并降低因平方成本诅咒（quadratic cost curse）而产生的高昂 API 使用费用。

**2. 发挥各自优势**

`claude-code` 环境及其完整的工具生态系统，是与 **Claude 4** 模型系列的训练紧密结合的。这使得 **Claude 4** 系列成为目前在"vibe coding"领域最强大的 Agent。结合实际使用 Cursor、Augment 等软件的体验，目前提出的这套方案是实践中门槛最低，并且能够切实完成代码重构等复杂任务的最优解。

## 联系与反馈

如果您有任何建议或疑问，欢迎在本仓库中提出 issue，或联系 `2217173240@qq.com`（请勿发送 QQ 好友请求）。


