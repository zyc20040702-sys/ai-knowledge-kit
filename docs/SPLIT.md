# 公私仓怎么拆

一页说清：**什么进本开源 kit，什么必须留在私有项目仓。**

## 进本仓（public · ai-knowledge-kit）

- 可复用 **Skill** 手续（已脱敏：无真实路径、无账户名、无岗位专名）
- 知识库 **目录骨架** 与空 README
- 通用 **SOUL / HEALTH / PROCEDURES**（检索优先、分岗原则、健康检查、先检索再实现）
- 薄 **AGENT / INDEX** 模板
- 上游已 MIT 的方法论文档指针（如 shengjiang-skills 收录说明）
- `SYNC.example.md`、本拆分说明

## 进私有仓（private · 你的项目知识库）

- 真实 **项目档案** 与源码包（`03.项目档案/<项目>/`）
- `system/USER.md`、记忆日志、变更日志、健康状态、`_本周.md` 类周重点
- **团队协作**：具名 Bot / 岗位对照表、群聊规矩、同步口令
- 私密硬规则（仓必须 private、仅哪些账户可访问等）
- 客户数据、密钥、内部 URL、未脱敏案例

## 推荐工作流

1. Fork / clone 本 kit，或把 `skills/` + `templates/` 拷进私有仓。
2. 私有仓用 GitHub **private**；本仓保持 **public**。
3. 通用规则有改进时：先改本开源仓，再 cherry-pick / 手工同步进私有仓的 `system/`。
4. 业务与项目变更**只**提交私有仓。

## 不要做的事

- 不要把私有仓整仓 history fork 成 public（旧提交可能泄漏）。
- 不要在开源 Skill 里写死 `<KB_ROOT>` 以外的本机绝对路径或私有 Git URL。
- 不要把「本开源仓永久 private」之类规则写进 public README。
