# Platform Bootstrap

Adapt a single VPS into a minimal shared infrastructure platform for multiple business apps.

## When to use

Use this skill when the user wants to provision or evolve baseline platform services on a VPS, such as:

- reverse proxy / routing (Nginx)
- metrics (Prometheus)
- dashboards (Grafana)
- logs (Loki + Promtail)
- optional APM (SkyWalking)
- a simple Docker-based CI/CD skeleton

## Non-goals

- Do NOT pretend to be Kubernetes, Terraform Cloud, or a managed PaaS
- Do NOT destroy existing named volumes without explicit confirmation
- Do NOT expose Grafana/Prometheus/SkyWalking publicly without auth guidance
- Do NOT install packages system-wide when Docker Compose is enough

## Operating procedure

1. Probe the host (non-destructive):
   - `uname -a`, free disk, free memory
   - `docker version`, `docker compose version`
   - listening ports: 80, 443, 3000, 9090, 3100, 11800, 12800
2. Ask (or infer from args) which modules to enable:
   - always: nginx, prometheus, grafana, loki, promtail, ci skeleton
   - optional: skywalking (`ENABLE_APM=1`)
3. Generate files under `platform/` by running:
   ```bash
   DOMAIN=<domain> ENABLE_APM=<0|1> ./scripts/bootstrap-platform.sh
   ```
4. Show `platform/INVENTORY.md` and the compose service list to the user
5. Only after confirmation, run:
   ```bash
   cd platform && docker compose up -d
   ```
6. Record what changed in `platform/CHANGELOG.md` (append a dated note)

## Adaptation rules for "many business systems"

When the user already has apps on the same VPS:

- Prefer adding an Nginx upstream + `location` / server block per app
- Prefer each app exporting `/metrics` for Prometheus scrape
- Prefer apps writing logs to stdout so Promtail/Docker logging can collect them
- Keep platform services on a dedicated compose project name: `platform`
- Keep app compose projects separate; only share the external docker network `platform_edge`

## Safety checklist before `docker compose up`

- [ ] No port conflict with existing services
- [ ] Grafana admin password is not the default in production
- [ ] Data directories / volumes are on a disk with enough space
- [ ] User understands this is a starter stack, not production-hardened HA

## Example user prompts this skill should handle

- "幫我在這臺 VPS 長出基礎平臺：nginx + prometheus + grafana + 日誌"
- "我已經有兩個業務容器，幫我掛到 nginx 並納入監控"
- "加 SkyWalking，但先不要對外暴露 UI"
- "給我一個 docker 流的簡易 CI 骨架"

## Success criteria

- `docker compose config` succeeds
- `platform/INVENTORY.md` lists every service, port, and purpose
- User can open Grafana after stack is up (when they chose to start it)
- Adding a new business app only requires Nginx + Prometheus scrape edits, not reinventing the platform
