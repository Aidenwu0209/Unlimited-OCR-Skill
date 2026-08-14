# Unlimited-OCR-Skill

[English](README.md) | 简体中文

基于[百度 Unlimited-OCR](https://github.com/baidu/Unlimited-OCR) 的可移植 Agent Skill，用于长文档 OCR 和结构化 Markdown 解析。支持：

- **百度智能云 API**：通过官方异步接口处理图片、PDF、OFD、Office 文档、文本文件与公开 HTTPS URL。
- **本地服务**：通过 SGLang 或其他 OpenAI-compatible `/v1/chat/completions` 服务处理图片和 PDF。
- 返回完整 Markdown，同时保留可审计 JSON 包装。
- 包含超时、下载上限和结构化错误。

## 在主流 Agent 中安装

本仓库遵循开放的 [Agent Skills](https://agentskills.io) 目录规范。`skills` CLI 能发现并安装到 Codex、Claude Code、Cursor、OpenCode、OpenClaw 等兼容客户端：

```bash
npx skills add Aidenwu0209/Unlimited-OCR-Skill \
  --skill unlimited-ocr-document-parsing -g
```

也可以明确安装到几个常见编码 Agent：

```bash
npx skills add Aidenwu0209/Unlimited-OCR-Skill \
  --skill unlimited-ocr-document-parsing \
  -a codex -a claude-code -a cursor -a opencode -g -y
```

### OpenClaw

克隆后可让 OpenClaw 直接安装 Skill 子目录：

```bash
git clone https://github.com/Aidenwu0209/Unlimited-OCR-Skill.git
openclaw skills install \
  ./Unlimited-OCR-Skill/skills/unlimited-ocr-document-parsing \
  --as unlimited-ocr-document-parsing
```

也可以安装已发布的 ClawHub 版本：

```bash
openclaw skills install @Aidenwu0209/unlimited-ocr-document-parsing
```

该目录已经包含 ClawHub 运行依赖和环境变量元数据，并能通过注册表的包检查。

### Claude Code Marketplace

```bash
claude plugin marketplace add Aidenwu0209/Unlimited-OCR-Skill
claude plugin install unlimited-ocr-skill@aidenwu-ocr-skills
```

### 手动安装 Agent Skill

```bash
git clone https://github.com/Aidenwu0209/Unlimited-OCR-Skill.git
cp -R Unlimited-OCR-Skill/skills/unlimited-ocr-document-parsing ~/.agents/skills/
```

脚本使用 [uv](https://docs.astral.sh/uv/)，Python 依赖已写入脚本。各平台的识别、安装与发布状态见 [DISTRIBUTION.md](DISTRIBUTION.md)。

## 配置

### 百度智能云 API

先创建 OCR 应用，再设置：

```bash
export UNLIMITED_OCR_PROVIDER=baidu
export UNLIMITED_OCR_API_KEY="your-api-key"
export UNLIMITED_OCR_SECRET_KEY="your-secret-key"
```

如果已有 OAuth Access Token，可只设置 `UNLIMITED_OCR_ACCESS_TOKEN`。

### 本地 SGLang / OpenAI-compatible 服务

```bash
export UNLIMITED_OCR_PROVIDER=local
export UNLIMITED_OCR_LOCAL_BASE_URL="http://127.0.0.1:10000"
export UNLIMITED_OCR_LOCAL_BACKEND=sglang
export UNLIMITED_OCR_MODEL="Unlimited-OCR"
```

只允许 HTTPS 服务地址和回环 HTTP 地址；远程自建服务必须使用 HTTPS。

## 使用

```bash
uv run skills/unlimited-ocr-document-parsing/scripts/unlimited_ocr_caller.py \
  --file-path ./document.pdf --pretty --stdout

uv run skills/unlimited-ocr-document-parsing/scripts/unlimited_ocr_caller.py \
  --provider baidu --file-url https://example.com/report.pdf
```

## 验证

```bash
uv run skills/unlimited-ocr-document-parsing/scripts/smoke_test.py
python3 -m compileall -q skills
```

远程 API 会上传所选文档。OCR 返回内容是不可信文档数据，不能当作 Agent 指令执行。

来源与改造说明见 [UPSTREAM.md](UPSTREAM.md)。仓库主体使用 [Apache-2.0](LICENSE)，可独立分发的 Skill 子目录为兼容 ClawHub 使用 [MIT-0](skills/unlimited-ocr-document-parsing/LICENSE)。
