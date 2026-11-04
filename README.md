[EN](README.md) | [TH](README.th.md)

<h1 align="center">Project Prompt Builder</h1>

<p align="center">
  Visually select your project stack and generate a paste-ready AI coding prompt.
</p>

<p align="center">
  <a href="#overview">Overview</a> ·
  <a href="#features">Features</a> ·
  <a href="#tech-stack">Tech stack</a> ·
  <a href="#project-structure">Structure</a> ·
  <a href="#getting-started">Getting started</a> ·
  <a href="#environment-variables">Environment</a> ·
  <a href="#usage">Usage</a> ·
  <a href="#notes">Notes</a>
</p>

---

## Overview

Project Prompt Builder (PPB) is a single-page, fully client-side web app that turns technology choices and rough project notes into a coherent, deterministic Markdown brief you can paste into an AI coding agent such as Claude Code, Cursor, GitHub Copilot, or Codex.

The workflow is **Select → Configure → Generate → Copy**. A structured configurator feeds a rules engine that warns about known-fragile combinations and a template pipeline that always produces the same prompt for the same input.

There is **no LLM, no backend, and no database**. Everything runs in the browser and persists to `localStorage`.

## Features

- **21 technology categories** — 10 single-select and 11 multi-select, with search across names and descriptions.
- **12 named presets** — plus a blank/custom option. Applying a preset replaces the listed categories; you can tweak everything afterwards.
- **7 natural-language requirement fields** — project name, description, target users, main features, business requirements, technical requirements, and additional instructions.
- **3 prompt modes** — Basic, Standard, and Production.
- **Live prompt preview** — word count, character count, compatibility-note count, and an **Edited** badge after manual changes.
- **Warn-only compatibility notes** — explain the concern and link to the relevant category. Nothing blocks except the Database two-item cap.
- **Manual editing** — edit the generated prompt in place, save it, and reset to the generated version at any time.
- **Copy to clipboard** — with a hidden-textarea fallback for restrictive environments.
- **Share links** — opening a URL with a `?c=` parameter (base64url JSON) applies the full configuration, overriding stored state. Encoding exists in `lib/share/codec.ts`; there is no UI button for creating one yet.
- **Dark / light theme** — OS preference detection plus localStorage persistence.
- **Responsive layout** — two panes side by side on desktop, a tab switcher with a sticky copy bar on mobile.
- **Persistence** — selections, requirements, mode, and custom skills survive a refresh.
- **SEO/GEO** — metadata, Open Graph and Twitter cards, JSON-LD `SoftwareApplication`, `sitemap.xml`, `robots.txt`, and `public/llms.txt`.

### Prompt modes

| Mode | Contents |
| --- | --- |
| **Basic** | Role, project overview, technology stack with integration notes, requirements, and deliverables. |
| **Standard** | Adds architecture guidance, agent skills, development rules, code quality, testing, and deployment. |
| **Production** | Adds security, validation/error handling, logging/observability, CI/CD, and performance/scalability, plus a closing AI-agent instruction block. |

## Tech stack

