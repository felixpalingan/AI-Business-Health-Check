# AI Business Health Check

An AI-powered diagnostic tool for assessing the health of Micro, Small, and Medium Enterprises (MSMEs/UMKM) in Indonesia. Built on a **transparent, theory-driven methodology** — not a black box.

## 🎯 Purpose

This tool helps business consultants and MSME owners:
1. **Diagnose** the overall health of a business across 4 key dimensions
2. **Identify** critical red flags that require immediate intervention
3. **Receive** AI-generated 30-day action plans grounded in calculated scores (not hallucinated)
4. **Consult** interactively with an AI business advisor with full diagnostic context

## 🏗️ Architecture: White Box, Not Black Box

This tool **strictly separates deterministic scoring from AI narrative generation & conversation**:

```
                  ┌──────────────────────┐
                  │   User Input Form    │
                  └──────────┬───────────┘
                             │
                             ▼
              ┌──────────────────────────────┐
              │ Deterministic Scoring Engine │ ──> Hard-coded formulas (100% transparent)
              └──────────────┬───────────────┘
                             │
                             ▼
              ┌──────────────────────────────┐
              │  Structured Context Payload  │ ──> Profile, 4-pillar scores, red flags (JSON)
              └───────┬──────────────┬───────┘
                      │              │
           ┌──────────┴───┐      ┌───┴──────────────────────────────┐
           ▼              │      ▼                                  ▼
┌──────────────────────┐  │  ┌────────────────────────────────────────────────────────┐
│ Results Dashboard    │  │  │ Context-Aware Interactive AI Chat Advisor                 │
│ (Scores, Radar Chart,│  │  │ (Gemini 2.5 API + Injected Assessment Context)             │
│ 30-Day Action Plan)  │  │  │ - Follow-up questions ("How do I separate accounts?")    │
└──────────────────────┘  │  │ - Industry-specific coaching (F&B, Retail, Services)      │
                          │  │ - Step-by-step implementation guidance                   │
                          │  └────────────────────────────────────────────────────────┘
                          │
                          ▼
           ┌──────────────────────────────┐
           │ Continuous Vercel CI/CD      │ ──> Auto-deploy on push to GitHub main
           └──────────────────────────────┘
```

The AI **does NOT** calculate scores or trigger red flags. Those are computed by a rule-based engine with published, auditable formulas. The AI provides contextual analysis and interactive consultation based on pre-computed diagnostic data.

---

## 💬 Interactive AI Chat Advisor (Context-Aware Consultation)

After completing the diagnostic assessment, users can engage in a live dialogue with an AI business consultant:

- **Injected Context Payload**:
  - Business profile (name, industry sector, business age, employee headcount)
  - Business classification under **PP No. 7/2021** (Micro vs. Small vs. Medium based on revenue & capital)
  - Calculated 4-pillar scores: Financial ($S_{fin}$), Operations ($S_{ops}$), Market ($S_{mkt}$), Governance ($S_{gov}$)
  - Active Red Flags (P0 Liquidity Danger, P1 Operational Bottleneck, P2 Regulatory Risk)
  - Generated 30-day priority action items
- **Consultation Scope**:
  - Deep-dive into specific diagnostic results (*"Why did I get 20 on cash runway and how do I fix it?"*)
  - Practical execution advice (*"What bookkeeping apps work best for a small coffee shop in Indonesia?"*)
  - Regulatory guidance (*"What are the step-by-step requirements to get an NIB through OSS?"*)
- **Guardrails**:
  - Grounded strictly in the user's assessment data
  - Cannot alter or override calculated scores
  - Tailored to Indonesian regulatory and market realities

---

## 📚 Theoretical Foundation

### Framework: Adapted Balanced Scorecard (Kaplan & Norton, 1996)

The original BSC's 4 perspectives are adapted for Indonesian MSMEs:

| BSC Perspective | MSME Diagnostic Dimension | Weight |
|---|---|---|
| Financial | 💰 **Financial Health** ($S_{fin}$) | 35% |
| Internal Business Process | ⚙️ **Operations & Efficiency** ($S_{ops}$) | 25% |
| Customer | 📊 **Market & Customer** ($S_{mkt}$) | 20% |
| Learning & Growth | 🏛️ **Governance & Human Capital** ($S_{gov}$) | 20% |

**Why BSC?**
- Validated by academic literature (SLR 2022-2026) for SME applicability
- Connects financial performance with operational and governance drivers
- Easily adapted for micro/small businesses without losing analytical rigor

**Why NOT BCG Matrix?**
- Requires multi-product portfolio data (relative market share, industry growth rate)
- Most micro/small businesses operate 1-3 core products and lack external market research data
- Too macro-level for operational MSME health diagnosis

### Regulatory Basis: PP No. 7/2021

