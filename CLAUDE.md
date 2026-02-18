# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

@AGENTS.md

## Development Workflow

### Requirements
- Node.js 24+
- pnpm 10.22+ (enable via `corepack enable && corepack prepare pnpm@10.22.0 --activate`)

### Running n8n Locally

```bash
pnpm install
pnpm build > build.log 2>&1   # Always redirect build output
pnpm start                     # Run n8n at http://localhost:5678
```

### Dev Mode (Hot Reload)

Most common setup — run backend and frontend in separate terminals:

```bash
# Terminal 1: Backend
pushd packages/cli && pnpm dev

# Terminal 2: Frontend
pushd packages/frontend/editor-ui && pnpm dev
```

Filtered dev commands:
- `pnpm dev` — Full stack (resource-intensive)
- `pnpm dev:be` — Backend only (excludes frontend)
- `pnpm dev:fe` — Frontend only (editor-ui + design-system)
- `pnpm dev:ai` — AI/LangChain nodes only

For node development with hot reload:
```bash
N8N_DEV_RELOAD=true pnpm dev   # in packages/cli
```

Clean database: `N8N_USER_FOLDER=~/.n8n-clean/ pnpm dev`

### Local Container Testing

```bash
pnpm build:docker                              # Build Docker image
pnpm --filter n8n-containers stack:sqlite      # SQLite stack
pnpm --filter n8n-containers stack:postgres    # Postgres stack
pnpm --filter n8n-containers stack:queue       # Queue mode
pnpm --filter n8n-containers stack:multi-main  # Multi-main with load balancer
```

## PR Title Format

Format: `<type>(<scope>): <Summary>` — follows Angular commit convention.

**Types:** `feat` | `fix` | `perf` | `test` | `docs` | `refactor` | `build` | `ci` | `chore`

**Scopes:** `API` | `core` | `editor` | `* Node` (e.g., `Slack Node`)

- Summary: imperative present tense, capitalized, no period, no ticket IDs
- Append `(no-changelog)` to exclude from changelog
- Breaking changes: add `!` before `:` (e.g., `feat(editor)!: Remove dark mode`)
- Add breaking changes to `packages/cli/BREAKING-CHANGES.md`
