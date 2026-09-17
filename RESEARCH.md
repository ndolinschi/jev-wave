# TypeSafe AI Jev — Deep Research

**Date:** 2026-09-17 (Europe/Chisinau)  
**Sources:** [TypeSafe announcement](https://typesafe.ai/blog/introducing-system-one-models-and-jev), [docs quickstart](https://docs.typesafe.ai/introduction/quickstart), [API reference](https://docs.typesafe.ai/api), [developersdigest guide](https://www.developersdigest.tech/blog/typesafe-jev-system-one-models-release-guide-2026), [Actionbox review](https://actionbox.cloud/blog/typesafe-ai-jev-review/), [Every external test], community Enron screening demo.

---

## What Jev is

Jev is TypeSafe AI’s first **System One** model (early access since **2026-09-15**). It is a **decision-only** frontier model: software sends **state** + typed **questions**, and receives **Choice / Score / Noul** answers with **calibrated probabilities** — not generated text.

- Named after William Stanley Jevons (efficiency → more demand for intelligence).
- “System One” nods to Kahneman’s fast System 1 vs slow System 2.
- Founder Diogo Almeida (ex-OpenAI, instruction-following / ChatGPT lineage); ~$40M seed (DCVC).
- Training: **RLCD** (Reinforcement Learning for Calibrated Decisions) — optimize for epistemically honest probabilities, not chat preference.
- Sampling: **parallel sampler** (not autoregressive strings) → schema conformance is guaranteed; “zero hallucinations” means **no out-of-schema values**, not perfect factual accuracy.

**Mental model:** frontier-intelligence `if` / `switch` for code — unstructured or structured state in, typed probabilistic decisions out.

---

## How it works

1. App builds a `state` (string, object, or array): ticket text, EHR snippet, order + device fingerprint, resume + JD, post + policy, etc.
2. App defines one or many **questions** (up to hundreds; Choice cardinality ≤ 255; higher uses two-stage score→choice).
3. Single `POST` evaluates questions **in parallel / in isolation** against the same state (adding questions barely adds latency; batching is dramatically cheaper than N calls).
4. Response returns answers keyed by the same ids, plus `usage.input_tokens` / `output_tokens`.
5. **Code owns policy:** thresholds, confidence gates, human review, side effects. Jev never “acts.”

### Primitives

| Type | Question shape | Returns |
|------|----------------|---------|
| **Choice** | Pick one of defined options (`criteria` map) | `choice`, `probabilities{}`, `confidence` |
| **Score** | Ordered descriptive levels (`criteria` array ≥ 2) | weighted `score`, `legend`, `probabilities`, `confidence` |
| **Noul** | Yes/no (“noul” ≈ probability of yes) | `noul` ∈ [0,1] (no separate `confidence` field) |

**Patterns that matter in production**

- Confidence-gated routing: act only if `confidence` / `noul` clears a threshold; else human / slower model.
- Composite scoring: many atomic Scores + weights in code.
- Intent routing: Choice → team/queue; Noul → escalate flags.
- Speculative fan-out: ask many questions once; ignore irrelevant answers in code.
- Guardrails: Noul jailbreak / harm; Score severity; block/review/pass.

**Limits (public launch):** ~32k token / ~150k English char request budget; early-access waitlist for API keys; no public weights/SLA/rate-limit docs at launch.

---

## API shape

```http
POST https://api.typesafe.ai/v1/systemone
Authorization: Bearer $TYPESAFE_API_KEY
Content-Type: application/json
```

**Request**

```json
{
  "state": "…text or object…",
  "model": "jev-latest",
  "questions": {
    "department": {
      "type": "choice",
      "instructions": "Which team should handle this?",
      "criteria": {
        "billing": "Payment or subscription issues",
        "technical": "Bugs or integration problems",
        "sales": "Pricing or account questions"
      }
    },
    "frustration": {
      "type": "score",
      "instructions": "How frustrated the customer appears",
      "criteria": ["Calm, just stating facts", "Frustrated but civil", "Very angry, strong language"]
    },
    "is_urgent": {
      "type": "noul",
      "instructions": "The message conveys urgency or time-sensitivity"
    }
  }
}
```

**Response (shape)**

```json
{
  "model": "jev-latest",
  "answers": {
    "department": {
      "type": "choice",
      "choice": "technical",
      "probabilities": { "billing": 0.159, "technical": 0.84, "sales": 0.001 },
      "confidence": 0.596
    },
    "frustration": {
      "type": "score",
      "score": 1.035,
      "legend": { "0": "…", "1": "…", "2": "…" },
      "confidence": 0.842
    },
    "is_urgent": { "type": "noul", "noul": 0.999 }
  },
  "usage": { "input_tokens": 312, "output_tokens": 48 }
}
```

**SDKs:** `pip install typesafe-sdk` (Python), official JS/TS SDK, agent skill `typesafe-ai/skills`. Env: `TYPESAFE_API_KEY`.

**Errors:** 401 auth, 422 validation, 429 rate limit, 529 overloaded — exponential backoff.

---

## Pricing & latency

| Metric | Public claim |
|--------|----------------|
| Input | **$0.042 / MTok** ($42 / billion) |
| Output | **Free** (“too cheap to meter”; still counted in `usage`) |
| Latency | **~70–500 ms** e2e (vendor; West Coast evals) |
| Workflow evals (vendor) | Within ~3 pts of expensive frontier on decision workflows; homepage peak **~194× faster / ~445× cheaper** (high end of real-world) |
| Doom demo | ~10 decisions/sec ≈ ~$7/hour |
| Every (external) | 777 judgments / &lt;0.7s / ~$0.0025; vs Fable: ~25× faster, ~580× cheaper, slightly lower defect catch |

Arithmetic change: “is this urgent?” on every inbound message becomes a rounding error. Parallel questions in one call are ~10× faster / ~12× cheaper than N sequential calls (cookbook).

---

## Best-fit use cases

**Excellent**

- Routing & escalation (support, clinical, legal, ops)
- Priority / severity scoring
- Moderation & trust-and-safety gates
- Hiring first-pass / interview gate
- Fraud & payment risk signals
- LLM I/O guardrails (jailbreak, injection, harm)
- RAG passage filter / rerank / citation check
- Invoice / security-alert workflow branches
- High-volume document triage (community: ~45 docs/sec legal screening)

**Poor fit**

- Chat, prose, code generation, open-ended planning
- Exact arithmetic / permissions (keep in code)
- Consequential irreversible actions with no human fallback

**Design rule:** decompose judgment → atomic questions → combine with weights/thresholds you own; always keep `needs_review` / low-confidence paths.

---

## Moldova + global sale angles

### Moldova / RO / regional

1. **Clinic triage (PulseLane):** Polyclinics and small hospitals still use phone/FIFO; EN+RO UI; sell to private clinics in Chișinău, then RO/UA diaspora telemedicine.
2. **SMB support (LaneBreak):** Local SaaS, banks’ contact centers, e-gov ticketing — replace FIFO with urgency×impact routing without hiring more L1.
3. **Indie media / Telegram channels (TrustGate):** Local newsrooms and civic orgs need cheap T&S without Big Tech stacks.
4. **Hiring (HireSignal):** IT outsourcing & startups drowning in CV volume; first-pass score + interview Noul with bias-aware rubrics.
5. **Ecommerce fraud (CartShield):** Growing MD/RO Shopify-like shops and marketplace sellers — card testing / shipping risk without enterprise fraud suites.

### Global

- Same five verticals are universal; Jev’s cost/latency makes **shadow-mode → confidence-gated auto** viable for SMBs that could never afford LLM-as-judge at volume.
- Sell as **“decision layer”** bolt-on (API + dashboard), not another chatbot.
- Compliance story: typed outputs + confidence + human review queue = auditable automation (healthcare/finance need that narrative).

### Packaging

- Hobby Vercel demos → API keys → paid usage wrap on TypeSafe → vertical templates (questions + thresholds + sample states).
- Land: free demo with heuristic fallback when `TYPESAFE_API_KEY` missing; expand: webhook ingest + Slack/Email alerts.

---

## Architecture we ship

All products share:

```
UI → /api/decide → jevClient.systemOne(state, questions)
                      ├─ if TYPESAFE_API_KEY → live Jev
                      └─ else → local heuristic demo (same response shape)
```

Never commit keys; wire from `/workspace/secrets/typesafe-api-key.txt` → `.env.local` when present.
