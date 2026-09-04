# AI Business Health Check

An AI-powered diagnostic tool for assessing the health of Micro, Small, and Medium Enterprises (MSMEs/UMKM) in Indonesia. Built on a **transparent, theory-driven methodology** — not a black box.

## 🎯 Purpose

This tool helps business consultants and MSME owners:
1. **Diagnose** the overall health of a business across 4 key dimensions
2. **Identify** critical red flags that require immediate attention
3. **Receive** AI-generated action plans grounded in calculated scores (not hallucinated)

## 🏗️ Architecture: White Box, Not Black Box

This tool **separates deterministic scoring from AI narrative generation**:

```
[User Input Form]
        │
        ▼
[Deterministic Scoring Engine] ──> Hard-coded formulas, 100% transparent
        │
        ▼
[Structured Prompt Payload]     ──> Scores, red flags, context (JSON)
        │
        ▼
[LLM (Gemini API)]             ──> Narrative analysis & action plans ONLY
```

The AI **does NOT** calculate scores or determine red flags. Those are computed by a rule-based engine with published formulas. The AI only provides contextual narrative based on pre-computed data.

---

## 📚 Theoretical Foundation

### Framework: Adapted Balanced Scorecard (Kaplan & Norton, 1996)

The original BSC's 4 perspectives are adapted for Indonesian MSMEs:

| BSC Perspective | MSME Diagnostic Dimension |
|---|---|
| Financial | 💰 **Financial Health** |
| Customer | 📊 **Market & Customer** |
| Internal Business Process | ⚙️ **Operations & Efficiency** |
| Learning & Growth | 🏛️ **Governance & Human Capital** |

**Why BSC?**
- Validated by academic literature (SLR 2022-2026) for SME applicability
- Comprehensive: connects financial targets with operational capacity
- Adaptable: can be simplified for micro/small businesses without losing rigor

**Why NOT BCG Matrix?**
- Requires multi-product portfolio data (market share, industry growth rate)
- Most micro/small businesses have 1-3 products and no industry research data
- Too macro-level for individual MSME diagnosis

### Regulatory Basis: PP No. 7/2021

Business segmentation follows Indonesian Government Regulation No. 7/2021:

| Category | Business Capital (excl. land & buildings) | Annual Revenue |
|---|---|---|
| **Micro** | ≤ Rp 1 Billion | ≤ Rp 2 Billion |
| **Small** | > Rp 1B — Rp 5B | > Rp 2B — Rp 15B |
| **Medium** | > Rp 5B — Rp 10B | > Rp 15B — Rp 50B |

**v1 Scope**: Micro & Small businesses (revenue ≤ Rp 15B) with simple business models (F&B, Retail, Personal Services).

---

## 📊 Scoring Methodology

### Total Score Formula

```
Total Score = (S_fin × 0.35) + (S_ops × 0.25) + (S_mkt × 0.20) + (S_gov × 0.20)
```

Each dimension produces a score of 0-100. Final total is also 0-100.

### Score Interpretation

| Range | Category | Description |
|---|---|---|
| 80 – 100 | 🟢 **Healthy** | Business running well, ready to scale |
| 60 – 79 | 🟡 **Fairly Healthy** | Some areas need improvement |
| 40 – 59 | 🟠 **Needs Attention** | Significant issues detected |
| 0 – 39 | 🔴 **Critical** | Immediate intervention required |

---

### Dimension 1: Financial Health (S_fin — Weight 35%)

| Sub-Indicator | Weight | Options & Score | Basis |
|---|---|---|---|
| **Account Separation** (Business vs Personal) | 25% | Separated = 100 · Mixed = 0 (**→ Red Flag P0**) | Basic accounting best practice; KUR requirement |
| **Financial Record-Keeping** | 25% | Software/ERP = 100 · Spreadsheet = 75 · Manual book = 40 · None = 0 | BSC internal financial process |
| **Cash Runway** | 30% | ≥3 months = 100 · 1-2.9 months = 60 · <1 month = 20 (**→ Red Flag P0**) | `Available Cash ÷ Monthly OPEX` |
| **Gross Profit Margin** | 20% | Healthy (≥30% F&B, ≥20% Retail) = 100 · Thin/unknown = 20 | Industry benchmarks (BPS, LPPI) |

