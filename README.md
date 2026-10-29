# ai-knowledge-kit

**多 AI 共用知识库的开源脚手架：通用 Skill、固定规则与目录模板；真实项目与私密资料请另放私有仓。**

Open-source kit for multi-AI knowledge bases: reusable skills, procedures, and folder scaffolding — keep your private projects elsewhere.

仓库：https://github.com/zyc20040702-sys/ai-knowledge-kit

## 这套东西干什么用

给多个 AI（包括 Cursor 里的助手）一套可以整份复制的本地知识库结构，加上能按步骤执行的 Skill。你可以用它搭库、把已有资料接进索引、做健康巡检，也可以按手续做短视频调研和成片、微信小程序开发与虚拟支付、文献和作业成稿、界面打磨与设计审查，而不把私密项目公开。

克隆这一份，就能直接拿走这些东西：

- **一套能开工的知识库骨架。** 复制 [`templates/knowledge-base/`](templates/knowledge-base/) 就有收件箱、资料库、输出区、项目档案四层目录，以及 `AGENT.md`、`INDEX.md`、协作风格和固定规则。多个助手按同一入口读库。
- **可运行的搭库、接资料和巡检。** [`skills/shengjiang-knowledge/`](skills/shengjiang-knowledge/SKILL.md) 带初始化、资料扫描和健康检查脚本。先只读审计和预览，确认后再写入索引；原件默认留在原地。
- **短视频调研、拆解和成片手续。** 用你自己的接口做公开社媒采集，把一条视频拆成观看价值、逐段推进、评论、分镜和逐字稿，或按主题 / 脚本调用 MoneyPrinterTurbo 生成 MP4。
- **微信小程序开发、虚拟支付和选型。** 页面结构、预览发布和云开发场景的开发手续；虚拟商品的签名、回调和发货清单；以及只打开线上雷达做框架对比和迁移诊断。
- **文献和作业的成稿链。** 多份材料先整理成共识、缺口和提纲，再写成文献述评、案例分析、课程作业，以及上台用的分页内容稿。
- **界面审查和产品口径。** Impeccable 把前端打磨到可交稿，Taste 审查落地页像不像模板；产品手续先评估想法、锁住口径，再交给设计和开发。

技能逐条说明在下面。想按文件夹 id 深链时，打开 [`skills/CATALOG.md`](skills/CATALOG.md)。

## 已有能力一览

本仓 `skills/` 下共 **18** 个技能文件夹（不含说明文件）。克隆后可以在本目录直接跑脚本的是 `shengjiang-knowledge`（初始化、扫描、巡检）。`shengjiang-viral-breakdown` 同时带有来源对照和评测记录。其余文件夹提供 `SKILL.md` 手续；正文里点名的采集脚本、成片程序、界面检测器或小程序兄弟技能，按各技能说明另行准备。

### 知识库与资料

| 显示名 | 文件夹 id | 详细功能 |
| --- | --- | --- |
| 知识库搭建、资料接入与健康检查 | [`shengjiang-knowledge`](skills/shengjiang-knowledge/SKILL.md) | 把已有文件夹变成助手能稳定开工的本地知识库：先定根目录，只读查看入口和一两层目录，给出修改预览，确认后才写入。支持通用四层骨架、企业三库（产品库 / 客户库 / 技能库）和自媒体内容工作台（定位、对标、选题、草稿到发布复盘）。从零搭库用 `scripts/init_knowledge_base.py` 预览后再 `--apply`；接资料用 `scripts/scan_materials.py` 出清单并建索引，原件默认不搬家；健康检查用 `scripts/audit_knowledge_base.py` 标出断链、冲突和入口风险，修复前要确认。本目录含这三支脚本、模式说明和企业三库使用说明。 |

### 短视频与内容

