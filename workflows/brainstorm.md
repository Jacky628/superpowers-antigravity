---
description: 启动需求头脑风暴，在编写代码前明确设计方案
---
1. 加载并运行 `.agent/skills/brainstorming/SKILL.md` 指令。
2. 严格遵守 HARD-GATE：在用户确认设计前，不写任何代码、不调用任何实现技能。
3. 按照 SKILL 中的 8 步检查清单逐步推进：探索上下文 → 逐个提问 → 提出 2-3 个方案 → 分段呈现设计并逐段获得确认。
4. 将确认后的设计写入 `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md` 并提交。
5. 对 spec 做一次内联自审（占位符 / 矛盾 / 范围 / 歧义），修复后请用户复核。
6. 用户批准后，转入 `/write-plan`（writing-plans 技能）创建实施计划。
