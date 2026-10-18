---
description: 原生内联连续执行实施计划并进行终局全分支审查
---
1. 加载并遵循 `executing-plans` 技能（优先读取 `<skills>` 中已发现的路径，如 `~/.gemini/antigravity/skills/executing-plans/SKILL.md` 或 `.agent/skills/executing-plans/SKILL.md`）。
2. 完成计划预检（Pre-Flight）与工作区账本初始化（`.superpowers/sdd/<plan>/`），通过 `task-start` 与 `task-done` 记录每个任务的 TDD 执行与测试证据。
3. 连续执行完所有任务（仅在遇到破坏性或不可逆阻塞时停下询问），最后派发一次全分支代码审查并转入 `finishing-a-development-branch`。