Business classification follows Indonesian Government Regulation No. 7/2021:

| Category | Business Capital (excl. land & buildings) | Annual Revenue |
|---|---|---|
| **Micro (Mikro)** | ≤ Rp 1 Billion | ≤ Rp 2 Billion |
| **Small (Kecil)** | > Rp 1B — Rp 5B | > Rp 2B — Rp 15B |
| **Medium (Menengah)** | > Rp 5B — Rp 10B | > Rp 15B — Rp 50B |

**Initial Scope (v1)**: Micro & Small enterprises (revenue ≤ Rp 15B) in high-density sectors (F&B, Retail, Personal Services).

---

## 📊 Scoring Methodology

### Total Score Formula

```
Total Score = (S_fin × 0.35) + (S_ops × 0.25) + (S_mkt × 0.20) + (S_gov × 0.20)
```

Each dimension produces a score of 0-100. The final composite score is 0-100.

### Score Interpretation

| Range | Category | Description |
|---|---|---|
| 80 – 100 | 🟢 **Healthy** | Business running well, ready for expansion/credit |
| 60 – 79 | 🟡 **Fairly Healthy** | Operational, but specific structural gaps need attention |
| 40 – 59 | 🟠 **Needs Attention** | Significant vulnerabilities present |
| 0 – 39 | 🔴 **Critical** | High immediate risk of distress or insolvency |

---

### Dimension 1: Financial Health ($S_{fin}$ — Weight 35%)

| Sub-Indicator | Weight | Options & Score | Basis |
|---|---|---|---|
| **Account Separation** | 25% | Separated = 100 · Mixed = 0 (**→ Red Flag P0**) | SAK EMKM; basic accounting hygiene; KUR prerequisite |
| **Financial Record-Keeping** | 25% | Software/SaaS = 100 · Spreadsheet = 75 · Manual ledger = 40 · None = 0 | Financial transparency & auditability |
| **Cash Runway** | 30% | ≥3 months = 100 · 1-2.9 months = 60 · <1 month = 20 (**→ Red Flag P0**) | `Available Cash ÷ Monthly OPEX` |
| **Gross Profit Margin** | 20% | Healthy (≥30% F&B, ≥20% Retail) = 100 · Thin/unknown = 20 | Industry benchmarks (BPS, LPPI) |

```
S_fin = (Account × 0.25) + (Recording × 0.25) + (Runway × 0.30) + (GPM × 0.20)
```

### Dimension 2: Operations & Efficiency ($S_{ops}$ — Weight 25%)

| Sub-Indicator | Weight | Options & Score | Basis |
|---|---|---|---|
| **SOP Documentation** | 35% | Written & consistently executed = 100 · Partial = 50 · None = 10 | Process standardization |
| **Owner Dependency Index** | 35% | Can leave ≥1 month = 100 · Max 1 week = 50 · ≤2 days = 0 (**→ Red Flag P1**) | Business continuity & key-person risk |
| **Inventory / Waste Control** | 30% | Regular stock opname = 100 · Purchase logs only = 40 · None = 0 | Supply chain loss prevention |

```
S_ops = (SOP × 0.35) + (OwnerDep × 0.35) + (Inventory × 0.30)
```

### Dimension 3: Market & Customer ($S_{mkt}$ — Weight 20%)

| Sub-Indicator | Weight | Options & Score | Basis |
|---|---|---|---|
| **Revenue Concentration** | 50% | No client >40% revenue = 100 · Single client ≥50% = 30 | Customer dependency vulnerability |
| **Sales Channels** | 25% | Multi-channel (Offline + Online) = 100 · Single-channel = 50 | Revenue channel diversification |
| **Repeat Purchase Tracking** | 25% | Actively tracked = 100 · One-off / Untracked = 40 | Customer retention & LTV health |

```
S_mkt = (Concentration × 0.50) + (Channels × 0.25) + (Retention × 0.25)
```

### Dimension 4: Governance & Human Capital ($S_{gov}$ — Weight 20%)

| Sub-Indicator | Weight | Options & Score | Basis |
|---|---|---|---|
| **Business Licensing** | 50% | NIB + Sectoral Permit/Cert = 100 · NIB only = 60 · None = 10 | PP 7/2021; OSS compliance; formal finance access |
| **HR & Compensation** | 50% | Clear roles + performance incentives = 100 · Fixed salary only = 50 | Human capital retention & alignment |

```
S_gov = (Licensing × 0.50) + (HR × 0.50)
```

---

### Red Flag System (Rule-Based, Not AI)

Red flags are triggered **deterministically prior to LLM narrative generation**:

