# Jev Wave Status — 2026-09-17 (Europe/Chisinau)

## Research
- `/workspace/jev-wave/RESEARCH.md` — how Jev works, API, pricing, Moldova+global angles
- `/workspace/jev-wave/PRODUCTS.md` — 5 product specs
- Research repo: https://github.com/ndolinschi/jev-wave

## Products (all build + live Jev verified)

| # | Product | GitHub | Local smoke (live Jev) |
|---|---------|--------|------------------------|
| 1 | **PulseLane** clinic triage | https://github.com/ndolinschi/pulselane | mode=live · jev-1.13.0 · ~425ms |
| 2 | **LaneBreak** support routing | https://github.com/ndolinschi/lanebreak | mode=live · ~464ms |
| 3 | **TrustGate** indie T&S | https://github.com/ndolinschi/trustgate | mode=live · ~430ms |
| 4 | **HireSignal** resume first-pass | https://github.com/ndolinschi/hiresignal | mode=live · ~447ms |
| 5 | **CartShield** checkout risk | https://github.com/ndolinschi/cartshield | mode=live · ~407ms |

## Key wiring
- Source: `/workspace/secrets/typesafe-api-key.txt` (mode 600)
- Wired as `TYPESAFE_API_KEY` into each app `.env.local` (gitignored, never committed)
- Live curl + Next `/api/decide` confirmed against `POST https://api.typesafe.ai/v1/systemone` model `jev-latest` → `jev-1.13.0`

## Vercel
- **Blocked on auth** — CLI has no credentials / no `VERCEL_TOKEN`
- Device login pending: https://vercel.com/oauth/device?user_code=WBSX-HFRL
- After login: deploy each repo Hobby + set `TYPESAFE_API_KEY` env from secrets file

## Demo fallback
- Without key → heuristic demo same answer shape
- With key → live Jev (current state)
