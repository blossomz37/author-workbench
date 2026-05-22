# Author Workbench

**Snapshot date:** 2026-05-22  
**Status:** V1 scaffold and workflow proof  
**HTML version:** [PROJECT_SNAPSHOT.html](PROJECT_SNAPSHOT.html)  
**GitHub Pages URL:** <https://blossomz37.github.io/author-workbench/>

Author Workbench is a local-first author workflow app for testing AI-assisted fiction workflows. It loads author project files, assembles prompts, streams mock or OpenAI output, pauses for human review, and saves each run as local evidence.

This public snapshot is intentionally small. It is a shareable check-in for friends comparing parallel approaches, not the full source repo.

## Quick Summary

| Area | Current Shape |
|---|---|
| App type | Local-first author workflow tool |
| Backend | Express + TypeScript |
| Frontend | Svelte 5 + Vite + TypeScript |
| Streaming | Server-Sent Events |
| Prompt layer | `project.yaml`, Markdown context files, Handlebars templates |
| Providers | Mock provider and OpenAI SDK text streaming |
| Storage | Local project folders and timestamped `runs/` folders |
| Database | None for V1 |

## What It Proves

- Browser streaming works with Server-Sent Events.
- Local author files can be read into prompt context.
- Handlebars templates can assemble draft, audit, and skill-backed prompts.
- Run folders can preserve prompts, streamed events, drafts, approvals, audits, and outputs.
- A draft run can pause for human approval and then resume into an audit run.
- Mock mode can test the workflow without an API key.
- A heavier real-world fixture can stress prompt size and action metadata.

## Current Workflow Model

```text
Project files
  -> prompt assembly
  -> streamed draft
  -> author review
  -> approved text
  -> streamed audit
  -> saved run evidence
```

## Recent Direction

The project started as a single draft-and-audit proof. It now has a small app-action layer:

| Runner | Purpose |
|---|---|
| `draft-and-audit` | Draft, pause for review, then audit approved text |
| `single-step` | Run one-shot actions, such as auditing an existing chapter |
| `skill-template` | Wrap a local agent skill into an app-visible action |

Action metadata currently lives in `project.yaml`, with strict validation so malformed actions fail early.

## Design Bias

- Local files as truth.
- Visible prompt assembly.
- Saved evidence over hidden runtime state.
- Author approval before downstream audit.
- Mock-first testing.
- Simple infrastructure before a database or full workflow engine.
- Explicit action wrappers for skills, not automatic `SKILL.md` discovery.

## Support Skills Being Developed

| Skill | Purpose |
|---|---|
| `specforge` | Turns fuzzy app or workflow ideas into evidence-backed build specs |
| `skill-to-app-wizard` | Converts a readable agent skill into a testable app action |
| `express-svelte-monorepo` | Captures the Express + Svelte scaffold pattern |
| `sse-streaming` | Documents reusable SSE streaming patterns |
| `svelte5-rune-store` | Documents shared Svelte 5 rune-store state patterns |

## Still Open

- Whether to add an Anthropic provider adapter.
- How to handle very large dossiers and context-window limits.
- Whether multiple approval gates belong in V1 or a later workflow engine.
- Whether action metadata should stay in `project.yaml` or move to a dedicated `actions.yaml`.
- Which real skills should be wrapped next.
- How polished the UI needs to become before this is useful beyond internal testing.

## Local Run Commands

These commands apply to the private working repo, not necessarily to this public snapshot folder:

```bash
npm install
npm run dev:server
npm run dev:ui
```

Then open:

```text
http://localhost:5173/
```

Mock mode works without an API key. OpenAI mode requires `OPENAI_API_KEY` in the server environment.

## What This Docs Folder Contains

| File | Purpose |
|---|---|
| [`index.html`](index.html) | GitHub Pages entry point that opens the HTML snapshot |
| [`PROJECT_SNAPSHOT.md`](PROJECT_SNAPSHOT.md) | GitHub-friendly project overview |
| [`PROJECT_SNAPSHOT.html`](PROJECT_SNAPSHOT.html) | Cute standalone HTML snapshot |

The full app source, fixtures, docs, and private working notes are not included in this public share.
