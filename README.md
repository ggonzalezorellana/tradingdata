# tradingdata

Export horario (minuto 7, UTC) de los bots de Freqtrade del servidor `trading-srv`.
**Todos los bots están en dry-run** (200 USDT simulados cada uno, Binance spot).
Lo genera `/opt/trading/scripts/export-data.sh` en modo solo lectura sobre las bases SQLite.

## Estructura

| Ruta | Contenido |
|---|---|
| `metrics/bots.csv` | bot, estrategia, timeframe, puerto |
| `metrics/balances.csv` | snapshot cada 5 min por bot: saldo total (incl. posiciones abiertas), ganancia cerrada/total, trades abiertos/cerrados |
| `metrics/decisions.csv` | cada entrada/salida con precio a +1h/+4h/+24h (`ret_*`) y `edge_*` (entrada: `ret`; salida: `-ret`; positivo = buena decisión) |
| `metrics/backtests.csv` | backtests por bot: `ajuste` 2021-01-01→2024-01-01 y `validacion` 2024-01-01→hoy, mismo setup que el dry-run |
| `trades/<bot>/trades.csv` | tabla `trades` de Freqtrade del bot |
| `trades/<bot>/orders.csv` | tabla `orders` de Freqtrade del bot |
| `export_info.txt` | fecha UTC del export y filas por archivo |

## Bots

- `freqtrade`: bot principal, DonchianTrendV3a en 4h.
- `ft-v3a-<tf>`: DonchianTrendV3a (Donchian 20/55/100; entra si BTC 1d > EMA100; sale si trend_score ≤ 1/3 o BTC < EMA100) en 1h, 30m, 15m, 5m, 1m.
- `ft-nf-<tf>`: DonchianTrendV3aNF, igual sin filtro de BTC, en 4h, 1h, 30m, 15m, 5m, 1m.

Fechas en UTC. Precios y montos en USDT.
