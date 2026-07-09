---
name: ai-agent-fullstack-dev
version: 2.0.0
description: |
  Cross-platform, model-agnostic workflow for building a complete commercial AI Agent
  product from zero. Covers 7 phases: Init (scaffold), Core (Tools / Memory / Context /
  Permission), Brain (LLM + Query Engine), Team (multi-agent orchestration, Sessions,
  Command), Build (web / tools / content / image), Ship (deploy & ops), Loop (feedback &
  self-improvement). Maps directly onto the "10 core capabilities" architecture:
  Tools + Skills + Memory + Query Engine + Context + Permission + Sub-agent + Sessions +
  Command + Hook.

  Trigger when the user wants to "build an AI agent from scratch", "create a commercial AI
  project", "develop an agent-based product", "set up an agent dev toolchain", or any
  intent to go from 0 to a shippable AI product.
author: WorkBuddy AI (refactored for cross-platform use)
license: MIT
platforms:
  - claude-code
  - codex
  - openclaw
  - hermes
  - workbuddy
  - cursor
  - gemini-cli
homepage: https://github.com/your-org/ai-agent-fullstack-dev
tags: [ai-agent, fullstack, development, workflow, langgraph, typescript, commercial, cross-platform, toolchain]
---

# AI Agent Full-Stack Development Skill (Cross-Platform)

> **From zero to a shippable commercial AI Agent — one portable workflow, any model.**

## Overview

This skill encodes the complete process of building a commercial AI Agent product from
scratch, implementing the "10 core capabilities" architecture in full:

```
Tools + Skills + Memory + Query Engine + Context + Permission
    + Sub-agent + Sessions + Command + Hook
```

It is **model-agnostic and platform-agnostic**: the workflow, the 7 phases, and the
reference code are generic. Platform-specific tools (image generation, file ops, search)
are referenced as **capability tokens** and mapped per platform (see below). Drop this
folder into your agent's skill directory and it just works.

**How it is invoked**: when the user asks to build an AI agent / commercial AI product
from scratch, load this skill and run the phases in order. Natural-language triggers:
"build an AI agent from scratch", "create a commercial AI project", "develop an agent
product", "set up an agent dev toolchain".

**Prerequisites** (reference stack — swap freely):
- Node.js >= 18, pnpm >= 8, TypeScript >= 5
- An LLM with an OpenAI-compatible API (DeepSeek / OpenAI / Kimi / any)
- Git
- (optional) Any MCP servers your platform supports for search / browser / memory

---

## Platform Compatibility & Token Mapping

This skill runs on any agent that can read a `SKILL.md`. Capability references in the
body use the tokens below; map them to your platform's actual tools.

### Install locations

| Platform | Skill directory (drop this folder here) |
|----------|------------------------------------------|
| Claude Code | `~/.claude/skills/ai-agent-fullstack-dev/` |
| Codex | `~/.codex/skills/ai-agent-fullstack-dev/` (or project `.codex/skills/`) |
| OpenClaw | `~/.openclaw/skills/ai-agent-fullstack-dev/` |
| Hermes | `<hermes_skill_dir>/ai-agent-fullstack-dev/` |
| WorkBuddy | `~/.workbuddy/skills/ai-agent-fullstack-dev/` |
| Cursor | `.cursor/skills/ai-agent-fullstack-dev/` |
| Gemini CLI | `~/.gemini/skills/ai-agent-fullstack-dev/` |

> Any path written as `<SKILL_DIR>` below means "the directory containing this SKILL.md".

### Capability token mapping

| Token | Meaning | WorkBuddy | Claude | Codex | OpenClaw | Generic |
|-------|---------|-----------|--------|-------|----------|---------|
| `<IMAGE_GEN>` | text-to-image | ImageGen | image tool / API | DALL·E / API | image tool | any T2I API |
| `<SEARCH>` | web search | tavily / brave MCP | web tool | web tool | search tool | search API |
| `<FILE_OPS>` | file read/write | filesystem MCP | file tools | file tools | file tools | FS access |
| `<BROWSER>` | browser automation | puppeteer MCP | computer use | browser tool | browser tool | browser API |
| `<MEMORY>` | persistent memory | sqlite / memory MCP | memory | memory | memory | store/DB |
| `<REASON>` | structured reasoning | sequential-thinking MCP | extended thinking | reasoning | reasoning | any |

