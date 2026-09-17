# Jev Wave — Vercel Deploy Queue

**Status (2026-09-17 Europe/Chisinau):** CLI logged out — no `VERCEL_TOKEN` / no saved credentials.  
Browser deploy required: `vercel login` (or device login), then deploy each Hobby project and set env `TYPESAFE_API_KEY` from `/workspace/secrets/typesafe-api-key.txt`.

## Agent wave (this ship) — deploy these

| # | Project | Path | GitHub | Suggested Hobby URL after claim |
|---|---------|------|--------|----------------------------------|
| 1 | ToolGate | `/workspace/toolgate` | https://github.com/ndolinschi/toolgate | `toolgate.vercel.app` |
| 2 | SwarmRouter | `/workspace/swarmrouter` | https://github.com/ndolinschi/swarmrouter | `swarmrouter.vercel.app` |
| 3 | HarnessJudge | `/workspace/harnessjudge` | https://github.com/ndolinschi/harnessjudge | `harnessjudge.vercel.app` |
| 4 | SpendBrake | `/workspace/spendbrake` | https://github.com/ndolinschi/spendbrake | `spendbrake.vercel.app` |
| 5 | McpMatch | `/workspace/mcpmatch` | https://github.com/ndolinschi/mcpmatch | `mcpmatch.vercel.app` |
| 6 | JevPlay | `/workspace/jevplay` | https://github.com/ndolinschi/jevplay | `jevplay.vercel.app` |

## Earlier product wave (also queued)

| Project | GitHub |
|---------|--------|
| PulseLane | https://github.com/ndolinschi/pulselane |
| LaneBreak | https://github.com/ndolinschi/lanebreak |
| TrustGate | https://github.com/ndolinschi/trustgate |
| HireSignal | https://github.com/ndolinschi/hiresignal |
| CartShield | https://github.com/ndolinschi/cartshield |

## Per-project deploy recipe

```bash
export PATH="/home/box/.local/bin:/home/box/.grok/bin:$PATH"
vercel login   # complete in browser
cd /workspace/<slug>
npx vercel --prod --yes
npx vercel env add TYPESAFE_API_KEY production < /workspace/secrets/typesafe-api-key.txt
# redeploy so env is live
npx vercel --prod --yes
```

## Note

GitHub pushes are done. Local live Jev verified on all six (mode=live, jev-1.13.0, ~370–470ms). Only Vercel hosting is blocked on auth.
