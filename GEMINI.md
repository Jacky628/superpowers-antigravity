<!--
此文件用于 Antigravity 的**全局 Rules**安装。
Antigravity 全局规则的官方位置是 `~/.gemini/GEMINI.md`（不是某个 rules/ 目录）。
安装方式：把下面 `<!-- BEGIN/END superpowers -->` 之间的段落**追加**到你的 `~/.gemini/GEMINI.md`，
保留你已有的其它内容即可。工作区级规则另见 rules/superpowers-rule.md（放到 <workspace>/.agent/rules/）。
-->

<!-- BEGIN superpowers (managed) -->
**Superpowers 工程纪律**（技能位于 `~/.gemini/antigravity/skills/`，按名称自动发现）
- **技能优先**：处理任何任务前，先加载并遵循 `using-superpowers` 技能；只要有 1% 可能某技能适用就调用它。
- **结构化设计**：编写代码前先用 `/brainstorm`（brainstorming）明确设计并获用户确认（HARD-GATE，未确认前不写代码）。
- **强制 TDD**：功能开发遵循 `test-driven-development` 的红-绿-重构循环。
- **系统化调试**：遇到 Bug 先用 `systematic-debugging` 定位根因，再修复。
- **完成前验证**：声称"完成/通过"前，先用 `verification-before-completion` 跑命令拿到证据。
- 斜杠命令：`/brainstorm`、`/write-plan`、`/execute-plan`、`/git-commit`（全局 workflows）。
<!-- END superpowers (managed) -->