### Companion skills (optional, cross-platform)

- **`ai-image-prompts-skill`** — image prompt enhancer with a 10,000+ community library.
  Listed in `platforms` for claude-code / codex / openclaw / hermes / workbuddy / cursor /
  gemini-cli. Used in Phase 5.5. Install it alongside this skill for professional visuals.

### Ecosystem skill substitution

Phase 5 references concrete content-creation skills **by their WorkBuddy names**
(e.g. `pptx-generator`, `frontend-design`, `ui-ux-pro-max`). On other platforms, substitute
the equivalent in your skill registry — the workflow logic is identical. Treat them as
examples of *what capability is needed at that step*, not hard requirements.

---

## Phase 1: Init — Project Scaffold

### Goal
Create a runnable project skeleton: standard directory layout, config files, convention docs.

### Inputs
| Param | Type | Required | Notes |
|-------|------|----------|-------|
| projectName | string | yes | kebab-case |
| description | string | yes | one-line goal |
| targetDir | string | no | default: current dir |

### Steps

#### 1.1 Directory structure
```
project-root/
├── src/
│   ├── agent/           # Agent core
│   │   ├── core/        # Tools / Memory / Context
│   │   ├── brain/       # LLM / QueryEngine
│   │   ├── team/        # Sub-Agent orchestration
│   │   └── index.ts     # Agent entry
│   ├── server/          # API server (Fastify/Hono)
│   ├── web/             # Frontend (Next.js)
│   ├── skills/          # Custom skills
│   ├── tools/           # Custom tools
│   └── utils/           # Utilities
├── data/                # SQLite / local data
├── docs/                # Docs
├── tests/               # Tests
├── package.json
├── tsconfig.json
├── .env.example
├── AGENTS.md            # Agent conventions
└── MEMORY.md            # Project memory
```

#### 1.2 Init config
```bash
pnpm init
pnpm add -D typescript @types/node tsx nodemon eslint prettier
pnpm add @langchain/langgraph @langchain/core @langchain/community
pnpm add zod better-sqlite3 commander
npx tsc --init
git init
```

#### 1.3 .env.example
```env
# LLM (any OpenAI-compatible endpoint)
LLM_API_KEY=your_key_here
LLM_BASE_URL=https://api.deepseek.com/v1
LLM_MODEL=deepseek-chat

# Optional integrations
GITHUB_TOKEN=
SEARCH_API_KEY=
PORT=3000
NODE_ENV=development
```

#### 1.4 AGENTS.md template
```markdown
# Project Agents
## Tech Stack
- Runtime: Node.js + TypeScript
- Framework: LangGraph (JS SDK)
- LLM: OpenAI-compatible (DeepSeek default)
- Database: SQLite (better-sqlite3)
- Web: Next.js (App Router)
## Conventions
- Functional patterns over classes
- Strict TypeScript (no `any`)
- Zod for schema validation
- All tools must have error handling + timeout
## Commands
- `pnpm dev`: dev server
- `pnpm build`: type-check + build
- `pnpm test`: tests
- `pnpm agent:chat`: agent REPL
```

### Output
- ✅ Runnable skeleton, `pnpm dev` starts
- ✅ Git initialized

