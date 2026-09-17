# Agent Wave — Six Jev Decision Demos

Shipped 2026-09-17 (Europe/Chisinau). Centered on **agent harness decisions** (Choice / Score / Noul) — not chatbots, not CRUD.

| # | Name | Path | GitHub | Local live | Deploy |
|---|------|------|--------|------------|--------|
| 1 | **ToolGate** | `/workspace/toolgate` | https://github.com/ndolinschi/toolgate | mode=live · ~412ms | queued — see DEPLOY_QUEUE.md |
| 2 | **SwarmRouter** | `/workspace/swarmrouter` | https://github.com/ndolinschi/swarmrouter | mode=live · ~411ms | queued |
| 3 | **HarnessJudge** | `/workspace/harnessjudge` | https://github.com/ndolinschi/harnessjudge | mode=live · ~469ms | queued |
| 4 | **SpendBrake** | `/workspace/spendbrake` | https://github.com/ndolinschi/spendbrake | mode=live · ~412ms | queued |
| 5 | **McpMatch** | `/workspace/mcpmatch` | https://github.com/ndolinschi/mcpmatch | mode=live · ~411ms | queued |
| 6 | **JevPlay** | `/workspace/jevplay` | https://github.com/ndolinschi/jevplay | mode=live · ~376ms | queued |

## What each does

1. **ToolGate** — Paste planned tool/MCP call + context → `allow` / `ask_human` / `deny` + risk / exfil / irreversible / policy Nouls. Simulated agent-loop log.
2. **SwarmRouter** — Task → which agent (`research` / `code` / `browser` / `support` / `writer`) + confidence Score + multi/ambiguous Nouls. Visual swarm map.
3. **HarnessJudge** — Paste step trace → `ok` / `retry` / `escalate` / `stop` + quality Score + hallucination / tool_failed / loop Nouls.
4. **SpendBrake** — Budget + spent + plan → `continue` / `downgrade_model` / `stop` + value_for_cost Score. Cost×value dashboard.
5. **McpMatch** — Goal → two-stage match over ~40 fake-but-realistic MCP tools (category → tool). Catalog grid.
6. **JevPlay** — Flagship playground: freeform state + custom Choice/Score/Noul builder → live probability bars.

## Shared contract

- Next.js 15 App Router + TS + Tailwind 4
- `lib/jev.ts` — live TypeSafe when `TYPESAFE_API_KEY` set; else heuristic same shape
- `POST /api/decide`
- Distinct visual personalities
- `.env.local` gitignored; key from `/workspace/secrets/typesafe-api-key.txt`
- `npm run build` = 0 on all six

## Try locally

```bash
export PATH="/home/box/.local/bin:/home/box/.grok/bin:$PATH"
cd /workspace/jevplay && npm run dev
# open http://localhost:3000 — invent questions, hit Run Jev
```
