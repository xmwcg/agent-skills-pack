# 通用 AI Agent 开发技能包 — 安装

两个跨平台技能，丢进任意 agent 的 skills 目录即可，无需改任何代码。

## 一句话安装（按你的平台选一行执行）

**Claude Code**
```bash
cp -r ai-agent-fullstack-dev ~/.claude/skills/ && cp -r ai-image-prompts-skill ~/.claude/skills/
```
**Codex**
```bash
cp -r ai-agent-fullstack-dev ~/.codex/skills/ && cp -r ai-image-prompts-skill ~/.codex/skills/
```
**OpenClaw**
```bash
openclaw skill install ./ai-agent-fullstack-dev && openclaw skill install ./ai-image-prompts-skill
```
**Hermes**
```bash
cp -r ai-agent-fullstack-dev <hermes_skill_dir>/ && cp -r ai-image-prompts-skill <hermes_skill_dir>/
```
**WorkBuddy**
```bash
cp -r ai-agent-fullstack-dev ~/.workbuddy/skills/ && cp -r ai-image-prompts-skill ~/.workbuddy/skills/
```

## 自然语言触发（装好直接对话，无需命令）

- "从零开发一个 AI Agent"
- "做一个商业 AI 项目"
- "搭建 agent 开发工具链"

涉及图像需求时，会自动调用 `ai-image-prompts-skill` 检索专业提示词。

## 首次使用 companion 的图像库（可选，一次性）

`ai-image-prompts-skill` 首次使用前拉取提示库（约 43MB，之后自动增量更新）：
```bash
node ai-image-prompts-skill/scripts/setup.js --force
```

## 包含的技能

| 技能 | 作用 |
|------|------|
| `ai-agent-fullstack-dev` | 7 阶段完整开发工作流（Init→Core→Brain→Team→Build→Ship→Loop） |
| `ai-image-prompts-skill` | 10,000+ 社区精选文生图提示词增强（可选 companion） |
