---
name: find-skills
description: >-
  当用户问「怎么做 X」「有没有 X 的 skill」「帮我找能做 X 的技能」，或想从开放生态扩展代理能力时使用：先查 skills.sh
  排行，再搜索并核验质量后推荐，征得同意再装。
---
# Find Skills

帮助从开放 Agent Skills 生态发现并安装专门技能。来源：[vercel-labs/skills · find-skills](https://github.com/vercel-labs/skills)（MIT）。

## 何时使用

- 用户问「怎么做 X」且 X 可能已有现成 skill
- 「找一个做 X 的 skill」「有没有能…的 skill」
- 「你能做 X 吗」且 X 是专项能力
- 想扩展代理能力、搜工具/模板/工作流
- 希望在设计、测试、部署等某领域有现成助手

## Skills CLI（开放生态）

- `npx skills find [query] [--owner <owner>]` — 按关键词搜索
- `npx skills add <package>` — 从 GitHub 等安装
- `npx skills update` — 更新已装 skills
- 浏览：https://skills.sh/

在 Grok Bot 技能库内落盘时，用 UpdateSkill 写入（或改写）SKILL；**征得用户同意后再装**，不要擅自安装。

## 流程

### 1. 弄清需求

1. 领域（如 React、测试、设计、部署）
2. 具体任务
3. 是否常见到可能已有 skill

### 2. 先看排行榜

先打开 [skills.sh](https://skills.sh/) 看领域热门 skill，再跑 CLI 搜索。常见高质量来源：`vercel-labs`、`anthropics`、`microsoft`。

### 3. 搜索

```bash
npx skills find [query] [--owner <owner>]
```

示例：`npx skills find react performance`、`npx skills find pr review`。

### 4. 推荐前核验质量

不要只凭搜索结果推荐。至少看：

1. **安装量** — 优先 1K+；&lt;100 要谨慎
2. **来源信誉** — 官方/知名组织优于无名作者
3. **仓库星标** — 源仓 &lt;100 star 宜持怀疑

并对照工作区**已有** skill：已有同类能力则标「可不装」，除非明显更优。

### 5. 向用户呈现

每项给出：名称与用途、安装量/来源、安装命令或落盘方式、skills.sh / GitHub 链接。

### 6. 同意后再装

- Cursor / 本机 CLI：`npx skills add <owner/repo@skill> -g -y`（仅在用户明确同意后）
- Grok Bot 技能库：用 UpdateSkill 写入可复用 SKILL.md（仅在用户明确同意后）

重大或高风险 skill 可先交主管岗过目。

## 常见类别关键词

| 类别 | 示例查询 |
| --- | --- |
| Web | react, nextjs, typescript, css, tailwind |
| 测试 | testing, jest, playwright, e2e |
| DevOps | deploy, docker, kubernetes, ci-cd |
| 文档 | docs, readme, changelog, api-docs |
| 代码质量 | review, lint, refactor, best-practices |
| 设计 | ui, ux, design-system, accessibility |
| 效率 | workflow, automation, git |

## 搜不到时

1. 说明未找到匹配 skill
2. 直接用现有能力帮用户做
3. 可选：`npx skills init my-xyz-skill` 自建，或用 UpdateSkill 沉淀团队流程
