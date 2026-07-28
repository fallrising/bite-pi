# /platform — 對 Pi 說這句就會觸發基礎平臺搭建

讀 `.pi/skills/platform-bootstrap/SKILL.md`，然後執行：

```bash
DOMAIN=$1 ENABLE_APM=${2:-0} ./scripts/bootstrap-platform.sh
```

參數：
- `$1` 域名，預設 `apps.local`
- `$2` 是否產生 SkyWalking（`1`/`0`）

完成後摘要 `platform/INVENTORY.md`，**不要**自動 `docker compose up`，先等我確認。
