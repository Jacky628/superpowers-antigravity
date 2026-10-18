---
description: 根据需求说明书创建详细的逐步实施计划
---
1. 加载并遵循 `writing-plans` 技能（优先读取 `<skills>` 中已发现的路径，如 `~/.gemini/antigravity/skills/writing-plans/SKILL.md` 或 `.agent/skills/writing-plans/SKILL.md`）。
2. 计划记录决策而非照抄全量代码：包含 `Global Constraints`（全局约束）、每个任务的 `Interfaces`（接口契约）、精确函数签名与测试断言，以及 `Review Focus`（易漏边界条件）。
3. 将任务拆分为具备独立测试闭环的原子任务，步骤使用 `- [ ]` 复选框语法，并包含明确的验证命令与预期输出。
4. 将计划写入 `docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md`，完成比例与占位符自检后请用户审阅计划文件。
5. 用户确认计划后，推荐两种执行模式之一：`subagent-driven-development`（子代理驱动 + 任务级审查）或 `/execute-plan`（`executing-plans` 原生内联连续执行 + 终局全分支审查）。
