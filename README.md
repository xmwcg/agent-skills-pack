# agent-skills-pack

通用 AI Agent 全栈开发技能安装包（跨平台）。一句话或自然语言即可安装，供 Claude Code / Codex / OpenClaw / Hermes / WorkBuddy 等主流 agent 调用。

## 包含

- `ai-agent-fullstack-dev/` — 主技能：从 0 开发 AI Agent / 网站 / 工具 / 内容 / 文件应用的 **7 阶段商业闭环工作流**（跨平台 v2.0.0，Init→Core→Brain→Team→Build→Ship→Loop）
- `ai-image-prompts-skill/` — 图像提示词增强：14,717 条社区精选文生图模板检索，首次使用执行 `node scripts/setup.js --force` 拉取本地提示库

## 安装

把两个文件夹直接丢进对应 agent 的 `skills/` 目录即可，或装好后用自然语言触发（如"从零开发一个 AI Agent 商业项目"）。完整说明见 `INSTALL.md`。

## 支持平台

Claude Code · Codex · OpenClaw · Hermes · WorkBuddy · Cursor · Gemini CLI

## 目录结构

```
agent-skills-pack/
├── INSTALL.md
├── ai-agent-fullstack-dev/
│   └── SKILL.md
└── ai-image-prompts-skill/
    ├── SKILL.md
    └── scripts/setup.js
```
