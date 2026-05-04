---
name: inspect-repo
description: Inspect a local codebase to determine what should be deployed to PlugLayer, whether to use Dockerfile or docker-compose, likely ports, env vars, and service boundaries.
---

# Inspect Repo for PlugLayer Deploy

Use this skill when the user asks to deploy an app from a local codebase, wants to know whether the repo is deployable, or needs help choosing image vs docker-compose.

## Goals
- Identify deployable units
- Decide Dockerfile vs docker-compose
- Decide whether the current repo should be built locally and then deployed as an image
- Detect likely framework/runtime
- Find build/start commands
- Infer likely port
- Detect required environment variables
- Highlight missing deployment prerequisites

## What to inspect
1. Root files:
- `Dockerfile`
- `docker-compose.yml` / `compose.yaml`
- `package.json`
- `pnpm-workspace.yaml`
- `turbo.json`
- `pyproject.toml`
- `requirements.txt`
- `go.mod`
- `Cargo.toml`
- `.env.example`
- framework config files

2. Candidate app directories
- `apps/*`
- `services/*`
- `packages/*`
- `backend/`, `frontend/`, `api/`, `web/`

3. Runtime hints
- exposed ports in code or config
- `PORT` env usage
- server bind host
- health endpoints
- database/cache dependencies

## Output format
Provide a compact structured summary:
- deployable unit(s)
- recommended deploy path: `dockerfile` or `compose`
- detected framework/runtime
- likely port
- likely build/start command
- required env vars
- confidence/risk notes

## Decision rules
- If a single app can run alone, prefer Dockerfile deployment.
- If multiple tightly coupled services are required together, prefer docker-compose deployment.
- If a Dockerfile exists but is obviously incomplete, say so and propose a patch.
- If no Dockerfile exists but confidence is high, recommend generating one.
