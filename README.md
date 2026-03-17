# 🛡️ ShieldShift

## 🚨 The Problem

India's 12 million gig delivery workers have no safety net

A curfew. A flood. A red-alert rain day. Orders stop. Income vanishes. Nothing they can do.

They don't need vehicle insurance. They don't need health cover. They need one thing protection for wages lost when the world around them goes wrong through no fault of their own.


## 💡 What We Built

**ShieldShift** is a weekly parametric income-protection platform built exclusively for e-commerce delivery partners (Amazon, Flipkart).

When a verified calamity hits their zone — the system detects it, validates it, and **pays them automatically**. No claim form. No phone call. No waiting.

### 🔑 Our Unique Edge — Day-Part Aware Payouts
> Most platforms ask *did a disruption happen?* We ask *when did it happen relative to this worker's peak earning hours?*

A curfew at 7pm costs Ravi ₹900. The same curfew at 2pm costs him ₹200. **Our AI knows the difference. Our payout reflects reality.**


## 👤 Persona & Scenario

### Ravi — Amazon Flex Partner, Bengaluru
- **Age:** 26 | **Platform:** Amazon Flex | **Hours:** 10am–10pm
- **Peak earning window:** 5pm–9pm (dinner surge, highest order density)
- **Weekly income:** ₹4,500–₹6,000 | **Payment:** UPI (Google Pay)

**Scenario A — The Curfew Evening**
Section 144 is declared at 5pm in Ravi's zone due to a local protest. He cannot step out. His entire peak window — worth ~₹900 — is gone.

*With ShieldShift:* Curfew detected via civic feed → zone match confirmed → active policy verified → income loss calculated against his hourly baseline → **₹850 credited to his UPI in under 90 seconds. He never touched the app.**

**Scenario B — The Rain Week**
IMD declares a Red Alert for 3 consecutive days. Orders drop 60%. Ravi earns ₹1,200 instead of his usual ₹4,800 for those days.

*With ShieldShift:* Red Alert fires parametric trigger on Day 1 → AI calculates gap against his earnings baseline → partial-week payout processed → **money in his wallet by Day 2.**


## 🔄 Application Workflow

### Phase 1 — Onboarding *(Worker acts once — under 3 minutes)*

Download PWA  →  Phone + OTP  →  Link UPI (GPay / PhonePe)  →  Select Zone (pin code)  →  Activate Weekly Plan

Worker selects their risk tier (₹25 / ₹45 / ₹80/week). Every Sunday, they confirm next week's renewal. That's it — they never need to interact again unless they want to.


### Phase 2 — Continuous AI Monitoring *(24×7, zero human involvement)*

Three live data streams feed a unified **Zone Risk Score**, refreshed every 15 minutes:


[Weather API — Rain, heat, fog]  +  [AQI API — CPCB pollution]  +  [Civic Feed — Curfews, bandh]
                                          │
                                  [Zone Risk Score]
                                  Updated every 15 min
                                          │
                            [XGBoost Income Loss Predictor]
                                          │
                              [Isolation Forest Fraud Check]
                                          │
                            [Threshold Gate: Income Drop ≥ 20%?]


**Trigger Types (T1–T4):**
| Code | Event | Data Source |
|---|---|---|
| T1 | Rain / flood / storm / heat wave / fog | IMD + OpenWeatherMap (free tier) |
| T2 | Severe air pollution AQI 400+ | CPCB AQI API (free tier) |
| T3 | Curfew / bandh / zone closure / unrest | NewsAPI + civic feed (free tier) |
| T4 | Platform delivery suspension in zone | Amazon / Flipkart API (simulated mock) |


### Phase 3 — Auto Trigger & Claim *(Zero worker action)*

Trigger fires (T1/T2/T3/T4)
        │
        ▼
Claim auto-generated — no worker input needed
        │
        ▼
Push notification via Firebase FCM → Worker's phone (within 5 seconds)


### Phase 4 — Instant UPI Payout *(Under 90 seconds end-to-end)*

Payout = Loss × Coverage Ratio  →  Razorpay UPI Payout API  →  Credited to GPay / PhonePe  →  Confirmation push sent


## 💰 Weekly Premium Model

### Why Weekly?
Gig workers don't have monthly salaries. A ₹500/month premium feels abstract. **₹25–₹80/week**, deducted after their weekly platform payout, is tangible and fair.

### How Premiums Are Calculated — Dynamic, Not Flat

Every Sunday, the AI recalculates next week's premium for each worker based on:

| Factor | Why It Matters |
| City & pin code zone | Flood-prone or curfew-heavy zones carry higher risk |
| Historical disruption frequency | Past 12 months of calamity events in their zone |
| Worker's declared peak hours | Higher peak earnings = higher potential loss = adjusted coverage |
| Season & weather forecast | Monsoon weeks priced higher than dry winter weeks |
| Platform activity history | Longer tenure = more accurate baseline = fairer premium |

