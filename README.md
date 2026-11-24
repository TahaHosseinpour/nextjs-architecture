# nextjs-architecture

A [Claude Code](https://claude.com/claude-code) skill that packages a complete,
opinionated **feature-based full-stack Next.js architecture** — so a new project
comes out in this shape just by invoking the skill, and an existing one can be
migrated onto it incrementally.

Built for Next.js 16+ (App Router, `cacheComponents`), React 19, Prisma, Zod and
TypeScript. The UI stack is deliberately left open — the skill never installs a
component library by default.

## What the architecture looks like

- **`src/server/`** — infrastructure, written once: validated env, Prisma
  singleton, `AppError` hierarchy, pino logging with request context, audit
  events, JWT session + auth guards, `defineRoute`/`defineAction` factories,
  named rate limits, typed cache tags, health check.
- **`src/features/<name>/`** — one folder per business domain: `dal.ts` (all
  data access, identity from the session only), `actions.ts`, `inputs.ts`,
  `types.ts`, server/client barrels, `system.ts`.
- **`src/app/`** — URL shells only; no business logic in routes or pages.
- **ESLint guards** enforce the dependency rules between layers and features,
  so the architecture is checked by tooling rather than by memory.

## Repository layout

```
SKILL.md          # the skill itself: recipes, decision tables, triggers
references/       # the reasoning: architecture, security, data & caching,
                  # observability, ESLint guards, adoption/migration guide, …
templates/        # working generic code for every piece: src/server/*,
                  # an example `posts` feature, eslint.config.mjs,
                  # next.config.ts, AGENTS.md / CLAUDE.md starters, CI
```

## Install

Clone into your user-level skills directory:

```bash
git clone https://github.com/TahaHosseinpour/nextjs-architecture.git \
  ~/.claude/skills/nextjs-architecture
```

Or copy the folder into a single project's `.claude/skills/nextjs-architecture/`
to scope it to that project.

Claude Code picks it up automatically. It triggers when you start a new Next.js
project, migrate an existing one, or add a feature, page, route, Server Action,
query, cache rule, rate limit or env var to a project built this way.

## How it behaves

- **New project** — follows `references/adoption.md` phase by phase, copying
  from `templates/`, and asks only for genuinely open decisions (feature list,
  admin panel, language, auth method, UI stack, money unit, timezone).
- **Existing project on this architecture** — reads the project's `AGENTS.md`
  and `docs/` first; **project docs override the skill** where they differ.
- **Existing project on another structure** — additive infrastructure first,
  guards with a shrinking legacy list, one reference feature, then
  feature-by-feature PRs. Never a big-bang rewrite.
- **Not a Next.js project** — carries over only the stack-independent
  principles instead of copying templates.

## License

No license file yet — all rights reserved by the author until one is added.