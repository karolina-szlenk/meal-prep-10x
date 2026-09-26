---
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
---

## Why this stack

A solo, after-hours build with a 6-week MVP window and a hard deadline needs a starter that clears all four agent-friendly gates and ships auth, a database and deployment out of the box. 10x Astro Starter is the recommended default for a JS/TS web app: Supabase covers email+password login and, through row-level security, the requirement that users never see each other's targets, calendars or saved meals, while Postgres holds the CSV-sourced product base, admin-managed products and saved meals. Astro with React islands handles the interactive weekly calendar in the browser on both laptop and phone. Meal generation from a free-text wish is treated as an LLM call behind an API route, with kcal and macros always recomputed and checked server-side against product data, so the AI flag is set. The 10-second generation limit fits within an edge request. Payments, realtime and background jobs are out of scope. Deployment goes to Cloudflare Pages via GitHub Actions with auto-deploy on merge, which is the starter's default setup.
