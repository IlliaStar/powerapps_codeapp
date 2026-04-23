# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A **Power Apps Code App** — a code-first React/TypeScript application deployed to Microsoft Power Platform (EPAM environment). Uses spec-driven development via **OpenSpec** and is managed with the **Power Apps CLI (`pac`)**.

## Commands

### Development
```bash
npm run dev          # Start local dev server (http://localhost:5173)
npm run build        # Type-check and build to dist/
npm run lint         # ESLint
npm run preview      # Preview production build locally
```

### Power Apps CLI (pac)
```bash
pac auth list                    # List auth profiles (active profile targets epam.crm4.dynamics.com)
pac auth create                  # Re-authenticate (opens browser — run interactively)
pac code run                     # Run local dev server with live Power Platform connectors loaded
pac code push                    # Publish a new version of the app to Power Platform
pac code list                    # List deployed code apps in the environment
pac code add-data-source         # Add a Power Platform connector/data source
```

Alternatively, use the npm-based equivalents (preferred going forward as `pac code` is being deprecated):
```bash
npx power-apps run               # Local dev with live connectors
npx power-apps push              # Publish to Power Platform
npx power-apps add-data-source   # Add a connector
```

`pac code init` was run at project creation and generated `power.config.json`. The app is registered in the EPAM Default environment (`b41b72d0-4e9f-4c26-8a69-f949f367c91d`). `appId` will be populated after the first `pac code push`.

### OpenSpec (spec-driven development)
```
/opsx:propose "your idea"   # Start a new change: creates proposal + specs in openspec/changes/
/opsx:apply                 # Implement the approved spec
/opsx:archive               # Archive a completed change
```

OpenSpec structures all non-trivial changes as: `proposal.md` → `specs/` → `design.md` → `tasks.md` inside `openspec/changes/<change-name>/`. Use `/opsx:propose` before writing code for any meaningful feature.

## Architecture

```
src/                   # React/TypeScript source (Vite + React 19)
openspec/
  changes/             # Active spec-driven changes (proposal → specs → design → tasks)
  specs/               # Shared/reusable specs
.claude/
  commands/opsx/       # OpenSpec slash commands for Claude Code
  skills/              # OpenSpec skill implementations
dist/                  # Build output (gitignored) — pointed at by pac code push
```

**Tech stack:** React 19, TypeScript 6, Vite 8, ESLint 10.

**Deployment target:** Power Apps Code App on `https://epam.crm4.dynamics.com/` — build output in `dist/` with entry point `index.html` is what `pac code push` deploys.

**Data sources:** Power Platform connectors are added via `pac code add-data-source` and accessed via the Power Apps SDK in the app. Local dev uses `pac code run` (not `npm run dev`) when connector data is needed.

## Workflow

1. Propose a change with `/opsx:propose`
2. Implement with `/opsx:apply`
3. Build and test locally: `npm run build`
4. Push to Power Platform: `pac code push`
5. Archive the spec: `/opsx:archive`
