# ai-knowledge-kit

**多 AI 共用知识库的开源脚手架：通用 Skill、固定规则与目录模板；真实项目与私密资料请另放私有仓。**

Open-source kit for multi-AI knowledge bases: reusable skills, procedures, and folder scaffolding — keep your private projects elsewhere.

仓库：https://github.com/zyc20040702-sys/ai-knowledge-kit

## 这是什么 / What

| 本仓（public） | 你的私有仓（private） |
| --- | --- |
| `skills/` 可复用工作流手续 | 真实项目源码与业务档案 |
| `templates/knowledge-base/` 空骨架 + 通用规则 | `system/USER.md`、日志、健康状态、周重点 |
| `vendor` 式收录的上游 MIT 说明（shengjiang） | 团队具名 Bot 对照表、群聊规矩、同步口令 |
| 文档：如何拆分公私仓 | 密钥、客户数据、私密同步 URL |

详见 [`docs/SPLIT.md`](docs/SPLIT.md)。

## 怎么用 / How to use

1. **复制模板到私有知识库根**  
   将 `templates/knowledge-base/` 下的目录与文件拷到你的 `<KB_ROOT>`（建议 Git **private** 仓）。
2. **安装 / 引用 skills**  
   - 直接把本仓 `skills/<id>/` 拷进私有库或客户端 skill 目录；或  
   - 按各 `SKILL.md` 说明用 `npx skills add …`（仅在你同意后）。
3. **补私有层**  
   自行创建 `system/USER.md`、变更日志、周重点、团队协作文档；把 `SYNC.example.md` 改写成私有 `SYNC.md`。
4. **真实项目只进私有仓**  
   用 `03.项目档案/_模板/` 开新项目文件夹；**不要**把源码包推到本开源仓。

Companion private KB（请换成你的私有仓链接）: `<YOUR_PRIVATE_REPO_URL>`

## 目录一览

```
ai-knowledge-kit/
  README.md
  LICENSE                 # MIT, Copyright (c) 2026
  docs/SPLIT.md           # 公私仓边界
  SYNC.example.md         # 同步约定示例（无永久私密硬绑）
  skills/                 # 脱敏后的可复用 Skill
  templates/knowledge-base/
    AGENT.md INDEX.md
    00.收件箱/ 01.资料库/ 02.输出区/ 03.项目档案/_模板/
    system/SOUL.md HEALTH.md PROCEDURES.md
    01.资料库/shengjiang-skills/   # 上游 MIT 收录说明
```

## 上游致谢

生姜（Shengjiang）相关 Skill 与收录说明基于 [shengjiang-skills](https://github.com/aslanyushengjiang-coder/shengjiang-skills)（**MIT**，Copyright (c) 2026 Shengjiang）。完整上游请直接使用该仓库；本 kit 内为便于开箱的副本 / 指针。

## License

本仓库代码与文档默认 **MIT**（见 `LICENSE`），Copyright (c) 2026。  
`templates/knowledge-base/01.资料库/shengjiang-skills/` 保留上游 Shengjiang MIT 声明。
