---
name: MoneyPrinterTurbo 短视频
description: 当用户要用 MoneyPrinterTurbo / mpt 从主题或脚本一键生成短视频成片（含安装、配置、缺 Key 处理、定位 MP4）时使用。
---
# MoneyPrinterTurbo 短视频生成

用户给出主题或脚本后，自动完成生成并交付 MP4。不要停在“请自行安装”的说明上。

## 本机已就绪路径

- 项目目录：`/workspace/MoneyPrinterTurbo`
- 虚拟环境：`/workspace/MoneyPrinterTurbo/.venv`（已安装 `mpt` 1.3.8）
- 官方 Agent 助手：`/workspace/MoneyPrinterTurbo/docs/skill/mpt_agent.py`
- 上游仓库：https://github.com/harry0703/MoneyPrinterTurbo
- 数据目录默认：`$MPT_HOME` 或 `~/.local/share/MoneyPrinterTurbo`（成片在 `storage/tasks`）

## 默认参数

除非用户另说：中文、竖屏 `9:16`、Pexels 素材、Edge TTS 中文配音、字幕、背景音乐，生成 1 条。

## 执行步骤

1. **优先用官方 helper**（workdir 必须是助手所在目录）：

```bash
cd /workspace/MoneyPrinterTurbo/docs/skill
uv run --no-project --python 3.11 python mpt_agent.py --subject "<主题>"
```

超时至少 20 分钟；不要用 sleep/ps 轮询。若平台不支持长时间前台等待，改后台跑并等完成通知，再读 `latest-result.json`。

2. **helper 不可用时**，用本机已装的 `mpt`：

```bash
source /workspace/MoneyPrinterTurbo/.venv/bin/activate
export MPT_HOME=/workspace/MoneyPrinterTurbo/mpt-data
mpt cli --video-subject "<主题>"
```

3. **缺凭据**：退出码 10 / `MPT_NEEDS_INPUT` 时，只向用户要列出的 Key（一次问齐）。常用：
   - LLM：`MPT_LLM_PROVIDER` + `MPT_LLM_API_KEY`（及可选 `MPT_LLM_BASE_URL` / `MPT_LLM_MODEL_NAME`）
   - 素材：`MPT_PEXELS_API_KEY`（免费库存素材）
   - 付费文生视频类 Key 仅在用户明确要 AI 原创画面时再要，且收费确认标志出现时必须先征得同意再加重试参数。

4. **成功**（退出码 0）：从输出或 `~/MoneyPrinterTurbo/.agent-logs/moneyprinterturbo-video/latest-result.json` / `$MPT_HOME` 下任务目录取 `VIDEO_FILE`，把成片路径交给用户（可附带发送文件）。

5. **失败**：读短错误与日志尾部，可修复则重试一次；需要新 Key 再问用户。

## 硬性约束

- 绝不打印 API Key、完整 `config.toml` 或含凭据片段。
- 不要并行跑多个生成任务。
- 不要默认启动 Docker / WebUI；CLI/helper 即可。
- Edge TTS 免费无需 Key；真正生成前至少要有可用 LLM Key，用 Pexels 时还要 Pexels Key。
- 群聊里若需用户粘贴 Key，引导到与视频剪辑岗的 1:1 私聊完成，勿在群里贴密钥。

## 协作分工（可选）

- 剧本/分集：编剧侧先出稿，再把定稿主题或脚本交给本 skill 生成。
- 钩子文案/标题：文案侧可并行，不阻塞成片生成。
- 成片质检：生成后检查节奏、字幕可读性、画幅是否符合目标平台。