```
S_fin = (Account × 0.25) + (Recording × 0.25) + (Runway × 0.30) + (GPM × 0.20)
```

### Dimension 2: Operations & Efficiency (S_ops — Weight 25%)

| Sub-Indicator | Weight | Options & Score | Basis |
|---|---|---|---|
| **SOP Documentation** | 35% | All written & followed = 100 · Partial = 50 · None = 10 | BSC Internal Process |
| **Owner Dependency Index** | 35% | Can leave 1 month = 100 · Max 1 week = 50 · 2 days = 0 (**→ Red Flag P1**) | Scalability & continuity |
| **Inventory/Waste Tracking** | 30% | Regular stock opname = 100 · Purchase records only = 40 · None = 0 | Supply chain management |

```
S_ops = (SOP × 0.35) + (OwnerDep × 0.35) + (Inventory × 0.30)
```

### Dimension 3: Market & Customer (S_mkt — Weight 20%)

| Sub-Indicator | Weight | Options & Score | Basis |
|---|---|---|---|
| **Revenue Concentration Risk** | 50% | No client >40% revenue = 100 · Client ≥50% = 30 | Revenue dependency analysis |
| **Sales Channels** | 25% | Multi-channel = 100 · Single-channel = 50 | Channel diversification |
| **Repeat Orders / Retention** | 25% | Measured = 100 · One-off transactions = 40 | Customer Lifetime Value |

```
S_mkt = (Concentration × 0.50) + (Channels × 0.25) + (Retention × 0.25)
```

### Dimension 4: Governance & Human Capital (S_gov — Weight 20%)

| Sub-Indicator | Weight | Options & Score | Basis |
|---|---|---|---|
| **Basic Licensing** | 50% | NIB + Sectoral Cert = 100 · NIB only = 60 · None = 10 | PP 7/2021; KUR & procurement access |
| **HR Management** | 50% | Job desc + incentives = 100 · Basic salary only = 50 | BSC Learning & Growth |

```
S_gov = (Licensing × 0.50) + (HR × 0.50)
```

---

### Red Flag System (Rule-Based, Not AI)

Red flags are detected **deterministically before AI is called**:

| Priority | Trigger Condition | Label | Impact |
|---|---|---|---|
| **P0** (Critical) | Mixed accounts **OR** Cash runway < 1 month | 🔴 Liquidity Danger | Bankruptcy risk in near term |
| **P1** (Bottleneck) | Owner dependency ≤ 2 working days | 🟠 Operational Bottleneck | Cannot scale up |
| **P2** (Compliance) | Operating > 1 year without NIB | 🟡 Legal Risk | Vulnerable to raids, no KUR access |

---

## 🤖 AI's Role (Transparent Boundaries)

| ✅ AI Does | ❌ AI Does NOT |
|---|---|
| Explain *why* problems occur in user's industry context | Calculate scores (hardcoded formula) |
| Generate 30-day Action Plan (Week 1-4) | Determine red flags (rule-based) |
| Map findings to relevant regulations | Determine business category (PP 7/2021) |
| Provide analogies & explanations for MSME owners | Modify weights/formulas |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js (React) + Vanilla CSS |
| Scoring Engine | Pure JavaScript (deterministic, testable) |
| AI Layer | Google Gemini API |
| Deployment | Vercel (initial) → VPS (later) |
| Version Control | GitHub |
| Documentation | Notion |

---

## 📚 References

1. **Kaplan, R.S. & Norton, D.P. (1996)**. *The Balanced Scorecard: Translating Strategy into Action*. Harvard Business School Press.
2. **PP No. 7 Tahun 2021** — Kemudahan, Pelindungan, dan Pemberdayaan Koperasi dan UMKM.
3. **SAK EMKM (IAI)** — Standar Akuntansi Keuangan untuk Entitas Mikro, Kecil, dan Menengah.
4. **Systematic Literature Review (2022-2026)**: "Balanced Scorecard for SMEs" — various academic journals.

---

## 📄 License

TBD
