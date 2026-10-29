# 知识库 · 导航（模板）

> 这是全库地图。先用本文件判断去哪里找，再读取原始资料。  
> **真实项目、用户画像、团队具名对照表请只放在私有仓**；本模板不含业务数据。

## 核心入口

| 路径 | 作用 |
| --- | --- |
| `AGENT.md` | 跨助手主入口 |
| `system/SOUL.md` | AI 协作风格 |
| `system/USER.md` | （私有）用户、业务与目标 |
| `system/PROCEDURES.md` | 遇到 X 就做 Y 的固定规则 |
| `system/HEALTH.md` | 健康检查频率与状态处理 |
| `system/state/` | （私有）巡检报告 |
| `SYNC.example.md` → 私有 `SYNC.md` | 与私有真相源的同步约定 |

## 资料与产出

| 目录 | 放什么 | 不放什么 |
| --- | --- | --- |
| `00.收件箱/` | 暂时无法判断归属的新输入 | 已经明确归属的长期资料 |
| `01.资料库/` | 可长期复用的资料、案例和参考 | 正在制作的成稿；密钥 |
| `02.输出区/` | 文案、报告、方案和交付物 | 原始输入 |
| `03.项目档案/` | 真实项目全过程 + 可复原源码包 | 与项目无关的长期资料；密钥 |
| `skills/` | 可重复执行的 AI 工作流 | 单次业务资料 |

## 检索纪律（先索引后原件）

1. 先读入口薄文件与本 INDEX，再查索引层，再打开**命中路径**的原件。
2. **禁止**无明确任务目标时对全库做 `find` / `rg` / 递归浏览。
3. 大材料只在文件里；记忆与对话只留索引句与结论。

## 高频任务指路

| 任务 | 先读 |
| --- | --- |
| 搭建 / 巡检 / 接资料 | kit `skills/shengjiang-knowledge/SKILL.md` |
| 社媒调研 | kit `skills/shengjiang-research/SKILL.md`（自备 Key） |
| 拆解爆款短视频 | kit `skills/shengjiang-viral-breakdown/SKILL.md` |
| shengjiang 原件说明 | `01.资料库/shengjiang-skills/_来源说明.md` |
| 项目留档（须含源码包） | `03.项目档案/README.md` |
| 不熟悉功能先检索再实现 | `system/PROCEDURES.md` |

## Skill 分岗

在私有库维护「Skill → 默认主责岗位」对照表；原则摘要见 `system/PROCEDURES.md`。
