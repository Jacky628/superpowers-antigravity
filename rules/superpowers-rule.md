---
trigger: always_on
---

# Superpowers Development Rules

## 核心准则 (Prime Directives)
- **技能优先**：在处理任何任务前，优先加载并遵循 `using-superpowers` 技能（`SKILL.md`）。
- **强制 TDD**：所有功能开发必须遵循 `test-driven-development` 技能（`SKILL.md`）的红-绿-重构循环。
- **结构化设计**：在编写代码前，必须通过 `/brainstorm`（`brainstorming` 技能）明确设计规范并获用户确认（HARD-GATE）。
- **小步快跑**：所有实施计划必须通过 `/write-plan`（`writing-plans` 技能）拆分为极小的原子任务。
- **完成前验证**：声称"完成/通过"前，必须遵循 `verification-before-completion` 技能运行验证命令并获取证据。

## 常用命令 (Workflows)
- `/brainstorm`: 启动需求头脑风暴。
- `/write-plan`: 创建实施计划。
- `/execute-plan`: 执行并审查计划。
- `/git-commit`: 规范化提交更改。

## 提交规范
- 提交格式：Conventional Commits（中文）