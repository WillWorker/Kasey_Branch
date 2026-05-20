---
name: AGENTS
description: This file contains the main entrypoint for instruction.
---

# We are building a repo that can support deploying multiple services to Railway.

```
/api - barebones express backend app
/core - empty models file
/docs - empty readme
/scripts - handy dev tools
/web - angular frontend
/db - ???
```

## Structure (decision)

We will treat this repo as a **monorepo with per-service configs**, optimized for Railway:

- Root:
  - Shared documentation, scripts, and conventions.
  - Does not run its own server; it’s the “orchestration” layer.

- /api:
  - Express backend service.
  - Own `package.json` and `railway.toml`.
  - Deployable as its own Railway service.

- /web:
  - Angular frontend.
  - Builds to static files; deployed as a separate Railway service.

- /core:
  - Shared TypeScript models, utilities, and types.
  - Intended to be imported by /api, /web, and future services.
  - For now: lightweight; avoid heavy runtimes here.

- /db:
  - Database-related configuration and scripts (migrations, seeds, helpers).
  - We will use a managed Postgres add-on on Railway.
  - This directory ensures DB concerns have a home without cluttering /api.

- /docs:
  - Project documentation: architecture, decisions, operational runbooks.

- /scripts:
  - Local dev and deploy helpers (e.g., `web.dev.js`, `api.dev.js`).
  - Not part of production runtime; for developer workflow only.

## Railway deployment approach (decision)

- Each deployable service:
  - Has its own:
    - `package.json`
    - `railway.toml` (or equivalent)
    - Clear start/build commands
- We will:
  - Link each service to this same repo.
  - Configure per-service:
    - Root directory: `/api`, `/web`, etc.
    - Build and start commands from their `railway.toml`.
- /db:
  - No custom container now; use Railway Postgres add-on.
  - Later: migrations can be run from a small script or separate “migrate” service.

If a new question arises about layout, refer to this section instead of reinventing.