| 显示名 | 文件夹 id | 详细功能 |
| --- | --- | --- |
| 全平台社媒调研 | [`shengjiang-research`](skills/shengjiang-research/SKILL.md) | 把公开社媒调研收成可回指证据的资产：账号、作品、评论、字幕和表现数据，覆盖抖音、小红书、视频号、TikTok、YouTube、B 站、微博等 TikHub 端点。可能扣费的批量请求前先给出端点、次数和费用预览，先跑 1–3 条小样本再扩大；逐字稿优先官方字幕，否则只用你自己的第三方转写。产出是 JSON、Markdown、CSV 或 Excel 加证据索引，Key 自行保管，本仓不含密钥。本目录是手续说明，采集脚本需按正文另行准备。 |
| 爆款短视频拆解 | [`shengjiang-viral-breakdown`](skills/shengjiang-viral-breakdown/SKILL.md) | 拆一条指定短视频，或复盘自己的片子，回答三件事：说中了哪类人的什么处境、怎样让人看下去、哪些手法适合自己的账号。按四步交付：登记链接 / 原片 / 数据 / 评论，写观看价值与逐段推进，分析高赞评论，再给分镜和带时间戳的逐字稿。已有材料直接分析；只有链接、需要下载或转写时交给 `shengjiang-research`；缺原片、评论或时间就做部分拆解并写明缺口。本目录含手续、来源对照和评测记录。 |
| MoneyPrinterTurbo 短视频成片 | [`moneyprinterturbo`](skills/moneyprinterturbo/SKILL.md) | 给出主题或口播脚本后，调用本机已安装的 [MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo)，生成并交付一条 MP4。默认中文、竖屏 9:16、库存素材、Edge TTS 中文配音、字幕和背景音乐，一次一条。缺大模型或素材 Key 时只索取清单里的凭据，不打印密钥或完整配置；生成后检查节奏、字幕和画幅。本目录是手续，上游程序需另行安装。 |

### 微信小程序

