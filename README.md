<div align="center">

# ◈ CATALYST

### Experiment Intelligence Platform

*A portfolio-grade A/B testing analytics dashboard built in Power BI*

[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com)
[![DAX](https://img.shields.io/badge/DAX-60%2B%20Measures-C8956C?style=for-the-badge)](docs/dax_measures.md)
[![Theme](https://img.shields.io/badge/Theme-Dark%20Espresso-1E1714?style=for-the-badge)](theme/Catalyst.json)

---

*Did the new landing page increase conversions? Analyzing 290,585 users across 3 markets to find out.*

</div>

---

## 📸 Dashboard Preview

> Add screenshots here after building the dashboard

| Page | Description |
|------|-------------|
| ![Command Center](screenshots/01_command_center.png) | **Command Center** — Executive summary with verdict, KPIs, and conversion comparison |
| ![Experiment](screenshots/03_experiment_pulse.png) | **Experiment Pulse** — A/B test methodology validation and result deep-dive |
| ![Geography](screenshots/04_geographic_lens.png) | **Geographic Lens** — Country-level segmentation and lift analysis |
| ![Deep Dive](screenshots/05_deep_dive.png) | **Deep Dive** — Hypothesis explorer and business impact quantification |
| ![Strategic Brief](screenshots/06_strategic_brief.png) | **Strategic Brief** — Final recommendation and next steps |

---

## 🎯 Project Overview

### The Business Question

> An e-commerce company ran an A/B test on their landing page with ~290K users. Should they launch the new page?

### What I Built

A 5-page interactive Power BI dashboard that:

- **Analyzes** a real A/B testing dataset (290,585 users, 3 countries)
- **Cleans** data quality issues (3,893 mismatched rows, duplicate users)
- **Computes** 60+ DAX measures organized into 8 display folders
- **Visualizes** experiment results with a custom dark espresso theme
- **Recommends** a business decision backed by data

### The Answer

**⛔ Do NOT launch the new page.** The treatment group converted at 11.88% vs the control's 12.04% — a -0.16 percentage point decline. No country segment showed meaningful improvement.

---

## 🏗️ Architecture

### Data Pipeline

```
ab_data.csv (294K rows)     countries.csv (290K rows)
        │                            │
        ▼                            ▼
   Power Query                  Power Query
   • Remove mismatches          • Remove duplicates
   • Deduplicate                • Add display labels
   • Add computed columns       • Add sort order
        │                            │
        └───────── Join on ──────────┘
                  user_id
                    │
                    ▼
              Star Schema
         (Fact + Dimension)
                    │
                    ▼
           60+ DAX Measures
          (8 Display Folders)
                    │
                    ▼
         5-Page Dashboard
         + 3 Tooltip Pages
```

### Dashboard Pages

| # | Page | Story | Key Visual |
|---|------|-------|------------|
| 1 | **Command Center** | "Here's the result" | KPI cards + verdict banner |
| 2 | **Experiment Pulse** | "Is the result trustworthy?" | Scorecard + sample balance |
| 3 | **Geographic Lens** | "Any hidden winners?" | Country matrix + lift chart |
| 4 | **Deep Dive** | "Why did this happen?" | Hypothesis explorer + what-if |
| 5 | **Strategic Brief** | "What should we do?" | Recommendation + next steps |

### DAX Measure Organization

| Folder | Count | Purpose |
|--------|-------|---------|
| 📊 Core Metrics | 6 | Total users, conversions, rates |
| 🔬 Group Analysis | 10 | Control vs treatment breakdown |
| 📈 Lift Analysis | 9 | Absolute/relative lift, indicators |
| 🌍 Country Analysis | 7 | Geographic segmentation |
| 💬 Tooltip Measures | 15 | Rich hover tooltip content |
| 🧮 What-If Analysis | 5 | Business impact scenarios |
| 🏷️ Dynamic Labels | 6 | Responsive titles, verdicts |
| 🔧 Utility | 4 | Formatting helpers |

---

## 🎨 Design System

### Theme: Dark Espresso + Caramel Gold

A custom Power BI theme inspired by premium SaaS dashboards (Linear, Stripe, Vercel).

| Element | Color | Hex |
|---------|-------|-----|
| Canvas | Espresso Black | `#131010` |
| Surface | Dark Mocha | `#1E1714` |
| Accent | Caramel Gold | `#C8956C` |
| Text | Warm White | `#F5EDE4` |
| Positive | Sage Green | `#7FB069` |
| Negative | Muted Red | `#D4574E` |

### Design Principles

- **No default Power BI look** — custom navigation, containers, typography
- **Information hierarchy** — largest elements = most important data
- **Conditional coloring** — green/red for directional metrics
- **Rich tooltips** — mini-dashboards on hover with 8+ data points
- **Analyst voice** — every page includes written interpretation

---

## 📂 Project Structure

```
Catalyst/
├── README.md                  ← You are here
├── Catalyst.pbix              ← Power BI dashboard (not in repo)
├── theme/
│   └── Catalyst.json          ← Custom Power BI theme
├── docs/
│   ├── dashboard_architecture.md
│   ├── dax_measures.md
│   ├── power_query.md
│   └── build_guide.md
├── assets/
│   ├── icons/                 ← SVG icons for navigation + KPIs
│   └── backgrounds/           ← Background assets
├── data/                      ← CSV files (not in repo)
│   └── ecommerce_ab_testing_2022_dataset1/
│       ├── ab_data.csv
│       └── countries.csv
└── screenshots/               ← Dashboard screenshots
```

---

## 🔑 Key Findings

| Finding | Detail |
|---------|--------|
| **Overall lift** | -0.16 pp (treatment underperformed) |
| **Relative change** | -1.31% decline |
| **Best market** | UK (+0.12 pp, but not actionable) |
| **Worst market** | Canada (-0.69 pp, small sample) |
| **Sample balance** | Perfect 50/50 split (Δ = 37 users) |
| **Data quality** | 98.7% clean (removed 1.3% bad rows) |
| **Recommendation** | ⛔ Do NOT launch the new page |

---

## 🛠️ Technical Skills Demonstrated

- **Power BI Desktop** — Report design, custom themes, bookmarks, drill-through
- **DAX** — CALCULATE, DIVIDE, SWITCH, HASONEVALUE, ALL, conditional logic, formatting
- **Power Query (M)** — Data cleaning, type casting, column enrichment, deduplication
- **Data Modeling** — Star schema, Many-to-One relationships
- **A/B Testing** — Experiment analysis, lift calculation, sample balance validation
- **Data Storytelling** — Narrative dashboards, analyst interpretation, business recommendations
- **UX/Design** — Custom theme, typography hierarchy, conditional formatting, hover tooltips

---

## 📊 Dataset

| File | Rows | Columns |
|------|------|---------|
| `ab_data.csv` | 294,480 (raw) → 290,585 (clean) | user_id, group, landing_page, converted |
| `countries.csv` | 290,586 → 290,585 | user_id, country |

Source: E-commerce A/B Testing Dataset (2022)

---

## 📄 License

This project is for portfolio and educational purposes.

---

<div align="center">

**Built with** ☕ **and** Power BI

</div>
