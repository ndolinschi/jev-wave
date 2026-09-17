# Five Jev-Centered Products

Criteria: real utility tomorrow · Jev decisions (not chat) · Next.js / Vercel Hobby · distinct verticals · workflow change for many people.

---

## 1. PulseLane

| | |
|--|--|
| **One-liner** | Clinic intake → ESI-style acuity + service line in <500ms so the right nurse sees the right patient first. |
| **Slug / path** | `pulselane` → `/workspace/pulselane` |
| **GitHub** | `ndolinschi/pulselane` |
| **Deploy** | Vercel Hobby |
| **Vertical** | Healthcare / clinic ops (Moldova polyclinics + global ambulatory) |

**Jev questions:** `acuity` Score · `service_line` Choice · `red_flag` Noul · `language_barrier` Noul · `contagion_concern` Noul

**MVP screens:** Intake form · Live decision board · Queue wall (not FIFO) · Demo presets

---

## 2. LaneBreak

| | |
|--|--|
| **One-liner** | Kill support FIFO — priority × team × refund/churn risk on every ticket before a human opens it. |
| **Slug / path** | `lanebreak` → `/workspace/lanebreak` |
| **GitHub** | `ndolinschi/lanebreak` |
| **Deploy** | Vercel Hobby |
| **Vertical** | Customer support / SaaS ops |

**Jev questions:** `team` Choice · `priority` Score · `refund_intent` Noul · `churn_risk` Noul · `needs_human` Noul

**MVP screens:** Ticket paste · Routing card · Lane board by team · Demo samples

---

## 3. TrustGate

| | |
|--|--|
| **One-liner** | Indie media T&S gate: publish, soft-hold, or block — with calibrated harm scores. |
| **Slug / path** | `trustgate` → `/workspace/trustgate` |
| **GitHub** | `ndolinschi/trustgate` |
| **Deploy** | Vercel Hobby |
| **Vertical** | Trust & safety / indie publishers |

**Jev questions:** `action` Choice · `harm` Score · `hate_or_harassment` Noul · `misinfo_sensitive` Noul · `spam_or_scam` Noul

**MVP screens:** Composer + policy preset · Gate decision + probability bars · Mod queue

---

## 4. HireSignal

| | |
|--|--|
| **One-liner** | Resume × JD → fit Score + interview Noul so recruiters stop drowning in first-pass noise. |
| **Slug / path** | `hiresignal` → `/workspace/hiresignal` |
| **GitHub** | `ndolinschi/hiresignal` |
| **Deploy** | Vercel Hobby |
| **Vertical** | Recruiting / IT outsourcing (MD) + startups |

**Jev questions:** `fit` Score · `interview` Noul · `seniority` Choice · `must_have_gap` Noul · `red_flag` Noul

**MVP screens:** JD + resume side-by-side · Signal card · Batch table

---

## 5. CartShield

| | |
|--|--|
| **One-liner** | Checkout risk Noul for SMB ecommerce — approve, 3DS, hold, or decline without an enterprise fraud suite. |
| **Slug / path** | `cartshield` → `/workspace/cartshield` |
| **GitHub** | `ndolinschi/cartshield` |
| **Deploy** | Vercel Hobby |
| **Vertical** | Ecommerce / payments risk |

**Jev questions:** `disposition` Choice · `fraud_risk` Score · `card_testing` Noul · `shipping_mismatch` Noul · `friendly_fraud_cue` Noul

**MVP screens:** Order risk form · Decision strip + risk meter · Review queue

---

## Shared contract

- Next.js App Router + TypeScript + Tailwind
- `lib/jev.ts`: live TypeSafe when `TYPESAFE_API_KEY` set; else heuristic demo (same answer shape)
- `POST /api/decide`
- Distinct UI personalities
- Never commit secrets; optional wire from `/workspace/secrets/typesafe-api-key.txt`