| 显示名 | 文件夹 id | 详细功能 |
| --- | --- | --- |
| 微信小程序开发 | [`miniprogram-development`](skills/miniprogram-development/SKILL.md) | 创建、修改、调试、预览、上传或发布微信小程序，覆盖页面与组件、`project.config.json`、开发者工具，以及项目确实在用云开发时的 `wx.cloud`。主体类型、测试号限制、消息推送与客服回调、搜索收录按手续里的阅读顺序处理；虚拟支付改走下一项。输入是小程序工程和当前任务，输出是改过的页面、配置和发布前检查项。本目录是手续全文，它点名的兄弟技能与 reference 不在本文件夹。 |
| 微信小程序虚拟支付 | [`miniprogram-virtual-payment`](skills/miniprogram-virtual-payment/SKILL.md) | 从确认业务模式到上线的虚拟支付清单：道具直购、代币充值、iOS 走 Apple IAP、服务端 `paySig` / `signature`、回调与幂等发货、沙箱和错误码。执行前收集模式、阶段、服务端语言、是否已开通、是否要 iOS。手续含企业 / 个体户开通清单，并另有个人主体补充（认证、备案、类目以微信「虚拟支付：个人」文档为准）；`appKey` 只留服务端，实物商品用普通微信支付。输出是可勾选的进度清单和按官方文档落地的实现要点。 |
| 小程序雷达 | [`skill-1792424595149`](skills/skill-1792424595149/SKILL.md) | 打开线上小程序雷达做技术选型：对比 Taro、uni-app、原生、组件库和云能力，并看维护状态与迁移风险。Radar 筛选、Compare 对比、Advisor 给结论和证据、Doctor 诊断粘贴的项目配置、Weekly 看周报。结论用简体中文短述推荐与风险；默认只用官方站点 [miniapp.jjc.fun](https://miniapp.jjc.fun)（备用 [wechat-miniapp-radar.vercel.app](https://wechat-miniapp-radar.vercel.app)）。仓库为 GPL-3.0，本仓不随包分发其代码，也不代替上面的开发手续或虚拟支付手续。 |

### 产品与设计

| 显示名 | 文件夹 id | 详细功能 |
| --- | --- | --- |
| 产品/市场助手首次设置 | [`getting-started`](skills/getting-started/SKILL.md) | 产品 / 市场助手第一次被导入、需要初始设置时使用。一次只问一个问题并等回答：怎么称呼、负责哪个产品（名字加一句话）、团队里还有设计 / 研发 / 法务谁、有没有硬约束（只走本地、要避开的合规话题、语言）。答完后把称呼、产品名、约束和交接方式记成可复用画像（不含密钥），并确认工作方式：先评估想法再背书，口径锁定后再开发，云能力和合规项在明确解冻前先搁置。最后问第一件事是市场扫描、功能优先级，还是锁定某一个想法的口径。 |
| 产品与市场口径 | [`product-market-ops`](skills/product-market-ops/SKILL.md) | 负责市场与竞品摘要、功能先后，并在设计和开发开工前用白话写清做什么、不做什么，标清是只讨论还是现在就做。想法先看是否贴合：贴合就推进，不贴合就写风险、改法或建议先不做。需要 OCR、语音、大模型或敏感数据同意的功能先放进待办，直到负责人明确解冻且同意文案已过目。交接顺序是口径、线框、实现、真机验收；群聊里说「已拍板」之后只报阻塞和纠错，完整群规放私有库。 |
| 前端界面打磨与审计（Impeccable） | [`impeccable`](skills/impeccable/SKILL.md) | 设计、改版、打磨或审计落地页、产品 UI、仪表盘、表单和空状态，覆盖层级、排版、配色、间距、动效、无障碍和反模式。用户说 `/impeccable`、polish、audit、init 或 DESIGN.md 时，先加载项目上下文，再按子命令执行（`init` / `document`、`polish`、`audit` / `critique`，以及 bolder、quieter、typeset、colorize、animate、live）。本技能把界面做到可交稿；营销页像不像模板交给 Taste，纯后端不用。本目录是手续摘要，上游为 [pbakaus/impeccable](https://github.com/pbakaus/impeccable)，完整脚本不在本文件夹。 |
| 落地页设计审查（Taste） | [`taste-skill`](skills/taste-skill/SKILL.md) | 以设计总监身份审查落地页、营销站、作品集或公开网页，抓模板感和 AI 味。输入是 URL、截图或前端源码；能跑则先 lint 和页面检查，再只看图多遍评审，按设计 40%、可用性 30%、创意 20%、内容 10% 打分，检查失败时总分不超过 6.0。交付简体中文报告：总评、四项分、硬伤、最多 3–5 条可执行改稿、可保留的优点，以及没跑的检查。默认只交审查意见，用户明确要求时才改代码；仪表盘和应用内屏不用这套规则，上游为 [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill)。 |

### 学业写作

几份学术手续可以串起来用：文献知识整理 → 文献述评成稿 → 学术汇报内容稿。作业和案例分析可以按题目直接成稿。需求落地交付负责按点名的文件清单交到能交差。

| 显示名 | 文件夹 id | 详细功能 |
| --- | --- | --- |
| 文献知识整理 | [`skill-1792044951145`](skills/skill-1792044951145/SKILL.md) | 把多份 Word、PDF 或笔记整理成述评、开题或汇报用的知识骨架，不直接代写终稿。先按模块抽取文本并写清模块如何咬合，再各写 2–4 条带来源的共识、3–6 条缺口（已有什么、还缺什么），以及一段研究情境、对象、问题和预期贡献。同时给出 Word 总—分—合提纲和 PPT 页级大纲，标明口径冲突和必须回原件核对的数字。材料里没有的文献和数据不写入，交付一份结构化整理稿。 |
| 文献述评成稿 | [`skill-1792044959882`](skills/skill-1792044959882/SKILL.md) | 整理稿齐备后，写成可提交的中文文献述评 Word，并给 10–16 页上台大纲。结构是总—分—合：开篇线索，分块写进展、发现和局限，再写跨模块共识与缺口，收束到切入点和贡献。同一指标多来源时选定主口径或标明差异；参考文献按 GB/T 7714，不全处标待补。默认学术书面语，篇幅常见 2500–4000 字；若要 PPT，走用户指定的幻灯片流程，口径与 Word 一致。 |
| 学术汇报内容稿 | [`skill-1792044966130`](skills/skill-1792044966130/SKILL.md) | 述评或报告写完、准备上台时，先压成分页内容稿再做幻灯片。主线是问题缘起、地图、共识、缺口或争议、结论；常见 10–16 页，每页标题加 3–6 条短句，关键页附讲者备注。另做一页模块关系，缺口一条一行并指向切入点。不加入正文没有的新结论，也不在这一步做配色；文末标明口径以哪份 Word 为准。 |
| 案例分析成稿 | [`skill-1792044989589`](skills/skill-1792044989589/SKILL.md) | 按题目和案例材料写成背景、问题、分析、建议的中文稿。先摘字数、指定理论、必答问题和评分点，并把事实与推断分开。用课程指定框架；没指定就选 1–2 个最贴合的并说明理由，每点分析都指回材料。建议按优先级写且能回溯到分析段；交付 Word 或正文，文末列材料来源。 |
| 课程作业成稿 | [`skill-1792044993463`](skills/skill-1792044993463/SKILL.md) | 按题目、评分标准或老师要求写成可提交的中文作业，并逐问核对是否答到。优先用提供的讲义、笔记、文献和数据，不够就标明待补充；按小问分节，先论点句再展开，图表用文字说明。参考文献按课程要求，常见 GB/T 7714，交付以 Word 为先。需要汇报时另走内容稿；基于给定材料综合改写。 |
| 需求落地交付 | [`skill-1792044997429`](skills/skill-1792044997429/SKILL.md) | 交付物已经点名（Word、PPT 或内容稿）时，按清单交到能交差。先写清类型、篇幅、受众和截止，确认整理稿就绪；没说偏好就交完整可用稿。先定正文口径再排版，上台主线是问题、共识或发现、缺口、结论，关键数字标注来源。改稿只动点名的页或段落；纯知识整理交给「文献知识整理」。 |

### 其它工具

| 显示名 | 文件夹 id | 详细功能 |
| --- | --- | --- |
| 查找可安装技能 | [`find-skills`](skills/find-skills/SKILL.md) | 问「怎么做 X」或「有没有做 X 的 skill」时，先看 [skills.sh](https://skills.sh/) 热门项，再用 `npx skills find` 搜索。核验安装量、来源和星标，并对照本仓已有技能，同类能力标明可以不装。推荐时给出名称、用途、安装量和链接；只有明确同意后才 `npx skills add`。搜不到就用现有能力直接做或按需自建；来源是 [vercel-labs/skills · find-skills](https://github.com/vercel-labs/skills)（MIT）。 |

### 知识库模板、固定规则与拆分文档

[`templates/knowledge-base/`](templates/knowledge-base/) 是空骨架。复制到你的私有知识库根目录后，助手按下面的入口开工。真实项目、用户画像和健康状态文件由你在私有库补上，模板里故意留空。

**四层目录**

| 目录 | 里面有什么 |
| --- | --- |
| [`00.收件箱/`](templates/knowledge-base/00.收件箱/README.md) | 暂时不知道放哪里的新文件。处理后挪到真实归属；默认 30 天提醒，不自动删除。 |
| [`01.资料库/`](templates/knowledge-base/01.资料库/README.md) | 可长期复用的资料、案例和方法。变多再拆子目录，并为每批维护 `_资料索引.md`。其中 [`shengjiang-skills/`](templates/knowledge-base/01.资料库/shengjiang-skills/README.md) 是上游 MIT 收录说明（版本见同目录 `VERSION`），可执行副本在本仓 `skills/shengjiang-*`。 |
| [`02.输出区/`](templates/knowledge-base/02.输出区/README.md) | 正在制作或已经完成的文案、报告、方案。原始资料留在资料库。模板目录是空的，成稿进私有库。 |
| [`03.项目档案/`](templates/knowledge-base/03.项目档案/README.md) | 一次真实项目的输入、决策、交付、复盘，以及无密钥、可复原的源码。 [`_模板/`](templates/knowledge-base/03.项目档案/_模板/README.md) 含 `01-输入` 到 `05-复盘`，外加 `source/` 与 `packages/`。无源码包不算完成交档。真实档案只放私有仓。 |

**AGENT 与 INDEX**

- [`AGENT.md`](templates/knowledge-base/AGENT.md)：每次新对话先读 `system/SOUL.md`、私有的 `system/USER.md`、`system/PROCEDURES.md`、`INDEX.md`，再看可选的周重点；若存在健康状态且超过 7 天没检查，提醒一次。回答知识库问题要指出依据文件。密钥、Cookie 和聊天数据库不进库。
- [`INDEX.md`](templates/knowledge-base/INDEX.md)：全库地图。先读薄入口和索引，再打开命中的原件。没有明确任务时，不对全库做递归搜索。文末按任务指向搭库、调研、拆解和「先检索再实现」。

**通用规则（`system/`）**

- [`SOUL.md`](templates/knowledge-base/system/SOUL.md)：简体中文、结论先行；信息不够就说明缺口；原件默认不搬家，破坏性操作先确认。
- [`PROCEDURES.md`](templates/knowledge-base/system/PROCEDURES.md) 里和日常最相关的三条：
  1. **检索优先。** 入口层（AGENT / INDEX / 规则）→ 索引层（去哪找）→ 结论层（短句、带路径）→ 原件层（源码包和长文只留在文件里）。记忆里只写根路径、进行中的项目名、技能主责和最近一次健康结论。
  2. **先检索再实现。** 遇到不熟悉的功能、协议或 SDK，先找维护活跃、许可证明确的同类开源，比较思路、依赖和弃用风险，再借鉴许可证兼容的结构，并写下来源。检索结论放进项目档案或当次答复。
  3. **健康检查。** 按 [`HEALTH.md`](templates/knowledge-base/system/HEALTH.md)，用 `shengjiang-knowledge` 的 `audit_knowledge_base.py` 对 `<KB_ROOT>` 巡检。结果分 `healthy`（继续用）、`attention`（安排确认，不自动搬文件）、`critical`（先处理 P0）。改了入口或批量接过资料后立即复查；超过 7 天未检查则提醒一次。
- 同一份规则还写了：从零搭库走初始化预览再 `--apply`；接一批资料先扫描清单、确认后索引；进行中的项目要留无密钥源码包；可能扣费的调研先给请求数和费用预览。

**公私边界文档**

- [`docs/SPLIT.md`](docs/SPLIT.md)：一页说明什么进本开源 kit（可复用手续、空骨架、通用规则、上游 MIT 指针），什么必须留在私有仓（真实项目与源码包、`USER.md`、日志、健康状态、具名协作表、密钥和客户数据），以及不要把私有仓整仓 history 公开。
- [`SYNC.example.md`](SYNC.example.md)：同步约定示例。复制到私有库改名为 `SYNC.md` 后再填真实远程。示例里只有占位地址 `<YOUR_GITHUB_OWNER>/<YOUR_PRIVATE_REPO>` 和 `<KB_ROOT>`，以及「小改可并进周整理、改了规矩要立刻拉齐、无口令不假定远程已进记忆」的节奏。业务文件不推入本 kit。

## 公开仓与私有仓

| 本仓（public） | 你的私有仓（private） |
| --- | --- |
| `skills/` 可复用工作流手续 | 真实项目源码与业务档案 |
| `templates/knowledge-base/` 空骨架 + 通用规则 | `system/USER.md`、日志、健康状态、周重点 |
| `vendor` 式收录的上游 MIT 说明（shengjiang） | 团队具名 Bot 对照表、群聊规矩、同步口令 |
| 文档：如何拆分公私仓 | 密钥、客户数据、私密同步 URL |

细节以 [`docs/SPLIT.md`](docs/SPLIT.md) 为准。私有库的同步口令按 [`SYNC.example.md`](SYNC.example.md) 改写，不要把真实地址提交回本仓。

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
  skills/CATALOG.md       # 与上文同一份技能清单，按文件夹 id 深链
  skills/<id>/SKILL.md    # 18 个可复用 Skill
  templates/knowledge-base/
    AGENT.md INDEX.md
    00.收件箱/ 01.资料库/ 02.输出区/ 03.项目档案/_模板/
    system/SOUL.md HEALTH.md PROCEDURES.md
    01.资料库/shengjiang-skills/   # 上游 MIT 收录说明
```

## 上游致谢

生姜（Shengjiang）相关 Skill 与收录说明基于 [shengjiang-skills](https://github.com/aslanyushengjiang-coder/shengjiang-skills)（**MIT**，Copyright (c) 2026 Shengjiang）。完整上游请直接使用该仓库；本 kit 内为便于开箱的副本 / 指针。

本仓另收录手续摘要的上游包括 [vercel-labs/skills](https://github.com/vercel-labs/skills) 的 find-skills（MIT）、[pbakaus/impeccable](https://github.com/pbakaus/impeccable)、[Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill)、[harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo)，以及微信小程序生态的线上雷达 [justjavac/wechat-miniapp-radar](https://github.com/justjavac/wechat-miniapp-radar)（GPL-3.0，本 kit 只链到官方站点，不随仓分发其代码）。

## License

本仓库代码与文档默认 **MIT**（见 [`LICENSE`](LICENSE)），Copyright (c) 2026。  
`templates/knowledge-base/01.资料库/shengjiang-skills/` 保留上游 Shengjiang MIT 声明。
