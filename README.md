# Coding Agent Prompt Best Practice

## 仓库定位

这个仓库不是单一 Prompt 集合，而是一个面向 AI 编码 Agent 的资料库，当前内容分成四类：

- 可直接复制使用的执行型 / 顾问型 Prompt
- 面向 Codex CLI 的 CSV 自动闭环开发 Prompt
- 可本地使用的 Codex skill 目录
- 跨语言重构、门禁设计、人机协作的可复用知识库

仓库当前没有业务代码或可执行服务，主体是 Markdown 文档与 skill 元数据。

## 目录总览

```text
.
├── LICENSE
├── README.md
├── documents/
│   ├── claude-cn.md
│   ├── claude-en.md
│   ├── gemini-cn.md
│   ├── gemini-en.md
│   ├── cc的最佳用法.md
│   └── cross_language_refactor_reusable_kb.md
├── codex-auto-prompt/
│   ├── prompt/
│   │   ├── doc.md
│   │   └── prompt.md
│   └── seed-Agentsmd/
│       └── AGENTS.md
├── harness-engineering/
│   ├── SKILL.md
│   ├── agents/openai.yaml
│   └── references/harness-engineering-digest.md
└── cybernetic-systems-engineering/
    ├── SKILL.md
    ├── agents/openai.yaml
    ├── assets/quickstart.md
    └── references/
        ├── README.md
        └── gda-framework.md
```

## 文件导航

### 根目录

| 文件 | 实际内容 |
| --- | --- |
| `LICENSE` | MIT License。 |
| `README.md` | 当前导航文档，按目录与文件职责介绍仓库。 |

### `documents/`

| 文件 | 实际内容 |
| --- | --- |
| `documents/claude-cn.md` | Claude Code 中文执行协议 V2.0，包含身份、最高准则、工具、SOP、开发规范、Git 规范、思维模型。 |
| `documents/claude-en.md` | `claude-cn.md` 的英文版。 |
| `documents/gemini-cn.md` | Gemini CLI 中文顾问型 Prompt，强调深度审查、规则保真、系统性反馈、最小元任务拆解，并明确禁止直接写代码。 |
| `documents/gemini-en.md` | `gemini-cn.md` 的英文版。 |
| `documents/cc的最佳用法.md` | Claude Code 的三条实战心法：非 Plan Mode 不纠正、不要用 `/compact`、三轮未完成就重开。 |
| `documents/cross_language_refactor_reusable_kb.md` | 大型跨语言重构知识库，覆盖 Harness × CSV Loop 融合流程、阶段验收模板、最小证据集、文档职责分层、等价验证、CI 门禁、人审 + AI 协作、规则清单与附录模板。 |

### `codex-auto-prompt/prompt/`

| 文件 | 实际内容 |
| --- | --- |
| `codex-auto-prompt/prompt/prompt.md` | 给 Codex CLI 的驱动 Prompt，要求围绕 `issues.csv` 执行“读取-开发-测试-提交”循环，并包含 `[Code Review]` 任务的专门分支。 |
| `codex-auto-prompt/prompt/doc.md` | 对上面驱动 Prompt 的说明文档，解释 `issues.csv` 结构、长时间挂机的原理、使用步骤与避坑点。 |

### `codex-auto-prompt/seed-Agentsmd/`

| 文件 | 实际内容 |
| --- | --- |
| `codex-auto-prompt/seed-Agentsmd/AGENTS.md` | 从既有实战经验中提炼出的通用 `AGENTS.md` 模板。保留执行规范、任务工作流、测试与质量红线、防踩坑约束，但去掉了具体项目路径、文件名、项目名、账号、数据库 IP 等项目绑定信息，方便他人按自己的代码库继续扩展。 |

### `harness-engineering/`

| 文件 | 实际内容 |
| --- | --- |
| `harness-engineering/SKILL.md` | Harness-first 执行 skill，强调先定义成功边界、逐层放权、自主探索预算、主动求助阈值、选项型提问和可追溯交付。 |
| `harness-engineering/references/harness-engineering-digest.md` | 对 OpenAI《Harnesses are underrated》的中文蒸馏，整理了 harness 设计原则、案例信号、求助阈值与求助模板。 |
| `harness-engineering/agents/openai.yaml` | 该 skill 的展示名、简介和默认提示配置。 |

### `cybernetic-systems-engineering/`

| 文件 | 实际内容 |
| --- | --- |
| `cybernetic-systems-engineering/SKILL.md` | 新增 skill。用系统工程 + 工程控制论 + GDA 方法处理 bugfix、feature、refactor、性能、迁移、事故复盘、测试设计、架构审计与 gate 设计，核心内容包括 Control Contract、GDA 四步骨架、传感器工程、决策准则、反模式、交付格式和实战剧本。 |
| `cybernetic-systems-engineering/references/gda-framework.md` | CSE skill 的展开理论文档，解释五维方法论、GDA 四步法、架构启示、常见误区，并给出一页版执行模板。 |
| `cybernetic-systems-engineering/references/README.md` | CSE 引用索引，说明 `SKILL.md` 与 `gda-framework.md` 的阅读顺序。 |
| `cybernetic-systems-engineering/assets/quickstart.md` | CSE 快速入门，给出触发场景、最短使用姿势、最小输出模板和典型问题示例。 |
| `cybernetic-systems-engineering/agents/openai.yaml` | CSE skill 的展示名与简短说明。 |

## 怎么选用

- 想要一个“直接执行代码”的主 Agent Prompt：看 `documents/claude-cn.md` 或 `documents/claude-en.md`。
- 想要一个“只做审查和规划”的顾问 Prompt：看 `documents/gemini-cn.md` 或 `documents/gemini-en.md`。
- 想让 Codex CLI 按台账自动循环开发：先看 `codex-auto-prompt/prompt/prompt.md`，再看 `codex-auto-prompt/prompt/doc.md`。
- 想快速拿一个可复用的 `AGENTS.md` 基线模板：看 `codex-auto-prompt/seed-Agentsmd/AGENTS.md`。
- 想把复杂任务放入可观测、可协作的执行框架：看 `harness-engineering/SKILL.md`。
- 想用更强的系统级分析方法处理复杂工程任务：看 `cybernetic-systems-engineering/SKILL.md`。
- 想沉淀跨语言迁移、门禁和人机协作方法：看 `documents/cross_language_refactor_reusable_kb.md`。

## 推荐阅读顺序

### 只想快速拿来用

1. Claude Code 执行 Prompt：`documents/claude-cn.md`
2. Gemini 审查 Prompt：`documents/gemini-cn.md`
3. Codex CSV 自动化：`codex-auto-prompt/prompt/prompt.md`

### 想把仓库当作本地 skill 库

1. `harness-engineering/SKILL.md`
2. `cybernetic-systems-engineering/assets/quickstart.md`
3. `cybernetic-systems-engineering/SKILL.md`
4. 需要理论展开时再读 `cybernetic-systems-engineering/references/gda-framework.md`

### 想系统看跨语言重构方法论

1. `documents/cross_language_refactor_reusable_kb.md`
2. `harness-engineering/SKILL.md`
3. `cybernetic-systems-engineering/SKILL.md`

## 当前仓库最适合的场景

- Claude Code 与 Gemini CLI 分工协作
- Codex CLI 基于 `issues.csv` 的长时自动开发
- 需要 harness-first 执行框架的复杂排障或 CI 修复
- 需要把 bugfix / refactor / gate 设计放进系统级闭环控制的任务
- 需要为跨语言重构项目建立证据链、验收模板和协作协议
