# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Meal-prep planner (weekly calendar of meals that fit per-day/per-meal kcal and macro limits, built from Lidl/Biedronka products, with LLM-generated meals). Built on the 10x Astro Starter. Product and stack decisions live in `@context/foundation/prd.md` (written in Polish) and `@context/foundation/tech-stack.md` — read them before planning features. Setup and env details: `@README.md`.

## Critical rules

- Server-computed nutrition only: kcal and macros for generated meals are always recomputed and checked server-side against product data — never trust LLM-returned numbers. Over-limit meals get a warning, not a block (PRD FR-006).
- Per-user data isolation is enforced by Supabase row-level security; every new user-owned table needs RLS policies.
- `createClient()` in `src/lib/supabase.ts` returns `null` when `SUPABASE_URL`/`SUPABASE_KEY` are unset (they are `optional` in the `astro:env` schema). Every caller must handle the `null` case — see `src/pages/api/auth/signin.ts`.
- Supabase env vars are server-only secrets read via `astro:env/server`; never import them into React islands or expose them to the client.
- Never write to `context/archive/` — archived changes are immutable. If a target path starts with `context/archive/`, abort with: "This change is archived. Open a new change with `/10x-new` instead."

## Commands

- `npm run dev` — dev server on the Cloudflare workerd runtime (http://localhost:4321)
- `npm run lint` / `npm run lint:fix` — ESLint with type-checked rules
- `npx astro sync && npx astro check` — type check (CI runs `astro sync` first so `astro:env` types exist)
- `npm run build` / `npm run preview`
- `npm run smoke` — HTTP walk-through of the auth flow against a running server (`BASE_URL`, default `http://localhost:4321`); needs reachable Supabase with email confirmation off
- `npx supabase start|stop` — local Supabase (Docker); Studio at http://localhost:54323

There is no unit/e2e test runner yet; the smoke script is the only automated check. Pre-commit (husky + lint-staged) runs `eslint --fix` on ts/tsx/astro and `prettier --write` on json/css/md.

## Architecture

Stack overview: `@README.md`. Local specifics:

- shadcn/ui (new-york) components in `src/components/ui`, `cn` helper in `src/lib/utils`. Import alias `@/*` → `src/*`.
- Auth: `src/middleware.ts` sets `context.locals.user` (typed in `src/env.d.ts`) from a per-request Supabase SSR client; read the user from `locals`, don't create a new client for it.
- API routes in `src/pages/api/**` handle HTML form posts and respond with `context.redirect(...)`, passing errors as `?error=<encoded message>` on the originating page — not JSON.
- Keep `.env` (Astro) and `.dev.vars` (local Cloudflare runtime) in sync; setup in `@README.md`.
