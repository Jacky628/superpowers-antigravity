---
description: 根据需求说明书创建详细的逐步实施计划
---
1. 加载并运行 `.agent/skills/writing-plans/SKILL.md` 指令。
2. 将任务拆分为可独立完成的极小原子任务，步骤使用 `- [ ]` 复选框语法便于追踪。
3. 确保每个任务包含明确的验证步骤（运行什么命令、期望什么结果）。
4. 将计划写入 `docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md`。
5. 完成后提示两种执行方式：`subagent-driven-development`（当前会话、子代理隔离，推荐）或 `/execute-plan`（executing-plans，分批 + 人工检查点）。
