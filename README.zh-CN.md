# Unlimited-OCR-Skill

[English](README.md) | 简体中文

基于[百度 Unlimited-OCR](https://github.com/baidu/Unlimited-OCR) 的可移植 Agent Skill，用于长文档 OCR 和结构化 Markdown 解析。支持：

- **百度智能云 API**：通过官方异步接口处理图片、PDF、OFD、Office 文档、文本文件与公开 HTTPS URL。
- **本地服务**：通过 SGLang 或其他 OpenAI-compatible `/v1/chat/completions` 服务处理图片和 PDF。
- 返回完整 Markdown，同时保留可审计 JSON 包装。
- 包含超时、下载上限和结构化错误。

## 安装

把 `skills/unlimited-ocr-document-parsing` 复制到 Agent 运行时的 Skills 目录。例如 Codex-compatible 运行时：

```bash
git clone https://github.com/Aidenwu0209/Unlimited-OCR-Skill.git
cp -R Unlimited-OCR-Skill/skills/unlimited-ocr-document-parsing ~/.agents/skills/
```

脚本使用 [uv](https://docs.astral.sh/uv/)，Python 依赖已写入脚本。

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

来源与改造说明见 [UPSTREAM.md](UPSTREAM.md)，许可证见 [LICENSE](LICENSE)。

