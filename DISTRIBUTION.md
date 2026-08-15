# Distribution and compatibility

This repository keeps one canonical Agent Skill at
`skills/unlimited-ocr-document-parsing`. Platform-specific manifests only point
to that folder; they do not maintain separate OCR implementations.

| Ecosystem | Discovery/install surface | Status |
| --- | --- | --- |
| Agent Skills specification | `skills/unlimited-ocr-document-parsing/SKILL.md` | Native |
| skills.sh / `skills` CLI | `npx skills add Aidenwu0209/Unlimited-OCR-Skill --skill unlimited-ocr-document-parsing` | Discoverable from GitHub |
| Codex, Cursor, OpenCode and other `skills` clients | Install through the command above with `-a <agent>` | Portable Skill |
| OpenClaw | Install the cloned Skill directory with `openclaw skills install <path>` | Compatible |
| ClawHub | `openclaw skills install @Aidenwu0209/unlimited-ocr-document-parsing` | Registry release |
| Claude Code | Add this GitHub repository as a marketplace, then install `unlimited-ocr-skill@aidenwu-ocr-skills` | Self-hosted marketplace ready |
| DeepSeek Harness | Use the separate `Aidenwu0209/dsh-Unlimited-OCR-Skill` repository | Native GUI + Tool |

## Validation commands

```bash
uvx --from git+https://github.com/agentskills/agentskills#subdirectory=skills-ref \
  skills-ref validate ./skills/unlimited-ocr-document-parsing

npx skills add . --list

npx clawhub skill publish ./skills/unlimited-ocr-document-parsing \
  --slug unlimited-ocr-document-parsing \
  --name "Unlimited-OCR Document Parsing" \
  --version 1.1.0 --dry-run --json

claude plugin validate .
```

ClawHub distributes uploaded Skill bundles under MIT-0, so the independently
published Skill folder contains its own MIT-0 `LICENSE`. The surrounding
repository remains Apache-2.0.