| Priority | Trigger Condition | Label | Diagnostic Impact |
|---|---|---|---|
| **P0** (Critical) | Mixed accounts **OR** Cash runway < 1 month | 🔴 Liquidity Danger | Severe risk of sudden insolvency |
| **P1** (Bottleneck) | Owner dependency ≤ 2 working days | 🟠 Operational Bottleneck | Enterprise cannot scale beyond founder hours |
| **P2** (Compliance) | Operating > 1 year without NIB | 🟡 Regulatory Risk | Ineligible for KUR loans, government procurement, or formal permits |

---

## 🤖 AI's Role (Strict Boundaries)

| ✅ AI Does | ❌ AI Does NOT |
|---|---|
| Explain *why* vulnerabilities exist in the user's specific context | Calculate scores or override formulas |
| Synthesize a customized 30-day Action Plan (Week 1–4) | Determine red flag triggers (rule-based) |
| Map diagnostic findings to relevant regulations (PP 7/2021, OSS) | Reclassify business scale |
| Power the **Interactive AI Chat Advisor** with full assessment context | Hallucinate metrics or make speculative assumptions |

---

## 👥 Team & Responsibilities

| Contributor | Area of Responsibility |
|---|---|
| **Rayyan** | **UI/UX Design Mockups**, Visual Design System, typography, tokens, user flow layouts |
| **Felix & AI Pair** | **Methodology & Architecture**, Deterministic Scoring Engine, Gemini API Integration, Context-Aware AI Chat, Frontend Implementation, Vercel CI/CD, Notion Tracker |

---

## 🚀 Deployment & CI/CD Strategy

1. **Early Vercel Deployment**:
   - Vercel is linked directly to the GitHub repository from Day 1.
   - Every push to the `main` branch triggers an automated build and deployment preview.
   - Allows immediate, continuous testing of logic and UI updates across team members.
2. **Future VPS Migration**:
   - Designed with clean decoupling to facilitate seamless migration to a dedicated VPS for production scaling.

---

## 🗺️ Project Implementation Roadmap

- [x] **Phase 0: Groundwork & Methodology**
  - Theory formulation (Adapted Balanced Scorecard + PP 7/2021)
  - GitHub repo & Notion methodology sync
  - Alignment on white-box deterministic principles
- [ ] **Phase 1: Early Vercel Deployment & Baseline Setup**
  - Clean Next.js project skeleton
  - Vercel CI/CD integration linked to GitHub `main`
  - Automated deployment verification
- [ ] **Phase 2: Deterministic Scoring Engine & Unit Tests**
  - Pure JS/TS scoring calculator with published weights
  - P0/P1/P2 rule-based red flag detector
  - Comprehensive unit tests for all edge cases
- [ ] **Phase 3: UI & Multi-Step Assessment Form**
  - Integration of Rayyan's UI/UX design mockups
  - State machine for progressive assessment input
  - Live client-side score preview
- [ ] **Phase 4: Results Dashboard & Gemini AI Narrative**
  - Visual breakdown by 4 dimensions & radar chart
  - Gemini API structured payload integration
  - 30-day phased action plan generator
- [ ] **Phase 5: Context-Aware Interactive AI Chat Advisor**
  - Chat interface linked to assessment results
  - Context injection pipeline (Profile + Scores + Red Flags + Recommendations)
  - Streaming conversational responses
- [ ] **Phase 6: Bilingual Support & Internal Testing**
  - English-first with toggle to Bahasa Indonesia
  - Team review & internal dogfooding sessions

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Frontend Framework** | Next.js (React) |
| **Styling** | Vanilla CSS (Tailored Design System based on Rayyan's mockups) |
| **Scoring Engine** | Pure JavaScript / TypeScript (Deterministic, 100% testable) |
| **AI Layer** | Google Gemini API (Narrative Generator + Context-Aware Chat) |
| **Hosting & CI/CD** | Vercel (Initial & Staging) → Self-hosted VPS (Production target) |
| **Version Control** | GitHub (`felixpalingan/AI-Business-Health-Check`) |
| **Project & Task Sync** | Notion Database |

---

## 📚 Academic & Regulatory References

1. **Kaplan, R. S., & Norton, D. P. (1996)**. *The Balanced Scorecard: Translating Strategy into Action*. Harvard Business School Press.
2. **Peraturan Pemerintah Republik Indonesia No. 7 Tahun 2021** tentang Kemudahan, Pelindungan, dan Pemberdayaan Koperasi dan Usaha Mikro, Kecil, dan Menengah.
3. **Ikatan Akuntan Indonesia (IAI)**. *Standar Akuntansi Keuangan Entitas Mikro, Kecil, dan Menengah (SAK EMKM)*.
4. **Systematic Literature Review (2022–2026)**: *Balanced Scorecard Applications for Small and Medium Enterprises*.

---

## 📄 License

Internal Tooling — All Rights Reserved.
