# Antigravity Superpowers

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Upstream](https://img.shields.io/badge/upstream-obra%2Fsuperpowers-blue.svg)](https://github.com/obra/superpowers)
[![Aligned with](https://img.shields.io/badge/aligned%20with-v7.0.0-green.svg)](https://github.com/obra/superpowers/releases/tag/v7.0.0)
[![Platform](https://img.shields.io/badge/platform-Antigravity-8A2BE2.svg)](https://github.com/Jacky628/superpowers-antigravity)

这个目录包含了从 [Superpowers](https://github.com/obra/superpowers) 移植并适配到 **Antigravity** 环境的核心能力集（共 **15 个 Skills**、**4 个 Workflows** 与全局/工作区 **Rules**）。

## 什么是 Superpowers？
Superpowers 是一套结构化的软件开发工作流，通过一系列互相关联的 **Skills (技能)** 和 **Workflows (工作流)**，强制 Agent 遵循最佳工程实践（如结构化头脑风暴、契约式计划、红-绿-重构 TDD、子代理驱动开发、系统化调试与会话诊断）。

## 如何在 Antigravity 中使用？

### 1. 安装与设置

支持 **插件一键安装**、**全局目录安装** 与 **工作区级安装** 三种方式：

**方式 A：Antigravity CLI 插件安装（v7.0.0+ 原生支持）**
```bash
agy plugin install https://github.com/Jacky628/superpowers-antigravity
# 或安装上游官方源：agy plugin install https://github.com/obra/superpowers
```

**方式 B：全局目录安装（所有项目可用，含中文 Rules 与斜杠命令 Workflows，推荐）**
- **Skills** → 复制 `skills/*` 到 `~/.gemini/antigravity/skills/`（同时建议同步至 `~/.gemini/config/skills/` 以兼容 Antigravity 2.0 / CLI 全局发现）。
  > ⚠️ 注意：请复制 `skills/` 下的**各个子目录**，不要把仓库根目录直接拷入导致出现 `skills/skills/` 双层嵌套。
- **Workflows** → 复制 `workflows/*.md` 到 `~/.gemini/antigravity/global_workflows/`（注意是 `global_workflows`，斜杠命令名 = 文件名）。
- **Rules** → 全局规则位置是 `~/.gemini/GEMINI.md`；把本仓库 `GEMINI.md` 中 `<!-- BEGIN/END superpowers -->` 之间的段落**追加**进去（保留你已有内容）。

```bash
# 一键全局安装命令（在仓库根目录执行）
mkdir -p ~/.gemini/antigravity/skills ~/.gemini/config/skills ~/.gemini/antigravity/global_workflows
cp -r skills/* ~/.gemini/antigravity/skills/
cp -r skills/* ~/.gemini/config/skills/
cp -f workflows/*.md ~/.gemini/antigravity/global_workflows/
```

**方式 C：工作区级安装（按项目隔离，优先级高于全局）**
- 将 `skills/*`、`rules/*`、`workflows/*` 放到项目根目录的 `.agent/`（或 `.agents/`）对应子目录中（即 `<workspace>/.agent/skills/`、`<workspace>/.agent/rules/`、`<workspace>/.agent/workflows/`）。

**刷新配置**：安装或更新后，前往 Antigravity 右上角 `...` → **Customizations** 点击 **Refresh**；若新文件不显示，完全重启 Antigravity。

### 2. 自动技能加载 (Skills)
Antigravity 会按优先级自动检索工作区 `.agent/skills/`（或 `.agents/skills/`）及全局 `~/.gemini/config/skills/`、`~/.gemini/antigravity/skills/` 目录。在处理任务时，Agent 会根据需要自动加载并遵循相关技能指令。
- **强制规则**：当你开始开发或调试时，Agent 会根据上下文自动读取并执行对应的 `SKILL.md`。

### 3. 斜杠命令 (Workflows)
你可以直接在聊天框输入以下命令启动标准流程：
- `/brainstorm`：启动需求头脑风暴，澄清意图、动态分级并经 `builder-check` 审查产出 Specs。
- `/write-plan`：基于 Specs 编写含全局约束、接口契约与审查焦点的实施计划。
- `/execute-plan`：在当前会话内原生连续执行计划任务，并在终局进行全分支审查。
- `/git-commit`：按照规范生成中文提交信息。

## 移植与对齐说明（v7.0.0）

- **基线版本**：本能力集对齐上游 Superpowers [**v7.0.0**](https://github.com/obra/superpowers/releases/tag/v7.0.0)（2026-10-09 发布）。
- **核心升级亮点（v5.1.0 → v7.0.0）**：
  - **新增 `diagnosing-superpowers` 技能**：支持基于会话日志（Transcript）诊断技能未触发、重复劳动、偏离计划或 Token 异常消耗问题。
  - **重写 `brainstorming`**：先复述意图并按体量分级（微任务/小改动/项目级文档），使用 `builder-check-prompt.md` 替代旧 Spec Reviewer，并包含安全加固版 `visual-companion`（Per-session Key 鉴权）。
  - **重构 `subagent-driven-development`**：合并为单 `task-reviewer-prompt.md`（一次审查同时输出 Spec 与质量双结论）+ `re-review-prompt.md`，并通过 `.superpowers/sdd/<plan>/` 下的 `sdd-workspace`、`task-brief`、`review-package` 脚本进行文件化交接与账本追踪。
  - **重构 `writing-plans` 与 `executing-plans`**：计划只记决策与接口契约（不抄全量代码）；`executing-plans` 升级为带 `task-start` / `task-done` 账本的原生连续执行模式。
  - **重构 `test-driven-development`**：以正面清单 `writing-good-tests.md`（可证伪性与突变检验）替代旧 `testing-anti-patterns.md`，并要求跑全量测试套件确认 Green。
- **工具名映射**：见 [`skills/using-superpowers/references/antigravity-tools.md`](./skills/using-superpowers/references/antigravity-tools.md)，完整映射了 Antigravity 原生工具（`view_file`、`write_to_file`、`replace_file_content`、`run_command`、`invoke_subagent` 与 Task Artifact）。
- **产物路径**：设计规格写入 `docs/superpowers/specs/`，实施计划写入 `docs/superpowers/plans/`，执行账本位于 `.superpowers/sdd/<plan>/`。

---
*注：本项目中的能力集基于 Superpowers v7.0.0 移植与增强。*