**Premium Tiers:**
| Risk Level | Weekly Premium | Max Weekly Payout |
| Low (stable zone, dry season) | ₹25 | ₹1,000 |
| Medium (metro, monsoon) | ₹45 | ₹2,500 |
| High (flood-prone, curfew history) | ₹80 | ₹4,500 |

Workers see their next week's premium every Sunday before confirming. Full transparency, always.


## 🤖 AI/ML Integration Plan

### 1. Dynamic Premium Calculation
**Model:** XGBoost Regression
**Inputs:** Zone risk score, disruption history, worker peak hours, seasonal index, platform activity
**Output:** Weekly premium (₹) + risk score (0–100)
**Phase 1:** Rule-based calculator with mock training data → Phase 2: Live ML model

### 2. Income Loss Prediction (Persona-Specific)
**Model:** Linear Regression on worker's historical earnings baseline
**Key innovation:** Day-part weighting — a worker earning 70% of income between 5–9pm gets a **1.4× payout multiplier** if a trigger fires in that window vs **0.6×** if it fires at noon.

### 3. Intelligent Fraud Detection
**Model:** Isolation Forest (unsupervised anomaly detection)

Every triggered claim is validated against:
- **Location check** — worker's last GPS ping matches their declared zone
- **Activity check** — worker was active on platform within 2 hours of trigger
- **Duplicate prevention** — one payout per trigger event per worker per zone
- **Velocity check** — more than 2 payouts in 7 days flags for manual review
- **Pattern check** — if 90%+ of workers in a zone claim simultaneously → fraud signal


## 📱 Platform Choice — Mobile-First PWA

We chose a **Progressive Web App (PWA)** over a native mobile app. Here's why:

| Factor | Our Reasoning |
| No app download friction | Workers install from a browser link — one tap, done |
| Low-end device support | Works on budget Android phones with weak internet |
| WhatsApp onboarding | Workers join via a link shared in delivery partner groups |
| Single codebase | Web + mobile in one — critical for hackathon speed |
| Offline support | PWA caches key screens for areas with patchy connectivity |

A native app would take longer to build, require Play Store approval, and create a download barrier. A PWA removes all of that.


## 🔗 Integrations

| Service | Provider 
|---|---|---|
| Weather & rain alerts | IMD + OpenWeatherMap | 
| Air quality (AQI) | CPCB AQI API |
| Civic disruptions | NewsAPI.org |
| Flood / disaster alerts | NDMA public data | 
| Delivery platform data | Amazon / Flipkart | 
| UPI payouts | Razorpay Payouts API | 
| Worker authentication | Aadhaar eKYC | 
| Push notifications | Firebase FCM |
| SMS fallback | MSG91 / Twilio | 


## 📊 Analytics Dashboard

**Worker View (Mobile)**
- Active policy status + premium paid this week
- Claim history with payout timeline
- Zone risk level for the current week
- Personal earnings baseline tracker

**Admin / Insurer View (Web)**
- Live trigger event map — which zones are under alert right now
- Claims initiated vs paid (conversion funnel)
- Fraud flags queue (pending human review)
- Weekly premium pool vs payout liability
- City-level risk heatmap


## 🛠️ Tech Stack

| Layer | Technology |

| Frontend | React.js (PWA) + Tailwind CSS |
| Backend | Node.js + Express.js |
| Database | PostgreSQL |
| ML / AI | Python — XGBoost, scikit-learn, served via FastAPI |
| Real-time events | Socket.io (live trigger alerts) |
| Payments | Razorpay Sandbox |
| Notifications | Firebase FCM |
| Hosting | Vercel (frontend) + Render (backend) |
| Version control | GitHub |


## 🗓️ Development Plan — 

| Timeline | Focus | Key Deliverables |
| Week 1 (Mar 4–10) | Research & Architecture | Persona finalization · System design · DB schema · API contracts |
| Week 2 (Mar 11–17) | Core Build | Onboarding flow · Premium calculator · Trigger engine (mock) · Basic dashboard |
| Final Days (Mar 17–20) | Polish & Submit | Fraud rules · Payout sandbox · README · Demo video |

---

## 🌟 Why ShieldShift

- **Zero-friction claims** — workers never file anything. The system does it all.
- **Day-part aware AI** — payouts match actual earning loss, not flat amounts
- **Weekly model** — built around how gig workers actually live and earn
- **Laser focused** — one problem, one persona, solved completely
- **Parametric is the future** — global insurtech is moving here. We're already here.

*ShieldShift — Because your income deserves a shield.*