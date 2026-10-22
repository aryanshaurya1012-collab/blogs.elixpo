# Agent Guidelines for blogs.elixpo

Real-time collaborative blogging platform

<!-- TODO: Fill in the sections below with repo-specific details. The stub
     below is a template; keep what applies, remove what doesn't. -->

## Architecture

<!-- TODO: Describe the runtime, language, framework, key services, hosting. -->
- **Runtime**: <TODO>
- **Language**: <TODO>
- **Hosting**: <TODO>

## Repository Structure

<!-- TODO: One-line per top-level dir describing its purpose. -->

## Hard Constraints

<!-- TODO: List constraints that would break the build / tests / deploy if
     violated. Example from accounts.elixpo: "every API route must export
     `export const runtime = 'edge'`". -->

## Git & PR Workflow

- **Never commit to `main`.** It's branch-protected; use a feature branch.
- Branch naming: `elixpo/<issue-n>-<hex>` for agent-driven changes; `feat/<slug>` / `fix/<slug>` for manual.
- Commit format: conventional — `feat:`, `fix:`, `refactor:`, `docs:`, `chore:`, `ci:`.
- PR title: `[ELIXPO] <short>` for agent PRs.
- PR body ends with `Fixes #N` so GitHub auto-closes on merge.

## Communication Style

- Bullets over paragraphs. <200 words per PR body / comment unless the change genuinely needs more.
- Facts, not opinions. Link specific files/lines rather than describing.
- No marketing language ("seamlessly", "robust", "leverages").
- No hedging ("I think", "maybe").

## Agent Voice

- Never say "Claude", "Claude Code", "AI", "LLM", "analyzing".
- Speak as a teammate: "looking into this", "pushed a fix", "opened #N".

## Workflow Orchestration (for agents)

- Read `.elixpo-context/context.md` ONCE at the start if injected into the prompt; otherwise use `Glob`/`Grep` to locate files directly. Don't `ls -R`.
- For issue work, follow `.claude/commands/respond-to-issue.md` — question vs implement vs decline.
- For commit / push / PR, follow `.claude/commands/commit-push-pr.md`.
- For mechanical bulk refactors (>3 files, renames, string migrations), use `python .github/scripts/apply_refactor.py` with a JSON plan on stdin — one deterministic call beats N Read+Edit roundtrips.

## Common Mistakes (fill in as you discover them)

<!-- TODO: Track mistakes contributors / agents have actually made in this
     repo so future passes avoid them. Keep concrete and specific. -->

- Do not create `.dev.vars`. Local Next.js refuses to start when that file exists. `.env.local` is the only local secret file.

## Cursor Cloud specific instructions

- Node.js 22. Install with `npm ci`. The quality gate is `npm test`, then `npm run pages:build`.
- `npm run dev` needs `.env.local`. `NEXT_PUBLIC_URL=http://localhost:3000` and a generated `SESSION_SECRET` are enough to boot the public site and local D1. OAuth, Cloudinary, and remote Cloudflare credentials are not required for that.
- Apply the local D1 schema before feed or explore queries: `npx wrangler d1 migrations apply lixblogs --local`. The dev server is http://localhost:3000.
- This environment also checks out LinkShield and Gully Games. LinkShield: `npm ci`, then `npm run dev -- --host 0.0.0.0 --port 5173`. Gully Games: `npm install --legacy-peer-deps` (Vite 8 conflicts with the pinned esbuild range), then `PORT=3001 npm run dev`. Sample activities run without `GEMINI_API_KEY`.

@RTK.md
