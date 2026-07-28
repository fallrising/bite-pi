# AGENTS.md — pi-platform-demo

This repo demonstrates using Pi as a VPS platform scaffolder.

## Priority

1. Prefer the `platform-bootstrap` skill for any infra generation request
2. Prefer editing files under `platform/` over inventing a new layout
3. Prefer Docker Compose over installing host packages
4. Never run destructive docker commands (`down -v`, `volume rm`) unless the user explicitly asks

## Project shape

- `.pi/skills/` — Pi skills (recipes)
- `scripts/bootstrap-platform.sh` — deterministic generator Pi should call
- `platform/` — generated infrastructure (source of truth after bootstrap)

## Language

Answer the user in the same language they use. Default to Traditional Chinese if mixed.