| Area | Choice |
| --- | --- |
| Framework | [Next.js 16.4.0](https://nextjs.org) (App Router, Turbopack) · React 19.3.0 |
| Language | TypeScript 5 (strict, `noEmit`) |
| Styling | [Tailwind CSS](https://tailwindcss.com) v4 via `@tailwindcss/turbopack` |
| State | [Zustand](https://zustand-demo.pmnd.rs) 5 with the `persist` middleware |
| Icons | [`lucide-react`](https://lucide.dev) |
| UI utilities | `class-variance-authority`, `clsx`, `tailwind-merge` (`cn` helper) |
| Testing | [Vitest](https://vitest.dev) 5 + Testing Library + jsdom |
| Linting | ESLint 9 with `eslint-config-next` |
| Package manager | npm (`package-lock.json`) |

## Project structure

```
app/                    Next.js App Router: layout, page, robots, sitemap, globals.css
components/
  configurator/         preset bar, category sections, option cards, requirements form, custom skills
  prompt/               mode switch, validation list, prompt preview
  ui/                   Button, Card, ConfirmDialog primitives
  shared-config-loader.tsx
  theme-toggle.tsx
lib/
  data/                 catalog.ts (categories/options), presets.ts, logos.ts
  store/useConfig.ts    persisted Zustand configuration
  prompt/               builder.ts, stack-intel.ts, useGeneratedPrompt.ts
  compat/rules.ts       warn-only compatibility rule engine
  share/codec.ts        base64url encode/decode + sanitizer for `?c=`
  export/clipboard.ts   clipboard copy with fallback
  domain.ts selections.ts site.ts utils.ts
docs/
  adr/                  ADRs 0001–0004
  agents/               issue-tracker, triage-label, and domain docs
public/
  logos/                vendored brand SVGs
  llms.txt              guidance for AI assistants
scripts/fetch-logos.mjs re-vendors brand SVGs from svgl.app
.scratch/               local markdown issue tracker (specs and tickets)
CONTEXT.md              locked project vocabulary
AGENTS.md               agent instructions for this repo
```

## Getting started

Requirements: **Node.js >=22.23.4** (`.nvmrc` pins 24.22.1) and npm.

```bash
npm install
npm run dev
```

Open http://localhost:3000. No database or external service is required.

### Scripts

| Command | Description |
| --- | --- |
| `npm run dev` | Development server with Turbopack |
| `npm run build` | Production build |
| `npm run start` | Serve the production build |
| `npm run test` | Vitest suite (unit + component) |
| `npm run lint` | ESLint |
| `npm run typecheck` | `tsc --noEmit` |
| `npm run fetch-logos` | Refresh vendored technology logos from svgl.app |

## Environment variables

| Name | Required | Notes |
| --- | --- | --- |
| `NEXT_PUBLIC_SITE_URL` | Optional | Canonical site URL used for metadata, sitemap, and share links. Falls back to `https://project-prompt-builder.vercel.app` when unset. |

See `.env.example` for the template. Never commit real secret values.

## Usage

1. **Select a preset** — pick one of the 12 named presets or start from the blank/custom option.
2. **Choose technologies** — single-select categories behave like radio buttons; multi-select categories toggle. Database is capped at two options, and a second pick is flagged as a multi-database architecture.
3. **Fill requirements** — complete the seven fields. Custom agent skills are typed one per line; leading slashes are stripped.
4. **Choose a mode** — Basic, Standard, or Production controls the depth of the generated prompt.
5. **Review** — the preview rebuilds deterministically on every change. Compatibility notes explain potential issues and can jump to the relevant category.
6. **Copy or edit** — copy the prompt, or edit it manually and save. **Reset edits** returns to the generated version; **Clear** asks for confirmation.
7. **Share** — a `?c=` link applies a full configuration when opened. There is currently no UI control that creates one.

## Notes

- **Deterministic generation** — `buildPrompt(config)` in `lib/prompt/builder.ts` runs normalize → stack-intel notes → section builders → assemble by mode. It never calls an LLM.
- **Warn-only validation** — `evaluateRules(selections)` in `lib/compat/rules.ts` returns `ValidationIssue[]` for combinations such as `sqlc` with a non-SQL database, Vercel with a non-Node backend, or tRPC without a Node backend.
- **Storage keys** — the configuration uses `ppb-config-v1`; the theme uses `ppb-theme`.
- **Domain vocabulary** — fixed in [`CONTEXT.md`](CONTEXT.md); architecture decisions are in [`docs/adr/`](docs/adr/) (0001–0004).
- **Issue tracking** — a local markdown tracker under `.scratch/`; see [`docs/agents/`](docs/agents/).
- **Testing** — Vitest and Testing Library run in jsdom with `@testing-library/jest-dom`; test files live beside the code they cover (`*.test.ts` / `*.test.tsx`).
- **Logos** — vendored from [svgl.app](https://svgl.app); `npm run fetch-logos` refreshes them. Options without a logo render a letter tile.
- **Deployment** — deployable to Vercel as-is. Set `NEXT_PUBLIC_SITE_URL` to the canonical production URL for correct metadata and sitemap output.
- **License** — MIT (see [LICENSE](LICENSE)).
