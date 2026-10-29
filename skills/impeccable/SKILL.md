---
name: Impeccable
description: >-
  当需要设计、改版、打磨、审查或审计前端界面（落地页、产品
  UI、仪表盘、表单、空状态等）时使用；覆盖视觉层级、排版、配色、间距、动效、无障碍、反模式检测。用户说
  /impeccable、polish、audit、init、DESIGN.md 时必读。
---
# Impeccable 设计技能

完整安装路径：`/home/box/.grok/skills/impeccable`（同内容亦在 `~/.cursor/skills/impeccable`）。
上游：https://github.com/pbakaus/impeccable · 文档：https://impeccable.style

本技能用于**把前端界面做到可交稿品质**：新做、改版、打磨、审查、审计。先读完整 `SKILL.md` 与对应 reference，再动手。

## 每次开干前

1. 用 Read 打开 `/home/box/.grok/skills/impeccable/SKILL.md`，并按其 Setup 执行。
2. 在项目根跑上下文（cwd 保持在用户项目）：

```bash
/home/box/.grok/skills/impeccable/scripts/impeccable context
```

若失败：先告知「上下文加载未跑，改为直接读项目现有 PRODUCT.md / DESIGN.md」，再继续。
3. 动 UI 前读 `/home/box/.grok/skills/impeccable/reference/craft-floor.md`。
4. 有明确子命令时，读 Commands 表对应 reference（如 polish → polish.md）。

## 常用子命令（对用户可说 /impeccable …）

| 意图 | 子命令 |
|------|--------|
| 首次接入项目 | `init` → 再 `document` 生成 DESIGN.md |
| 打磨现有页 | `polish` |
| 审查/审计 | `audit` / `critique` |
| 更大胆 / 更克制 | `bolder` / `quieter` |
| 排版 / 配色 / 动效 | `typeset` / `colorize` / `animate` |
| 活浏览器迭代 | `live` |

完整命令表与禁令以安装目录内 `SKILL.md`、`reference/` 为准，不要凭记忆省略。

## 与 Taste Skill 的分工

- **Impeccable**：做与改产品/营销前端，并带检测器与 DESIGN.md 工作流。
- **Taste Skill**：偏落地页/营销页「AI 味」审查打分（设计总监把关）。
两者可并用：先 Impeccable 落地，再 Taste 审查。

## 不做

纯后端、无界面任务；不要在未解冻云能力的小程序壳上承诺 OCR 等云功能视觉。
