# Antigravity Superpowers

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Upstream](https://img.shields.io/badge/upstream-obra%2Fsuperpowers-blue.svg)](https://github.com/obra/superpowers)
[![Aligned with](https://img.shields.io/badge/aligned%20with-v5.1.0-green.svg)](https://github.com/obra/superpowers/releases)
[![Platform](https://img.shields.io/badge/platform-Antigravity-8A2BE2.svg)](https://github.com/Jacky628/superpowers-antigravity)

这个目录包含了从 [Superpowers](https://github.com/obra/superpowers) 移植到 **Antigravity** 环境的核心能力集。

## 什么是 Superpowers？
Superpowers 是一套结构化的软件开发工作流，通过一系列互相关联的 **Skills (技能)** 和 **Workflows (工作流)**，强制 Agent 遵循最佳工程实践（如 TDD、系统化调试和结构化设计）。

## 如何在 Antigravity 中使用？

### 1. 安装与设置

Antigravity 的全局与工作区级自定义位置不同（路径以官方文档为准）：

**全局安装（所有项目可用，推荐）**
- **Skills** → 复制 `skills/*` 到 `~/.gemini/antigravity/skills/`（Agent 按名称自动发现）。
- **Workflows** → 复制 `workflows/*.md` 到 `~/.gemini/antigravity/global_workflows/`（注意是 `global_workflows`，斜杠命令名 = 文件名）。
- **Rules** → 全局规则的唯一位置是 `~/.gemini/GEMINI.md`；把本仓库 `GEMINI.md` 中 `<!-- BEGIN/END superpowers -->` 之间的段落**追加**进去（保留你已有内容）。

**工作区级安装（按项目隔离）**
- 将 `rules/`、`workflows/` 放到项目根目录的 `.agent/`（即 `<workspace>/.agent/rules/`、`<workspace>/.agent/workflows/`）；Skills 仍走上面的全局目录。

**刷新配置**：安装或更新后，前往 Antigravity 右上角 `...` → **Customizations** 点击 **Refresh**；若新文件不显示，完全重启 Antigravity。

### 2. 自动技能加载 (Skills)
Antigravity 会自动检索 `.agent/skills/` 目录。在处理任务时，Agent 会根据需要自动加载并遵循相关技能指令。
- **强制规则**：当你开始开发或调试时，Agent 会根据上下文自动读取并执行对应的 `SKILL.md`。

### 3. 斜杠命令 (Workflows)
你可以直接在聊天框输入以下命令启动标准流程：
- `/brainstorm`：启动需求头脑风暴，产出 Specs。
- `/write-plan`：基于 Specs 编写详细的原子任务清单。
- `/execute-plan`：批量执行任务计划并进行阶段性审查。
- `/git-commit`：按照规范生成中文提交信息。

## 移植说明

- **基线版本**：本能力集对齐上游 Superpowers **v5.1.0**。
- **规则文件适配**：上游引用的 `CLAUDE.md` 在本移植中统一替换为 Antigravity 的规则文件 `superpowers-rule.md`。
- **工具名映射**：skills 仍使用 Claude Code 的工具名；Antigravity（Gemini 系工具）的对应关系见 `skills/using-superpowers/references/antigravity-tools.md`。
- **产物路径**（与 v5 一致）：设计规格写入 `docs/superpowers/specs/`，实施计划写入 `docs/superpowers/plans/`。
- **未移植**：上游 v5 的「可视化头脑风暴」浏览器伴侣（`brainstorming/scripts/` 与 `visual-companion.md`）依赖本地 Node 服务 + 浏览器，未在 Antigravity 环境验证，故本移植保留纯文本/对话式头脑风暴流程。

---
*注：本项目中的能力集基于 Superpowers v5.1.0 移植。*
