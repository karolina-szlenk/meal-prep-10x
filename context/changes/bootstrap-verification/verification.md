---
bootstrapped_at: 2026-09-27T15:24:29Z
starter_id: 10x-astro-starter
starter_name: "10x Astro Starter (Astro + Supabase + Cloudflare)"
project_name: meal-prep
language_family: js
package_manager: npm
cwd_strategy: git-clone
bootstrapper_confidence: first-class
phase_3_status: ok
audit_command: "npm audit --json"
---

## Hand-off

```yaml
starter_id: 10x-astro-starter
package_manager: npm
project_name: meal-prep
hints:
  language_family: js
  team_size: solo
  deployment_target: cloudflare-pages
  ci_provider: github-actions
  ci_default_flow: auto-deploy-on-merge
  bootstrapper_confidence: first-class
  path_taken: standard
  quality_override: false
  self_check_answers: null
  has_auth: true
  has_payments: false
  has_realtime: false
  has_ai: true
  has_background_jobs: false
```

### Why this stack

A solo, after-hours build with a 6-week MVP window and a hard deadline needs a starter that clears all four agent-friendly gates and ships auth, a database and deployment out of the box. 10x Astro Starter is the recommended default for a JS/TS web app: Supabase covers email+password login and, through row-level security, the requirement that users never see each other's targets, calendars or saved meals, while Postgres holds the CSV-sourced product base, admin-managed products and saved meals. Astro with React islands handles the interactive weekly calendar in the browser on both laptop and phone. Meal generation from a free-text wish is treated as an LLM call behind an API route, with kcal and macros always recomputed and checked server-side against product data, so the AI flag is set. The 10-second generation limit fits within an edge request. Payments, realtime and background jobs are out of scope. Deployment goes to Cloudflare Pages via GitHub Actions with auto-deploy on merge, which is the starter's default setup.

## Pre-scaffold verification

| Signal      | Value                                                   | Severity | Notes                                                                                   |
| ----------- | ------------------------------------------------------- | -------- | --------------------------------------------------------------------------------------- |
| npm package | not run                                                 | —        | cmd_template starts with `git clone`; no `create-*` CLI to check                        |
| GitHub repo | przeprogramowani/10x-astro-starter last pushed 2026-09-12 | fresh    | from card.docs_url; `gh` CLI not installed, fetched via public GitHub REST API (curl)   |

## Scaffold log

**Resolved invocation**: `git clone https://github.com/przeprogramowani/10x-astro-starter .bootstrap-scaffold && cd .bootstrap-scaffold && npm install`
**Strategy**: git-clone
**Exit code**: 0
**Files moved**: 20 top-level entries (.env.example, .github, .husky, .nvmrc, .prettierrc.json, .vscode, AGENTS.md, README.md, astro.config.mjs, components.json, eslint.config.js, node_modules, package-lock.json, package.json, public, scripts, src, supabase, tsconfig.json, wrangler.jsonc)
**Conflicts (.scaffold siblings)**: CLAUDE.md → CLAUDE.md.scaffold
**.gitignore handling**: moved silently (absent in cwd)
**.bootstrap-scaffold cleanup**: deleted (cloned `.git/` removed before move-up; cwd `.git/` untouched)

**Environment notes**:

- `node`/`npm` were not on the non-interactive shell's PATH; the run used nvm's Node v22.18.0 (card requires "node 22").
- `npm install` emitted EBADENGINE warnings: `eslint-plugin-astro@3.1.0` requires Node `^22.22.3 || ^24.16.0 || >=26.3.0`, `undici@8.10.2` requires `>=22.19.0`. Install still succeeded.
- The starter's `.nvmrc` pins `22.14.0`, which is below those engine ranges.
- Husky's `prepare` hook ran inside the temporary clone (whose `.git/` was then deleted), so git hooks are not yet wired into this repository.

## Post-scaffold audit

**Tool**: npm audit --json
**Summary**: 0 CRITICAL, 0 HIGH, 0 MODERATE, 0 LOW (0 INFO)
**Direct vs transitive**: 0/0/0/0 direct of total 0/0/0/0 (804 dependencies audited: 377 prod, 269 dev, 167 optional)

#### CRITICAL findings

None.

#### HIGH findings

None.

#### MODERATE findings

None.

#### LOW / INFO findings

None.

## Hints recorded but not acted on

| Hint                    | Value                |
| ----------------------- | -------------------- |
| bootstrapper_confidence | first-class          |
| quality_override        | false                |
| path_taken              | standard             |
| self_check_answers      | null                 |
| team_size               | solo                 |
| deployment_target       | cloudflare-pages     |
| ci_provider             | github-actions       |
| ci_default_flow         | auto-deploy-on-merge |
| has_auth                | true                 |
| has_payments            | false                |
| has_realtime            | false                |
| has_ai                  | true                 |
| has_background_jobs     | false                |

## Next steps

Next: a future skill will set up agent context (CLAUDE.md, AGENTS.md). For now, your project is scaffolded and verified — happy hacking.

Useful manual steps in the meantime:
- `git init` (if you have not already) to start your own repo history.
- Review any `.scaffold` siblings the conflict policy created and decide which version of each file to keep.
- Address audit findings per your project's risk tolerance — the full breakdown is in this log.
