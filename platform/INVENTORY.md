# Platform inventory

Generated at: 2026-07-28T12:18:46-04:00
Domain: `apps.local`
APM enabled in compose file: `1`

| Service | Port | Role |
|---------|------|------|
| nginx | 80 | edge router for business apps |
| prometheus | 9090 | metrics |
| grafana | 3000 | dashboards (admin / changeme) |
| loki | 3100 | log store |
| promtail | (internal) | ship container logs to Loki |
| skywalking-oap | 11800 / 12800 | APM backend (compose profile `apm`) |
| skywalking-ui | 8080 | APM UI (compose profile `apm`) |

## Start

```bash
cd platform
docker compose up -d
docker compose --profile apm up -d   # optional APM
```

## Attach a business app

1. Put the app container on network `platform_edge`
2. Add Nginx upstream/location in `nginx/conf.d/`
3. Add Prometheus scrape job if the app exposes `/metrics`
4. Reload nginx: `docker compose exec nginx nginx -s reload`