### Sub-tools
- file-management capability (`<FILE_OPS>`)
- repo management (e.g. `github` MCP / your platform's git tool) — optional
- `skill-creator` — for later custom skills

---

## Phase 2: Core — Agent Runtime

### Goal
Implement the runtime core: Tools layer, Memory system, Context management, Permission control.

### Prerequisite
Phase 1 complete.

### Steps

#### 2.1 Tools layer
Six base tools (reference implementation):
```typescript
// src/tools/index.ts
import { tool } from '@langchain/core/tools';
import { z } from 'zod';

// 1. read_file — safe file read (path-traversal guarded, size-limited)
// 2. write_file — validated write
// 3. exec — sandboxed command (allowlist only)
// 4. run_python — Python script execution
// 5. web_search — web search (<SEARCH>)
// 6. web_fetch — URL content fetch
```
**Security rules**: restrict file ops to project dir; exec allowlist only; network calls
must have timeout + size limit.

#### 2.2 Memory system (3 layers)
```typescript
// src/memory/index.ts
interface MemorySystem {
  shortTerm: ConversationMemory;     // in-memory context
  longTerm: PersistentMemory;        // SQLite FTS / your <MEMORY>
  profile: UserProfileMemory;        // user preference JSON
}
```

#### 2.3 Context pipeline (4-level compression)
```
raw system prompt → micro-compact → tool-compact → auto-compact → token_budget met
```

#### 2.4 Permission system
```typescript
enum PermissionLevel { SAFE, RESTRICTED, DANGER }
interface PermissionRule { pattern: RegExp; level: PermissionLevel; humanConfirm?: boolean }
```
Defaults: in-project file rw = SAFE; out-of-project path = DANGER; rm/del/format = DANGER;
install = RESTRICTED; network = RESTRICTED.

### Output
- ✅ 6 base tools callable
- ✅ Memory read/write works
- ✅ Context auto-compaction active
- ✅ Permission blocks dangerous ops

### Sub-tools
- `<FILE_OPS>` capability
- `<REASON>` capability (structured reasoning)
- permission guard, memory reference impl

---

## Phase 3: Brain — LLM & Query Engine

### Goal
Connect an OpenAI-compatible LLM; implement the full Query Engine
(API / stream / cache / error / retry / rate-limit).

### Prerequisite
Phase 2 complete; needs `LLM_API_KEY`.

### Steps

#### 3.1 LLM connection
```typescript
// src/brain/llm.ts
import { ChatOpenAI } from '@langchain/openai'; // works for any OpenAI-compatible endpoint

const llm = new ChatOpenAI({
  apiKey: process.env.LLM_API_KEY,
  configuration: { baseURL: process.env.LLM_BASE_URL },
  model: process.env.LLM_MODEL || 'deepseek-chat',
  temperature: 0.7,
  maxTokens: 4096,
  streaming: true,
});
```

#### 3.2 Query Engine (6 capabilities)
```typescript
interface QueryEngine {
  query(messages): Promise<QueryResult>;     // API
  stream(messages): AsyncIterable<Chunk>;     // SSE stream
  cache: SemanticCache;                       // semantic cache
  errorHandler: ErrorHandler;                 // classified errors
  retry(config): RetryWrapper;                // exponential backoff
  rateLimiter: RateLimiter;                   // token bucket
  tokenTracker: TokenTracker;                 // usage tracking
}
```

#### 3.3 Prompt template system
```typescript
const SYSTEM_PROMPTS = {
  agent: `You are a professional AI Agent.\n{{tools}}\n{{memory_context}}\n{{permission_rules}}\nThink before acting; safety first; answer concisely.`,
  researcher: `You are a deep researcher. Use search, then structured report with sources.`,
  coder: `You are a senior engineer. TypeScript expert. Write testable, maintainable code.`,
};
```

#### 3.4 Enhancement via search/reason
- `<SEARCH>` — web search
- `<REASON>` — structured reasoning for hard problems

### Output
- ✅ LLM conversational
- ✅ Streaming works
- ✅ Cache hit < 100ms
- ✅ Auto-retry on error
- ✅ Token usage tracked

### Sub-tools
- `<SEARCH>`, `<REASON>`
- prompt-optimizer (your platform's equivalent)

---

## Phase 4: Team — Multi-Agent Orchestration

### Goal
Design Sub-Agent architecture; implement Sessions, Command system, Hooks.

### Prerequisite
Phase 3 complete.

### Steps

#### 4.1 Sub-Agent topology
```
Coordinator
├── Researcher Agent   — research & gathering
├── Coder Agent        — code & debug
├── Writer Agent       — content (docs/PPT/articles)
├── Reviewer Agent     — QA & testing
└── Deployer Agent     — deploy & ops
```

#### 4.2 LangGraph state
```typescript
// src/team/graph.ts
const workflow = new StateGraph(AgentStateAnnotation)
  .addNode('coordinator', coordinatorNode)
  .addNode('researcher', researcherNode)
  .addNode('coder', coderNode)
  .addNode('writer', writerNode)
  .addNode('reviewer', reviewerNode)
  .addEdge(START, 'coordinator')
  .addConditionalEdges('coordinator', routeByIntent,
    { researcher: 'researcher', coder: 'coder', writer: 'writer' })
  .compile();
```

#### 4.3 Sessions
```typescript
interface SessionManager {
  create(meta): Session;
  list(filters?): Session[];
  rewind(sessionId, step): Session;   // resume from history point
  export(sessionId): SessionExport;
}
```

#### 4.4 Command system
```
/help /status /clear /switch <agent> /history /export
system: agent:list tool:list memory:stats session:create config:set
```

### Output
- ✅ Multi-agent graph runs
- ✅ Roles clear
- ✅ Session persistence
- ✅ Rewind / resume works

### Sub-tools
- multi-agent orchestration engine (your platform's equivalent)
- team management capability

---

## Phase 5: Build — Product Production

### Goal
Use the agent team to produce real product: website, tools, content, files.

### Prerequisite
Phase 4 complete.

### 5.1 Website (Web Frontend)
Next.js 14+ App Router + Tailwind + shadcn/ui.
Flow: design system → style → components → Coder implements → Reviewer audits.
Pages: home, chat UI, knowledge base (RAG), learning center, user settings.

### 5.2 Tool modules
- KB import/export, rich-text/Markdown editor, data dashboard, API-key manager.

### 5.3 Content production
Use your platform's content skills: PPT, SEO articles, docs, video scripts.

### 5.4 File output
PDF report, Excel export, Markdown doc, ZIP asset bundle.

### 5.5 Image Prompt Enhancer ⚡ (optional companion)

**Purpose**: before any image deliverable, retrieve a community-validated prompt via
`ai-image-prompts-skill` instead of hand-writing one — sharply raising visual quality.

**Proactive trigger**: if the user request contains any of
`image / cover / poster / avatar / product shot / thumbnail / marketing visual / illustration /
logo / banner / photo / hero image`, **call `ai-image-prompts-skill` first**.

**Flow**:
```
1. call ai-image-prompts-skill → retrieve matched template (with reference image)
2. user picks style → remix to customize the English prompt for the content
3. generate final image via <IMAGE_GEN>
4. deliver image + prompt used + source attribution
```

### Output
- ✅ Accessible site (localhost:3000)
- ✅ >= 5 core modules
- ✅ >= 10 content assets
- ✅ Product PPT presentable
- ✅ Image deliverables based on pro prompts (not ad-hoc)

### Sub-tools
- content-creation skills (PPT / article / doc / video — your platform's equivalents)
- frontend-design, ui-ux equivalents
- **`ai-image-prompts-skill`** — image prompt enhancer (10k+ library)

---

## Phase 6: Ship — Deploy & Ops

### Goal
Deploy to production; configure monitoring & automation.

### Prerequisite
Phase 5 complete.

### Steps

#### 6.1 Docker
```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package.json pnpm-lock.yaml ./
RUN corepack enable && pnpm install --frozen-lockfile
COPY . .
RUN pnpm build
FROM node:20-alpine AS runner
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

#### 6.2 CI/CD
Push to main → auto build/deploy; PR → tests + lint. (Use your platform's repo tool / MCP.)

#### 6.3 Hook system
```typescript
interface HookSystem {
  preTool(name, args): Promise<HookResult>;
  postTool(name, result): Promise<HookResult>;
}
// logging, cost tracking, security audit, cache invalidation
```

#### 6.4 Monitoring
Error (Sentry / Grafana), performance (latency, success rate), cost (token/day, budget alert).

### Output
- ✅ Docker image builds
- ✅ CI/CD runs
- ✅ Hooks intercept key events
- ✅ Monitoring dashboard

### Sub-tools
- repo & actions tool (e.g. `github` MCP)
- container / monitoring MCP — optional

---

## Phase 7: Loop — Feedback & Self-Improvement

### Goal
Close the loop; continuously improve agent & product quality.

### Steps
- 7.1 Collect feedback (ratings, logs, error/success rate, token cost)
- 7.2 Self-reflection: review records, root-cause failures, generate improvements
- 7.3 Skill lifecycle: evaluate usage, retire weak skills, discover/install new, bump versions
- 7.4 Iterate (2-week cadence): collect → analyze → goal → improve → verify → ship

### Sub-tools
- self-improvement capability
- reflection / learning capability
- skill-lifecycle manager (your platform's equivalent)

---

## Quick-Start Checklist

### MVP (minimum viable loop)
- [ ] Phase 1: scaffold (30 min)
- [ ] Phase 2.1: read_file + write_file + web_search (1 hr)
- [ ] Phase 2.2: SQLite memory basic (1 hr)
- [ ] Phase 3.1: LLM connect + simple chat (30 min)
- [ ] Phase 4.1: single agent minimal (Coordinator + Coder) (2 hr)
- [ ] Verify: agent can read/write files + search + write code via natural language

### Full product
- [ ] MVP done
- [ ] Phase 2.3–2.4: Context + Permission (2 hr)
- [ ] Phase 3.2–3.4: Query Engine full (2 hr)
- [ ] Phase 4.2–4.4: multi-agent + Sessions + Commands (4 hr)
- [ ] Phase 5: web + content (as needed)
- [ ] Phase 6: deploy (as needed)
- [ ] Phase 7: feedback loop (ongoing)

---

## Capability → Tool Mapping (reference ecosystem)

| Architecture module | Capability token | Reference impl (MCP / tool) |
|---------------------|------------------|------------------------------|
| Tools / filesystem | `<FILE_OPS>` | filesystem MCP |
| Tools / search | `<SEARCH>` | tavily / brave MCP |
| Tools / browser | `<BROWSER>` | puppeteer MCP |
| Tools / repo | repo tool | github MCP |
| Memory / graph | `<MEMORY>` | memory MCP (knowledge graph) |
| Memory / store | `<MEMORY>` | sqlite MCP |
| Brain / reasoning | `<REASON>` | sequential-thinking MCP |
| Build / image prompt | companion | `ai-image-prompts-skill` |
| Build / image gen | `<IMAGE_GEN>` | ImageGen (or your T2I) |

> On non-MCP platforms, replace each MCP with the platform-native equivalent; the module
> it fulfills is what matters, not the specific package.

---

## Troubleshooting

| Issue | Possible cause | Fix |
|-------|----------------|-----|
| LLM no response | invalid/empty API key or quota | check `LLM_API_KEY` in `.env` |
| Memory fails | data dir missing | ensure `data/` exists & writable |
| Tool/MCP connect fail | package not fetched / network | pre-run `npx -y <pkg>` to test |
| Agent loops | missing terminal condition | check graph conditional edges |
| Token overflow | context not compacted | enable auto-compact |
| Permission too strict | over-conservative rules | relax allowlist |

---

## Version History

| Version | Date | Change |
|---------|------|--------|
| 1.0.0 | 2026-07-09 | Initial 7-phase workflow (WorkBuddy) |
| 1.1.0 | 2026-07-09 | Integrated `ai-image-prompts-skill` (Phase 5.5) |
| 2.0.0 | 2026-07-09 | **Cross-platform refactor**: model/agent-agnostic, `platforms` + token mapping, per-platform install docs, MIT licensed |
