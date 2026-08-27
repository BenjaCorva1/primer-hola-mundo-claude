# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Harborline: a single-file static HTML prototype of a helpdesk/ticketing UI (Zendesk-style). The entire app — markup, CSS, and JS — lives in `index.html`. There is no build system, package manager, framework, or test suite.

## Running it

Open `index.html` directly in a browser, or serve it locally, e.g.:

```
python3 -m http.server
```

There is no build, lint, or test command — there are no config/tooling files in this repo.

## Architecture

Everything is in `index.html`, in three parts:

1. **`<style>`** — all CSS, driven entirely by custom properties on `:root`. Light theme is the default `:root` block; dark theme is applied both via `@media (prefers-color-scheme: dark)` and via `:root[data-theme="dark"]` (for an explicit toggle), so any new color must be added as a variable in all three places to stay theme-consistent. Layout is a single CSS grid (`.app`) with named areas (`rail topbar list detail context`) that collapses at two breakpoints (1100px hides the context pane, 760px collapses to a mobile single-pane view with a slide-in detail panel).

2. **HTML skeleton** — static shell (`#app`, rail, topbar, and three empty containers `#tabs`, `#ticketList`, `#detailPane`, `#contextPane`) that JS renders into.

3. **`<script>`** — plain vanilla JS, no framework:
   - `tickets`: the in-memory array of all ticket data (id, status, priority, requester, messages). This is the only data source — there is no backend/API/persistence; state resets on page reload.
   - Module-level mutable state: `activeTab`, `query`, `selectedId`, `replyTab`.
   - Render functions (`renderTabs`, `renderList`, `renderDetail`, `renderContext`) each fully re-render their DOM region from current state via `innerHTML`, then re-bind event listeners on the new elements. There is no diffing/virtual DOM — any state change is followed by calling the relevant `render*` function(s) again.
   - `selectTicket(id)` is the central navigation action: updates `selectedId`, resets `replyTab`, and re-renders list/detail/context (plus toggles the mobile `detail-open` body class).
   - Ticket status/priority edits, and new replies/notes, mutate objects directly inside the `tickets` array (e.g. `t.status = ...`, `t.messages.push(...)`), then trigger a re-render — there are no setters or events, just direct mutation followed by manual re-render calls.

UI copy/labels are in Spanish (e.g. `STATUS_LABEL`, `PRIORITY_LABEL`, button titles); keep new user-facing strings consistent with that.
