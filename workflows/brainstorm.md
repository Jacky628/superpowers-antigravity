---
description: 启动需求头脑风暴，在编写代码前明确设计方案
---
1. 加载并遵循 `brainstorming` 技能（优先读取 `<skills>` 中已发现的路径，如 `~/.gemini/antigravity/skills/brainstorming/SKILL.md` 或 `.agent/skills/brainstorming/SKILL.md`）。
2. 严格遵守 HARD-GATE：在用户批准完整描述或设计文档前，不写任何业务代码、不调用任何实现技能（明确同意的投石问路 Spike 除外）。
3. 通过单问题对话澄清意图与"为什么"，分段（200-300 字）复述理解并标明猜测，明确哪些技术栈/实现决策由用户把控、哪些交由 Agent 决定。
4. 评估任务体量（Size the Work）：清晰微任务直接执行；小改动在对话中确认后执行；项目级需求写入 `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md` 并提交。
5. 对项目级设计文档派发子代理执行 `builder-check-prompt.md` 审查，收敛问题并与用户完成一轮确认。
6. 用户批准设计文档后，转入 `/write-plan`（`writing-plans` 技能）创建实施计划。
