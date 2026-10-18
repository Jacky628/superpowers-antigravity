# Superpowers 技能库详解

本文档详细介绍 `skills/` 目录下所有 15 个技能的定义、使用方法和适用场景。

> 对齐上游 Superpowers **v7.0.0**。规则文件支持 `GEMINI.md` / `AGENTS.md` / `superpowers-rule.md`；产物路径为 `docs/superpowers/specs/`、`docs/superpowers/plans/` 与 `.superpowers/sdd/<plan>/`。

---

## 目录

1. [brainstorming - 设计讨论](#1-brainstorming---设计讨论)
2. [writing-plans - 编写计划](#2-writing-plans---编写计划)
3. [executing-plans - 执行计划（原生内联执行）](#3-executing-plans---执行计划原生内联执行)
4. [subagent-driven-development - 子代理驱动开发](#4-subagent-driven-development---子代理驱动开发)
5. [test-driven-development - 测试驱动开发](#5-test-driven-development---测试驱动开发)
6. [systematic-debugging - 系统化调试](#6-systematic-debugging---系统化调试)
7. [verification-before-completion - 完成前验证](#7-verification-before-completion---完成前验证)
8. [using-git-worktrees - 使用 Git Worktrees](#8-using-git-worktrees---使用-git-worktrees)
9. [finishing-a-development-branch - 完成开发分支](#9-finishing-a-development-branch---完成开发分支)
10. [dispatching-parallel-agents - 并行调度子代理](#10-dispatching-parallel-agents---并行调度子代理)
11. [requesting-code-review - 请求代码审查](#11-requesting-code-review---请求代码审查)
12. [receiving-code-review - 接收代码审查](#12-receiving-code-review---接收代码审查)
13. [using-superpowers - 使用技能系统](#13-using-superpowers---使用技能系统)
14. [writing-skills - 编写技能](#14-writing-skills---编写技能)
15. [diagnosing-superpowers - 会话诊断](#15-diagnosing-superpowers---会话诊断)

---

## 1. brainstorming - 设计讨论 (v7.0 重构)

### 核心定义
在动手构建任何东西之前，通过自然、单问题推进的对话，弄清楚用户**真正想要什么、以及为什么**，并将其转化为清晰的文字或设计文档。

### 适用场景
- **必须使用**：任何创意性工作之前，包括创建功能、构建组件、添加功能、修改行为，乃至非软件项目规划。

### 硬性门禁 (HARD-GATE)
在用户批准完整描述（小改动在对话中确认，项目级需求在设计文档中确认）之前，**不得**构建任何内容（不得写产品代码、脚手架、安装依赖、创建项目或调用实现技能）。
- **两项例外**：1) 用户明确选择跳过提问直接开始；2) 用户同意进行一次性的投石问路（Spike，标记为 throwaway，若要保留仍需回到门禁走流程）。
- **项目设计文档获批后**：下一步唯一可调用的技能是 `writing-plans`。

### v7.0 核心流程
1. **首条消息（Your First Message）**：一句话告知用户若想跳过提问可直接说；紧跟一个引导用户描述具体场景的开放式问题（不附选项菜单）。
2. **对话与技术栈归属（How It Gets Made）**：尽早明确平台、语言、核心库等决策哪些由用户把控、哪些交由 Agent 决定（Agent 决定的部分需显式标明为 Agent 的选择）。
3. **分段复述（Play It Back）**：每理解一部分（约 200-300 字）就向用户复述一次，标明猜测项，并询问是否有误或遗漏。
4. **按需提供可视化伴侣（Visual Companion）**：当涉及界面布局、流程对比或结构图，"看比读更直观"时，即时提议启动带 Session Key 鉴权的浏览器可视化伴侣（`visual-companion.md`）。
5. **动态体量分级（Size the Work）**：
   - **清晰微任务**：直接完成并汇报。
   - **小改动**：在对话中确认描述后直接按标准工作流实现。
   - **项目级需求**：写入 `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md` 并提交。
6. **实现者视角审查（Builder Check）**：派发全新子代理执行 `builder-check-prompt.md`，将问题分为"已回答 / 细枝末节（Agent 自行裁决并标注）/ 必须由用户决定（合并为单轮提问）"，更新文档后交由用户批准。

---

## 2. writing-plans - 编写计划 (v6.4 重构)

### 核心定义
将已确认的设计规格转化为聚焦**决策、接口契约与测试断言**的实施计划（计划记录决策，而非照抄全量代码）。

### 适用场景
- 当多步骤任务已有规格或明确需求，**在动手写代码之前**使用。

### 核心设计（v6.4+）
- **Global Constraints（全局约束块）**：原样摘录绑定所有任务的规则（版本下限、依赖限制、命名与精确常量），确保下游实现者与审查者可见。
- **Interfaces（任务级接口契约块）**：明确每个任务消费和产出的精确函数签名、类型与模块边界。
- **Review Focus（审查焦点）**：列出最多 5 个规格隐含但易被漏测的边界输入或故障模式，并绑定到对应任务的测试中。
- **What a Step Contains（步骤内容准则）**：测试步骤给出测试名与断言；代码步骤给出文件路径、精确签名与规格值（仅在签名无法决定的复杂算法处才写代码体）；验证步骤给出命令与预期通过输出。
- **比例自检（Self-Review）**：计划长度不应是规格文档的数倍；若大段代码块占主导，将其精简为签名与测试断言。

### 输出路径与用户审阅门禁
- 保存至 `docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md`。
- **必须先请用户审阅保存后的计划文件**，获批后再推荐两种执行模式之一（`subagent-driven-development` 或 `executing-plans`）并说明各自的成本与推荐理由。

---

## 3. executing-plans - 执行计划（原生内联执行，v6.4 重构）

### 核心定义
在当前会话内，按照与子代理驱动开发（SDD）相同的工作区账本、TDD 纪律和停止规则，由主会话亲自连续实现所有任务，并在最后派发一次全分支代码审查。

### 适用场景
- 希望以更低的 Token 成本执行实施计划（无需为每个任务单独派发实现与审查子代理）。

### 工作流程
1. **计划预检（Pre-Flight）**：扫描计划内部冲突或潜在缺陷，记录裁决并初始化 `.superpowers/sdd/<plan>/` 工作区账本。
2. **逐任务内联执行**：
   - 运行 `bash scripts/task-start` 登记任务开始。
   - 严格按 TDD（先红后绿）完成实现与验证。
   - 运行 `bash scripts/task-done` 登记测试命令与通过证据。
3. **连续执行不中断**：不在任务之间停下来询问"是否继续"，仅在遇到破坏性或不可逆阻塞时才请求人类介入。
4. **终局全分支审查**：所有任务完成后，在最强模型上派发一次全分支审查子代理，修复发现的问题后转入 `finishing-a-development-branch`。

---

## 4. subagent-driven-development - 子代理驱动开发 (v6.0~v7.0 重构)

### 核心定义
为每个任务（或同类微任务批次）派遣上下文隔离的实现子代理，随后派遣单次只读审查子代理同时完成**规格合规性**与**代码质量**双项裁决，并通过 `.superpowers/sdd/<plan>/` 文件化交接与进度账本实现高可靠、低成本的自治执行。

### 与 `executing-plans` 的区别
| 特点 | subagent-driven-development | executing-plans (Native) |
| :--- | :--- | :--- |
| 执行主体 | 每任务派遣独立实现子代理（零上下文污染） | 当前会话亲自连续实现所有任务 |
| 审查机制 | 每任务单次 `task-reviewer`（双结论）+ 终局全分支审查 | 终局派发一次全分支审查 |
| 交接方式 | `sdd-workspace` / `task-brief` / `review-package` 文件化交接 | 共享 `.superpowers/sdd/<plan>/` 账本 |
| Token 成本 | 较高（适合复杂、多任务大计划） | 最低（适合中小型计划或预算敏感场景） |

### 核心机制（v6.0~v7.0）
1. **计划级隔离工作区（Plan-Scoped Workspace）**：通过 `scripts/sdd-workspace` 在 `.superpowers/sdd/<plan-basename>/` 下维护进度账本、任务简报（`task-brief`）和审查包（`review-package`），避免粘贴长 diff 污染主上下文。
2. **单 Reviewer 双结论（`task-reviewer-prompt.md`）**：每个任务只需派发 1 个只读审查子代理，读取 diff 文件后同时输出 Spec 合规性裁决与代码质量裁决（含 `⚠️` 无法仅凭 diff 验证的条目交由 Controller 核查）。
3. **唤醒修复与增量复审（`re-review-prompt.md`）**：若审查发现问题，唤醒原实现子代理修复并重跑相关测试，随后使用 `re-review-prompt.md` 仅针对修复点进行增量复审（设 5 轮熔断器）。
4. **禁止预判与诱导**：严禁 Controller 在派发审查时暗示 Reviewer 忽略某问题或预先降低严重级别。
5. **终局全分支审查**：全部任务通过后，派发一次全分支审查并清理临时 `.superpowers/sdd/<plan>/` 目录，随后转入 `finishing-a-development-branch`。

---

## 5. test-driven-development - 测试驱动开发 (v6.4 更新)

### 核心定义
**先写测试，看它失败，再写最小代码使其通过。**

### 铁律
```
没有失败的测试就没有生产代码。
```
先写了代码？**删除它，重新开始。**

### RED-GREEN-REFACTOR 循环与 v6.4 增强
| 阶段 | 行动 | 验证要求 |
| :--- | :--- | :--- |
| **RED** | 写一个失败的测试 | 运行测试，确认因预期功能缺失而失败（并在报告中记录 RED 证据） |
| **GREEN** | 写最小代码使测试通过 | 运行**整个项目的测试套件**（不仅是当前测试文件），确认全绿且无相邻回归 |
| **REFACTOR** | 重构代码，保持测试绿色 | 再次运行全量测试套件确认仍通过 |

### 编写好测试的六条准则（`writing-good-tests.md`）
- 替代了旧版 `testing-anti-patterns.md`，强调**可证伪性（Falsifiability）**：
  1. 能明确指出哪处生产代码改动会让该测试失败。
  2. 测试期望值必须独立于被测代码推导，拒绝"变更检测器陷阱"（Change-Detector Trap）。
  3. 严禁对脚本、技能或 Prompt 做伪造可证伪性的纯字符串存在性测试（String-Presence Trap，应测试真实行为而非 grep 文本）。
  4. 闭合突变检验（Mutation Check）：验证完 Green 后思考若破坏核心逻辑测试是否真的会挂。

---

## 6. systematic-debugging - 系统化调试

### 核心定义
**在尝试任何修复之前，必须先找到根本原因。**

### 铁律
```
没有根因调查就没有修复方案。
```

### 适用场景
- 任何技术问题：测试失败、生产 Bug、意外行为、性能问题、构建失败、集成问题。
- **尤其**在时间紧迫时（紧急情况最容易诱发瞎猜）。

### 四阶段流程

| 阶段              | 关键活动                                           | 成功标准               |
| ----------------- | -------------------------------------------------- | ---------------------- |
| **1. 根因调查**   | 仔细阅读错误信息、稳定复现、检查最近变更、收集证据 | 理解问题的 What 和 Why |
| **2. 模式分析**   | 找到相似的正常代码、对比差异、理解依赖             | 识别差异点             |
| **3. 假设与测试** | 提出单一假设、做最小变更验证                       | 假设得到验证或否定     |
| **4. 实施**       | 创建失败测试用例、实施单一修复、验证               | Bug 解决，测试通过     |

### 红旗思维（立即停止）
- "先快速修一下，之后再调查"
- "试试改 X 看看有没有用"
- "已经尝试了多个修复但都没用"（如果 ≥3 次修复失败，应质疑架构）

### 核心原则
**症状修复是失败。必须找到根本原因。**

---

## 7. verification-before-completion - 完成前验证

### 核心定义
**声称完成之前，必须有新鲜的验证证据。**

### 铁律
```
没有刚执行的验证输出就没有完成声明。
```

### 适用场景
- 在声称任何成功/完成/通过之前。
- 在提交、创建 PR、标记任务完成之前。
- 在移动到下一个任务之前。

### 验证门函数
```
在声称任何状态之前：
1. 识别：什么命令能证明这个声称？
2. 执行：运行完整命令（新鲜的，完整的）
3. 阅读：完整输出、检查退出码、计数失败数
4. 验证：输出是否确认声称？
   - 否：陈述实际状态及证据
   - 是：陈述声称并附上证据
5. 然后才能发表声称
```

### 禁止用语
- "应该可以了"
- "应该通过了"
- "看起来没问题"
- "完美！" / "搞定！"（在验证之前）

### 核心原则
**证据优先。运行命令、阅读输出、然后声称结果。不可商量。**

---

## 8. using-git-worktrees - 使用隔离工作区

### 核心定义
确保工作在隔离的工作区中进行。**优先使用平台原生的 worktree 工具，仅在没有原生工具时才回退到手动 git worktree。**（v5 重写）

### 适用场景
- 开始需要与当前工作区隔离的功能开发。
- 执行实施计划之前。

### Step 0：先检测是否已隔离
创建任何东西之前，先判断当前是否已在隔离工作区：
```bash
GIT_DIR=$(cd "$(git rev-parse --git-dir)" 2>/dev/null && pwd -P)
GIT_COMMON=$(cd "$(git rev-parse --git-common-dir)" 2>/dev/null && pwd -P)
```
- **`GIT_DIR != GIT_COMMON`（且非 submodule）**：已在 linked worktree 中，直接跳到项目设置，**不要**再建 worktree。（submodule 守卫：用 `git rev-parse --show-superproject-working-tree` 排除 submodule 误判。）
- **`GIT_DIR == GIT_COMMON`**：普通检出。若用户未声明偏好，先征得同意再创建 worktree。

### Step 1：创建隔离工作区（两种机制，按序尝试）
- **1a. 原生 worktree 工具（优先）**：若有形如 `EnterWorktree`、`/worktree` 命令或 `--worktree` 标志的能力，直接用它——原生工具会自动处理目录、分支与清理。用 `git worktree add` 会制造 harness 看不见的幽灵状态。
- **1b. git worktree 回退（仅当无原生工具）**：手动创建。目录选择优先级：用户声明偏好 > 现有 `.worktrees`/`worktrees` > 现有全局目录 > 默认项目根 `.worktrees/`。

### 安全验证（仅项目本地目录）
**创建前必须确认目录已被忽略**：
```bash
git check-ignore -q .worktrees 2>/dev/null || git check-ignore -q worktrees 2>/dev/null
```
未被忽略则加入 `.gitignore` 并提交。全局目录（`~/.config/superpowers/worktrees/`）无需验证。

### 核心原则
**先检测已有隔离，再用原生工具，最后才回退 git。永远不要与 harness 对抗。**

---

## 9. finishing-a-development-branch - 完成开发分支

### 核心定义
验证测试 → 检测环境 → 呈现选项 → 执行用户选择 → 清理工作区。

### 适用场景
- 实施完成、所有测试通过。
- 需要决定如何集成工作成果。

### 工作流程
1.  **验证测试**：测试失败则停止，先修复，不得继续。
2.  **检测环境**（决定显示哪种菜单与如何清理）：
    ```bash
    GIT_DIR=$(cd "$(git rev-parse --git-dir)" 2>/dev/null && pwd -P)
    GIT_COMMON=$(cd "$(git rev-parse --git-common-dir)" 2>/dev/null && pwd -P)
    ```
    | 状态 | 菜单 | 清理 |
    | ---- | ---- | ---- |
    | `GIT_DIR == GIT_COMMON`（普通仓库） | 标准 4 选项 | 无 worktree 可清理 |
    | `!=`，具名分支 worktree | 标准 4 选项 | 基于来源（provenance）清理 |
    | `!=`，detached HEAD | 精简 3 选项（无合并） | 不清理（外部托管） |
3.  **确定基础分支**（main/master，或询问用户）。
4.  **呈现选项**：
    - 标准 4 选项：① 本地合并 ② 推送并创建 PR ③ 保持现状 ④ 丢弃工作（需输入 "discard" 确认）。
    - detached HEAD 精简为 3 选项（无本地合并）。
5.  **执行用户选择**，并据环境进行相应清理。

### 核心原则
**测试失败不得继续。先检测工作区环境再决定菜单与清理方式。丢弃工作需明确确认。**

---

## 10. dispatching-parallel-agents - 并行调度子代理

### 核心定义
当面对多个独立问题（不同测试文件、不同子系统），并行派遣多个子代理同时处理。

### 适用场景
- 3+ 个测试文件因不同根因失败
- 多个子系统独立损坏
- 各问题可独立理解，无共享状态

### 不适用场景
- 失败相互关联（修复一个可能修复其他）
- 需要理解完整系统状态
- 代理间会相互干扰

### 工作流程
1.  **识别独立领域**：按问题源分组（工具批准流、批处理行为、中止功能等）。
2.  **创建聚焦任务**：每个代理获得明确范围、清晰目标、约束条件、期望输出。
3.  **并行派遣**。
4.  **审查与整合**：阅读摘要、验证无冲突、运行完整测试套件。

### Agent Prompt 结构示例
```markdown
修复 src/agents/agent-tool-abort.test.ts 的 3 个失败测试：
1. "should abort tool with partial output capture" - 期望消息包含 'interrupted at'
2. ...

这些是时序/竞态条件问题。你的任务：
1. 阅读测试文件，理解每个测试验证什么
2. 识别根因 - 时序问题还是实际 Bug？
3. 修复...
不要只增加超时时间 - 找到真正问题。

返回：发现了什么、修复了什么的摘要。
```

### 核心原则
**每个独立问题域一个代理。让它们并行工作。**

---

## 11. requesting-code-review - 请求代码审查

### 核心定义
派遣代码审查子代理，在问题级联之前捕获。

### 适用场景
- **必须**：子代理驱动开发中每个任务之后、完成主要功能后、合并到 main 之前。
- **可选**：卡住时（新视角）、重构前（基线检查）、修复复杂 Bug 后。

### 工作流程
1.  **获取 Git SHA**：
    ```bash
    BASE_SHA=$(git rev-parse HEAD~1)
    HEAD_SHA=$(git rev-parse HEAD)
    ```
2.  **派遣 `superpowers:code-reviewer` 子代理**：填充 `requesting-code-review/code-reviewer.md` 模板的占位符（v5 已将审查者统一整合为单一 code-reviewer 模板）。
3.  **处理反馈**：
    - Critical → 立即修复
    - Important → 继续前修复
    - Minor → 稍后处理

### 核心原则
**早审查，勤审查。**

---

## 12. receiving-code-review - 接收代码审查

### 核心定义
代码审查需要技术评估，而非情感表演。

### 适用场景
- 收到代码审查反馈时。
- 在实施建议之前。
- 反馈不清晰或技术上可疑时。

### 禁止回应
```
❌ "你说得太对了！"
❌ "好观点！"
❌ "让我马上实现"（未验证前）
```

### 正确回应模式
```
1. 阅读：完整反馈，不做反应
2. 理解：用自己的话复述需求（或询问）
3. 验证：对照代码库现实检查
4. 评估：对这个代码库技术上是否合理？
5. 回应：技术确认或有理有据的反驳
6. 实施：一次一项，逐个测试
```

### 何时反驳
- 建议破坏现有功能
- 审查者缺乏完整上下文
- 违反 YAGNI（未使用的功能）
- 对当前技术栈技术上不正确
- 与架构决策冲突

### 核心原则
**验证后再实施。提问后再假设。技术正确优于社交舒适。**

---

## 13. using-superpowers - 使用技能系统

### 核心定义
开始任何对话时使用，确立如何查找和使用技能。

### 铁律
```
如果你认为有 1% 的可能某个技能适用，你必须调用它。
这不是建议，这是强制。
```

### 指令优先级 (v5)
技能会覆盖默认系统提示行为，但**用户指令永远优先**：
1.  用户显式指令（`superpowers-rule.md`、其他 `.agent/rules/` 文件、直接请求）——最高
2.  Superpowers 技能——覆盖默认系统行为
3.  默认系统提示——最低

若 `superpowers-rule.md` 说"不要用 TDD"而技能说"始终 TDD"，听用户的。

### 平台适配 (Antigravity)
技能文档使用 Claude Code 的工具名；Antigravity 基于 Gemini 系工具，遇到工具名时按 `using-superpowers/references/antigravity-tools.md` 映射（如 `Read`→`view_file`、`Write`→`write_to_file`、`Edit`→`replace_file_content`、`Bash`→`run_command`、`Task`→`invoke_subagent`、`Skill`→通过 `view_file` 读取对应 `SKILL.md`）。

### 子代理豁免
若你本身是被派遣来执行特定任务的子代理（SUBAGENT-STOP），跳过本技能。

### 使用流程
1.  **用户消息到达**
2.  **判断**：是否有任何技能可能适用？
3.  **即使只有 1% 可能性**：调用 Skill 工具
4.  **宣布**："我正在使用 [技能名] 来 [目的]"
5.  **如有检查清单**：为每项创建 TodoWrite
6.  **精确遵循技能指令**

### 红旗思维（停止并检查技能）
| 想法                     | 现实                         |
| ------------------------ | ---------------------------- |
| "这只是个简单问题"       | 问题也是任务。检查技能。     |
| "我需要先了解更多上下文" | 技能检查在询问之前。         |
| "让我先探索代码库"       | 技能告诉你如何探索。先检查。 |
| "这不需要正式技能"       | 如果技能存在，就使用它。     |
| "我记得这个技能"         | 技能会演进。阅读当前版本。   |

### 技能类型
- **刚性**（TDD、调试）：精确遵循，不要放弃纪律。
- **灵活**（模式）：将原则适应上下文。

---

## 14. writing-skills - 编写技能

### 核心定义
**编写技能就是将测试驱动开发应用于流程文档。**

### 适用场景
- 创建新技能
- 编辑现有技能
- 部署前验证技能有效

### TDD 映射

| TDD 概念             | 技能创建对应               |
| -------------------- | -------------------------- |
| **测试用例**         | 压力场景（子代理）         |
| **生产代码**         | 技能文档（SKILL.md）       |
| **测试失败 (RED)**   | 没有技能时代理违规（基线） |
| **测试通过 (GREEN)** | 有技能时代理遵守           |
| **重构**             | 堵住漏洞，保持遵守         |

### 何时创建技能
- 技术对你来说不是直觉显而易见的
- 你会在不同项目中再次引用
- 模式广泛适用（非项目特定）

### 不创建技能
- 一次性解决方案
- 标准实践（其他地方有良好文档）
- 项目特定约定（放入 CLAUDE.md）

### SKILL.md 结构
```markdown
---
name: skill-name-with-hyphens
description: Use when [触发条件和症状]
---

# Skill Name

## Overview
这是什么？1-2 句核心原则。

## When to Use
(如决策不明显则用小流程图)
症状和用例列表
何时不使用

## Core Pattern
代码前后对比

## Quick Reference
扫描常见操作的表格或列表

## Common Mistakes
问题 + 修复

## Real-World Impact (可选)
具体结果
```

### 核心原则
**没有失败的测试就没有技能。与代码的 TDD 原则完全相同。**

---

## 15. diagnosing-superpowers - 会话诊断 (v6.4 新增)

### 核心定义
当一次会话出现异常（如重复劳动、忽略了计划、某个技能应当触发却未触发、Token 消耗远超预期）时，通过读取磁盘上的会话日志（Transcript）定位根因，并输出每一条结论均带有 `path:line` 证据链的诊断报告。

### 适用场景
- 用户要求"弄清楚这次（或某次历史）会话里 Superpowers 出了什么问题"。
- 需要生成脱敏复现包（Scrubbed Bundle）或起草带证据链的 GitHub Issue。

### 工作流程
1. **定位会话（Session Discovery）**：遵循 `references/session-discovery.md` 找到当前或目标历史会话的 Transcript 文件，并严格遵守 `references/context-safety.md`（禁止将长日志整段读入主上下文）。
2. **多维专项分析（Analyst Prompts）**：按问题类型使用 `prompts/` 下的分析模板（`skill-timeline`、`plan-adherence`、`repeated-work`、`stumbles`、`cost-and-time`、`quality-evidence`、`request-conflicts`）派发分析子代理提取证据。
3. **出具诊断报告（`templates/report.md`）**：汇总问题时间线与根因，所有结论必须附带 `path:line` 原始日志引用。
4. **可选脱敏打包与 Issue 起草**：遵循 `references/redaction-policy.md`（`prompts/scrub.md` + `scrub-audit.md`）对敏感信息脱敏并保留引用证据完整，经用户批准后导出或提交。

---

## 技能使用总结

| 技能                           | 何时使用                       | 核心价值                        |
| ------------------------------ | ------------------------------ | ------------------------------- |
| brainstorming                  | 任何创意或新功能工作开始前     | 意图澄清、动态分级、Builder 审查 |
| writing-plans                  | 有规格后，写代码前             | 聚焦决策、接口契约与审查焦点    |
| executing-plans                | 有计划，希望低成本连续内联执行 | 账本追踪 + 原生连续执行 + 终局审查 |
| subagent-driven-development    | 有计划，当前会话高可靠自治迭代 | 独立子代理 + 单次双结论审查     |
| test-driven-development        | 实现任何功能或修复 Bug         | 红绿重构、全量套件验证、可证伪性 |
| systematic-debugging           | 遇到任何 Bug 或意外行为        | 四阶段根因定位、杜绝瞎猜        |
| verification-before-completion | 声称完成或提交之前             | 跑命令拿证据、诚实汇报          |
| using-git-worktrees            | 需要隔离开发环境时             | 项目内 `.worktrees/` 隔离开发   |
| finishing-a-development-branch | 实施完成，测试通过             | 安全收尾、跨平台 PR、保护未跟踪文件 |
| dispatching-parallel-agents    | 多个独立问题需同时处理         | 并行派发子代理、节省时间        |
| requesting-code-review         | 完成任务后、合并前             | 质量门禁、显式记录未决判断      |
| receiving-code-review          | 收到审查反馈后                 | 技术核实、拒绝盲从              |
| using-superpowers              | 任何对话开始时                 | 技能系统入口与平台工具映射      |
| writing-skills                 | 创建/编辑技能时                | 文档 TDD、形式匹配与微测试      |
| diagnosing-superpowers         | 会话表现异常或技能未按预期工作 | 基于 Transcript 证据链诊断根因  |
