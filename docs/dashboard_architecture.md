# CATALYST — Dashboard Architecture & Visual Design Bible

---

## Design System Reference

### Color Tokens

| Token | Hex | Usage |
|-------|-----|-------|
| `--bg-canvas` | `#131010` | Page background |
| `--bg-surface` | `#1E1714` | Card / panel background |
| `--bg-elevated` | `#2C1F1A` | Hover states, nav buttons, elevated panels |
| `--bg-subtle` | `#231A15` | Table alternating rows, subtle containers |
| `--border` | `#3D2B22` | Dividers, card borders (only when needed) |
| `--border-emphasis` | `#5C4033` | Active nav indicator, focus rings |
| `--accent-primary` | `#C8956C` | KPI values, active state, brand accent |
| `--accent-gold` | `#D4A76A` | Highlight, headers, special callouts |
| `--text-primary` | `#F5EDE4` | Primary content, KPI numbers |
| `--text-secondary` | `#BFA98E` | Subtitles, descriptions |
| `--text-muted` | `#8A7560` | Labels, axis text, metadata |
| `--text-cream` | `#E8C9A0` | Data labels on charts |
| `--positive` | `#7FB069` | ↑ Positive lift, good indicators |
| `--negative` | `#D4574E` | ↓ Negative lift, poor indicators |
| `--neutral` | `#A8845E` | Neutral / inconclusive indicators |

### Typography Scale

| Element | Font | Size | Weight | Color |
|---------|------|------|--------|-------|
| Page Title | Segoe UI | 22pt | Semibold | `--text-primary` |
| Page Subtitle | Segoe UI | 11pt | Regular | `--text-muted` |
| Section Header | Segoe UI | 13pt | Semibold | `--text-secondary` |
| KPI Value | Segoe UI | 28pt | Bold | `--text-primary` |
| KPI Label | Segoe UI | 9pt | Regular | `--text-muted` |
| KPI Comparison | Segoe UI | 9pt | Regular | `--positive` / `--negative` |
| Chart Data Label | Segoe UI | 10pt | Semibold | `--text-cream` |
| Axis Label | Segoe UI | 10pt | Regular | `--text-muted` |
| Table Header | Segoe UI | 11pt | Semibold | `--accent-primary` |
| Table Cell | Segoe UI | 11pt | Regular | `--text-cream` |
| Body Text | Segoe UI | 11pt | Regular | `--text-secondary` |
| Nav Button | Segoe UI | 10pt | Semibold | `--text-cream` |

### Spacing System

| Element | Value |
|---------|-------|
| Page padding (all sides) | 24px |
| Card inner padding | 16px |
| Card corner radius | 10px |
| Gap between cards (horizontal) | 12px |
| Gap between rows (vertical) | 12px |
| Section title to content | 8px |
| Nav bar height | 48px |

### Canvas Dimensions

| Property | Value |
|----------|-------|
| Page type | Custom |
| Width | 1280 px |
| Height | 720 px |
| Aspect ratio | 16:9 |

---

## Navigation Design

### Top Navigation Bar

A persistent navigation bar at the **top** of every page, built from **Shapes + Buttons + Textboxes**.

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  ◈ CATALYST                   ❶ ❷ ❸ ❹ ❺                              ◀ PREV  NEXT ▶ │
│  Experiment Intelligence      ■  ○  ○  ○  ○                                         │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

**Dimensions:** Full width (1280px) × 48px, position: `x=0, y=0`

#### Components

| Component | Position | Size | Details |
|-----------|----------|------|---------|
| Nav Background | `x=0, y=0` | `1280×48` | Rounded rectangle, fill `#1E1714`, no border |
| Logo Text "◈ CATALYST" | `x=24, y=8` | `200×20` | Textbox, 13pt Segoe UI Semibold, `#C8956C` |
| Subtitle "Experiment Intelligence" | `x=24, y=28` | `200×16` | Textbox, 8pt Segoe UI, `#8A7560` |
| Page Button 1 "Command Center" | `x=340, y=8` | `140×32` | Button → Page Navigation, fill `#2C1F1A` when inactive, `#3D2B22` when active |
| Page Button 2 "Experiment" | `x=488, y=8` | `120×32` | Same pattern |
| Page Button 3 "Geography" | `x=616, y=8` | `120×32` | Same pattern |
| Page Button 4 "Deep Dive" | `x=744, y=8` | `110×32` | Same pattern |
| Page Button 5 "Strategic Brief" | `x=862, y=8` | `130×32` | Same pattern |
| Active Indicator | Below active button | `width×3` | Rounded rectangle, fill `#C8956C` |
| Prev Button "◀" | `x=1130, y=10` | `50×28` | Button → Page Navigation |
| Next Button "▶" | `x=1190, y=10` | `50×28` | Button → Page Navigation |

#### Button States
- **Inactive:** Fill `#2C1F1A`, text `#8A7560`
- **Active (current page):** Fill `#3D2B22`, text `#C8956C`, gold underline bar
- **Hover:** Defined by button's default hover in Power BI (lighter shade)

#### How to Build
1. Insert a **Rounded Rectangle** shape (full width, `#1E1714`, no border, corner radius 0)
2. Add **Blank Buttons** for each page name
3. Set each button's **Action → Type: Page Navigation → Destination: [page name]**
4. For "active state": On each page, change that page's button fill to `#3D2B22` and text to `#C8956C`. Add a thin rectangle underneath in `#C8956C`
5. Add a logo textbox (left-aligned)
6. Add Prev/Next buttons with left/right arrow icons and Page Navigation actions

---

## Page Content Area

After the 48px nav bar, the content area is:
- **Position:** `x=24, y=60`
- **Available:** `1232 × 648` (with 24px padding on all sides, 12px gap below nav)

---

## Page 1: COMMAND CENTER
**Story:** *"Here's what happened — in one glance."*

### Purpose
The executive summary. A stakeholder should understand the experiment result within 5 seconds of seeing this page. It answers: **"Did the new page win?"**

### Layout Blueprint

```
y=60  ┌──────────────────────────────── HEADER AREA ────────────────────────────────┐
      │  "A/B Test Results"                                                         │
      │  "Landing Page Experiment · 290,585 Users · 3 Markets · Jan 2022"           │
y=110 └──────────────────────────────────────────────────────────────────────────────┘

y=118 ┌───────────── VERDICT BANNER ─────────────────────────────────────────────────┐
      │  ⛔  NEW PAGE DID NOT OUTPERFORM   │   Lift: -0.16 pp  │  Relative: -1.31%  │
y=160 └──────────────────────────────────────────────────────────────────────────────┘

y=172 ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐
      │ TOTAL  │ │ TOTAL  │ │OVERALL │ │CONTROL │ │TREAT   │ │ABSOLUTE│
      │ USERS  │ │ CONV   │ │ RATE   │ │  RATE  │ │  RATE  │ │  LIFT  │
      │290,585 │ │ 34,753 │ │ 11.96% │ │ 12.04% │ │ 11.88% │ │-0.16pp │
      │        │ │        │ │        │ │        │ │        │ │  ↓1.3% │
y=272 └────────┘ └────────┘ └────────┘ └────────┘ └────────┘ └────────┘

y=284 ┌────────────── LEFT PANEL ──────────┐  ┌────── RIGHT PANEL ──────────────────┐
      │  CONVERSION COMPARISON             │  │  COUNTRY SNAPSHOT                    │
      │  ┌─────────────────────────────┐   │  │  ┌──────────────────────────────┐   │
      │  │  ● Control ████████ 12.04%  │   │  │  │ US  ████████████████ 11.95% │   │
      │  │  ○ Treat.  ███████▌ 11.88%  │   │  │  │ UK  ████████████████ 12.06% │   │
      │  │                             │   │  │  │ CA  ██████████████▌  11.53% │   │
      │  └─────────────────────────────┘   │  │  └──────────────────────────────┘   │
y=460 └────────────────────────────────────┘  └────────────────────────────────────┘

y=472 ┌────────────── SAMPLE BALANCE INDICATOR ──────────────────────────────────────┐
      │  ✅ Sample Balanced   Control: 145,274 (50.0%)  │  Treatment: 145,311 (50.0%) │
y=510 └──────────────────────────────────────────────────────────────────────────────┘

y=518 ┌─── SLICER ───┐
      │  Country ▾    │
y=550 └───────────────┘
```

### Component Specifications

| # | Component | Visual Type | Position | Size | WHY This Visual |
|---|-----------|------------|----------|------|-----------------|
| 1 | Title "A/B Test Results" | Textbox | `x=24, y=60` | `600×24` | Textbox gives full typographic control. 22pt Segoe UI Semibold, `#F5EDE4` |
| 2 | Subtitle | Textbox | `x=24, y=86` | `600×18` | Context line. 11pt, `#8A7560`. Includes user count and market scope |
| 3 | Verdict Banner | Rounded Rectangle + Textboxes | `x=24, y=118` | `1232×42` | A red-tinted banner (`#2A1515`) that immediately communicates the result. Contains icon + verdict text + lift metrics side by side |
| 4 | KPI Cards (×6) | Card visuals inside rounded rectangles | `x=24, y=172` | Each: `196×100` with 8px gaps | Cards are the fastest way to scan metrics. Each card sits inside a rounded rectangle shape container for the premium look |
| 5 | Conversion Comparison | Clustered Bar Chart (horizontal) | `x=24, y=284` | `600×176` | **WHY:** Horizontal bars make label reading natural (left-to-right). Two bars side-by-side with data labels enable instant comparison. Alternatives: Bullet chart (too complex for exec), gauge (can't compare two values easily), column chart (labels truncate vertically) |
| 6 | Country Snapshot | Clustered Bar Chart (horizontal) | `x=636, y=284` | `620×176` | **WHY:** Consistent with the left panel. Horizontal bars with country names on y-axis. Data labels show conversion rates. Simple and scannable for 3 countries |
| 7 | Sample Balance Bar | Rounded Rectangle + Textboxes | `x=24, y=472` | `1232×38` | A subtle confidence indicator. Green accent. Shows the split is clean. Builds stakeholder trust in the data |
| 8 | Country Slicer | Slicer (Dropdown) | `x=24, y=518` | `200×36` | Allows filtering the entire page by country for drill-down |

### KPI Card Design (Each Card)

```
┌──────────────────────────┐
│  ○ icon                   │  ← 9pt, `#8A7560`
│  290,585                  │  ← 28pt Segoe UI Bold, `#F5EDE4`
│  Total Users              │  ← 9pt, `#8A7560`
│  ▬▬▬ vs baseline          │  ← 9pt, conditional color
└──────────────────────────┘
```

For each KPI card:
1. Place a **Rounded Rectangle** shape: `196×100`, fill `#1E1714`, corner radius 10
2. Place a **Card visual** on top with the measure, transparent background
3. Place a **Textbox** below the card for the comparison line (use conditional formatting or a measure that outputs text)

| KPI | Main Measure | Comparison Line | Conditional Color |
|-----|-------------|-----------------|-------------------|
| Total Users | `[Total Users]` | "50/50 split" | `--text-muted` |
| Total Conversions | `[Total Conversions]` | "of 290,585 users" | `--text-muted` |
| Overall Rate | `[Conversion Rate %]` | "baseline benchmark" | `--accent-primary` |
| Control Rate | `[Control CR Display]` | "old_page" | `--text-muted` |
| Treatment Rate | `[Treatment CR Display]` | "new_page" | `--text-muted` |
| Absolute Lift | `[Absolute Lift Display]` | "↓ 1.31% relative" | `--negative` |

---

## Page 2: EXPERIMENT PULSE
**Story:** *"Let's look at the experiment mechanics. Was the test fair? What do the numbers really say?"*

### Purpose
Deep-dive into the A/B test methodology and results. Answers: **"Is this result trustworthy?"** and **"How big is the difference?"**

### Layout Blueprint

```
y=60  ┌──────────────────────────────── HEADER ─────────────────────────────────────┐
      │  "Experiment Results"                                                        │
      │  "Evaluating conversion impact of old_page vs new_page"                      │
y=110 └──────────────────────────────────────────────────────────────────────────────┘

y=118 ┌──────────────── LEFT COLUMN ──────────┐  ┌─────── RIGHT COLUMN ────────────┐
      │                                        │  │                                  │
      │  CONVERSION RATE BY GROUP              │  │  EXPERIMENT SCORECARD            │
      │  ┌──────────────────────────────┐      │  │  ┌──────────────────────────┐   │
      │  │                              │      │  │  │ Metric     Ctrl   Treat  │   │
      │  │   Control  █████████ 12.04%  │      │  │  │ ──────     ─────  ─────  │   │
      │  │                              │      │  │  │ Users      145K   145K   │   │
      │  │   Treat.   ████████▌ 11.88%  │      │  │  │ Conversns  17.5K  17.3K  │   │
      │  │                              │      │  │  │ Conv Rate  12.04  11.88  │   │
      │  │   ◆ Reference: 12.00%        │      │  │  │ Lift       —      -0.16  │   │
      │  └──────────────────────────────┘      │  │  └──────────────────────────┘   │
      │                                        │  │                                  │
y=340 └────────────────────────────────────────┘  └──────────────────────────────────┘

y=352 ┌──────────────── LEFT COLUMN ──────────┐  ┌─────── RIGHT COLUMN ────────────┐
      │                                        │  │                                  │
      │  CONVERSION FUNNEL                     │  │  SAMPLE BALANCE                  │
      │  ┌──────────────────────────────┐      │  │  ┌──────────────────────────┐   │
      │  │  Control                     │      │  │  │                          │   │
      │  │  ██████████████████ 145,274  │      │  │  │    ┌────────────────┐    │   │
      │  │  █████              17,489   │      │  │  │    │  50.0%  50.0%  │    │   │
      │  │                              │      │  │  │    │ Ctrl    Treat  │    │   │
      │  │  Treatment                   │      │  │  │    └────────────────┘    │   │
      │  │  ██████████████████ 145,311  │      │  │  │                          │   │
      │  │  ████▌              17,264   │      │  │  │  ✅ Balanced (Δ=37)      │   │
      │  └──────────────────────────────┘      │  │  └──────────────────────────┘   │
      │                                        │  │                                  │
y=540 └────────────────────────────────────────┘  └──────────────────────────────────┘

y=552 ┌─── KEY TAKEAWAY ────────────────────────────────────────────────────────────┐
      │  "The experiment was properly randomized with a balanced 50/50 split.        │
      │   The treatment group showed a marginal -0.16pp decline in conversion rate.  │
      │   This difference is not practically significant."                           │
y=620 └──────────────────────────────────────────────────────────────────────────────┘
```

### Component Specifications

| # | Component | Visual Type | Size | WHY |
|---|-----------|------------|------|-----|
| 1 | Conversion Rate by Group | **Clustered Bar (Horizontal)** + Reference Line | `600×210` | **WHY:** The single most important comparison visual. Horizontal bars with data labels. Reference line at 12.00% (overall average) gives context. Alternatives: Gauge (can't show both groups), pie (meaningless for rates), column (worse for 2-item comparison) |
| 2 | Experiment Scorecard | **Matrix / Table** | `600×210` | **WHY:** Tables are the most efficient way to present multiple metrics for two groups side-by-side. Color-code the lift row. This is what analysts use in real experiment reports |
| 3 | Conversion Funnel | **100% Stacked Bar (Horizontal)** | `600×176` | **WHY:** Shows the conversion rate as a visual proportion. Two stacked bars (one per group) make it obvious the segments are nearly identical. Alternative: Funnel chart (only shows one path, can't compare groups) |
| 4 | Sample Balance | **Donut Chart** | `600×176` | **WHY:** The donut chart is the clearest way to show a 50/50 split. The nearly-equal halves immediately communicate "balanced." Center label shows the exact ratio. Below it, a text confirmation. Alternative: Bar chart (overkill for a binary split) |
| 5 | Key Takeaway | Textbox in rounded rectangle | `1232×68` | Analyst's interpretation. Makes the dashboard tell a story, not just show data |

---

## Page 3: GEOGRAPHIC LENS
**Story:** *"Does geography matter? Are there any hidden winners?"*

### Purpose
Country segmentation analysis. Answers: **"Does the new page work better in any specific market?"**

### Layout Blueprint

```
y=60  ┌──────────────────────────────── HEADER ─────────────────────────────────────┐
      │  "Geographic Segmentation"                                                   │
      │  "Conversion performance across US, UK, and Canada"                          │
y=110 └──────────────────────────────────────────────────────────────────────────────┘

y=118 ┌──────┐  ┌──────┐  ┌──────┐
      │  US  │  │  UK  │  │  CA  │   ← 3 country KPI cards
      │11.95%│  │12.06%│  │11.53%│
      │ 70%  │  │ 25%  │  │  5%  │
y=208 └──────┘  └──────┘  └──────┘

y=220 ┌──────────────── LEFT ──────────────────┐  ┌─────── RIGHT ──────────────────┐
      │                                         │  │                                 │
      │  LIFT BY COUNTRY                        │  │  USER DISTRIBUTION              │
      │  ┌────────────────────────────────┐     │  │  ┌───────────────────────┐      │
      │  │  US  ██████▌  -0.22 pp    ↓   │     │  │  │                       │      │
      │  │  UK  ▌         +0.12 pp   ↑   │     │  │  │   ◕ US 70.1%         │      │
      │  │  CA  █████████ -0.69 pp   ↓   │     │  │  │     UK 24.9%         │      │
      │  └────────────────────────────────┘     │  │  │     CA  5.0%         │      │
      │                                         │  │  └───────────────────────┘      │
y=400 └─────────────────────────────────────────┘  └─────────────────────────────────┘

y=412 ┌────────────────── FULL WIDTH TABLE ──────────────────────────────────────────┐
      │  COUNTRY PERFORMANCE MATRIX                                                  │
      │  ┌────────────────────────────────────────────────────────────────────────┐  │
      │  │ Country │ Ctrl Users │ Treat Users │ Ctrl CR │ Treat CR │ Lift  │ Rec │  │
      │  │ US      │ 101,716    │ 101,904     │ 12.06%  │ 11.85%   │-0.22  │ ⛔  │  │
      │  │ UK      │  36,360    │  36,106     │ 12.00%  │ 12.12%   │+0.12  │ 🔍  │  │
      │  │ CA      │   7,198    │   7,301     │ 11.88%  │ 11.19%   │-0.69  │ ⛔  │  │
      │  └────────────────────────────────────────────────────────────────────────┘  │
y=560 └──────────────────────────────────────────────────────────────────────────────┘

y=572 ┌─── ANALYST NOTE ────────────────────────────────────────────────────────────┐
      │  "UK shows a slight positive signal (+0.12 pp) but with only 72K users and   │
      │   a 0.12 pp lift, it's not actionable. Canada's -0.69 pp is notable but the  │
      │   sample (14.5K) is too small for confidence."                               │
y=630 └──────────────────────────────────────────────────────────────────────────────┘

y=638 ┌─── SLICER ───┐
      │  Country ▾    │
y=670 └───────────────┘
```

### Component Specifications

| # | Component | Visual Type | Size | WHY |
|---|-----------|------------|------|-----|
| 1 | Country KPI Cards (×3) | Card + Rounded Rectangle | Each: `400×90` | Quick per-country snapshot. Shows conversion rate, user share, and a spark indicator |
| 2 | Lift by Country | **Bar Chart (Horizontal)** with conditional colors | `600×180` | **WHY:** Bars pointing left (negative) and right (positive) from zero make lift direction instantly clear. Color-code: green for positive, red for negative. Alternative: Column chart (less intuitive for diverging data) |
| 3 | User Distribution | **Donut Chart** | `600×180` | **WHY:** Shows proportional representation. The dominant US segment is immediately visible. This context matters — Canada's decline is from a tiny 5% slice |
| 4 | Country Performance Matrix | **Table** with conditional formatting | `1232×148` | **WHY:** The complete view. One row per country with all metrics. Conditional formatting on Lift column (green/red). Recommendation column adds the analyst voice |
| 5 | Analyst Note | Textbox in rounded rectangle | `1232×58` | Senior analyst interpretation. Makes the geographic story complete |

---

## Page 4: DEEP DIVE
**Story:** *"Let's understand WHY the results look this way, and what hypotheses we can form."*

### Purpose
The analytical deep-dive. Answers: **"What's driving the result?"** and **"What should we test next?"**

### Layout Blueprint

```
y=60  ┌──────────────────────────────── HEADER ─────────────────────────────────────┐
      │  "Analytical Deep Dive"                                                      │
      │  "Understanding conversion drivers and forming hypotheses"                    │
y=110 └──────────────────────────────────────────────────────────────────────────────┘

y=118 ┌──────────────── LEFT ──────────────────┐  ┌─────── RIGHT ──────────────────┐
      │                                         │  │                                 │
      │  LANDING PAGE PERFORMANCE              │  │  CONVERSION BY GROUP × COUNTRY  │
      │  ┌────────────────────────────────┐     │  │  ┌───────────────────────┐      │
      │  │                                │     │  │  │                       │      │
      │  │  old_page  ███████████  12.04% │     │  │  │  [Clustered Column    │      │
      │  │  new_page  ██████████▌  11.88% │     │  │  │   Chart: 2 bars per  │      │
      │  │                                │     │  │  │   country: ctrl,treat]│      │
      │  │  ◇ Difference: -0.16 pp        │     │  │  │                       │      │
      │  └────────────────────────────────┘     │  │  └───────────────────────┘      │
      │                                         │  │                                 │
y=330 └─────────────────────────────────────────┘  └─────────────────────────────────┘

y=342 ┌──────────────── LEFT ──────────────────┐  ┌─────── RIGHT ──────────────────┐
      │                                         │  │                                 │
      │  HYPOTHESIS EXPLORER                   │  │  WHAT-IF ANALYSIS               │
      │  ┌────────────────────────────────┐     │  │  ┌───────────────────────┐      │
      │  │  ① Design didn't resonate     │     │  │  │ If treatment matched  │      │
      │  │    The new page may not have   │     │  │  │ control CR at 12.04%: │      │
      │  │    addressed actual UX pain    │     │  │  │                       │      │
      │  │                                │     │  │  │ Missed conversions:   │      │
      │  │  ② Novelty bias absent        │     │  │  │ 230 additional users  │      │
      │  │    Unlike A/B tests where new  │     │  │  │                       │      │
      │  │    novelty boosts engagement   │     │  │  │ If 1pp improvement:   │      │
      │  │                                │     │  │  │ +1,453 conversions    │      │
      │  │  ③ Sample size sufficient     │     │  │  │                       │      │
      │  │    290K users is well above    │     │  │  │ Revenue impact at     │      │
      │  │    minimum detectable effect   │     │  │  │ $10/conv: +$14,530    │      │
      │  └────────────────────────────────┘     │  │  └───────────────────────┘      │
      │                                         │  │                                 │
y=560 └─────────────────────────────────────────┘  └─────────────────────────────────┘

y=572 ┌─── GROUP SLICER ───┐  ┌─── COUNTRY SLICER ───┐
      │  Group ▾            │  │  Country ▾             │
y=604 └─────────────────────┘  └────────────────────────┘
```

### Component Specifications

| # | Component | Visual Type | Size | WHY |
|---|-----------|------------|------|-----|
| 1 | Landing Page Performance | **Bar Chart (Horizontal)** | `600×200` | **WHY:** Direct comparison of old_page vs new_page conversion rates. The simplest, most honest visual for a two-item comparison. Reference line optional |
| 2 | Group × Country | **Clustered Column Chart** | `600×200` | **WHY:** This is the interaction analysis visual — seeing whether the treatment effect varies by country. Columns work well for 3 categories × 2 series. Each country pair shows control vs treatment side by side |
| 3 | Hypothesis Explorer | Textbox with rich formatting inside rounded rectangle | `600×210` | **WHY:** This is what separates a junior dashboard from a senior one. Hypotheses show analytical thinking. Numbered for readability. Not a chart — it's interpretation |
| 4 | What-If Analysis | Card visuals + Textboxes inside rounded rectangle | `600×210` | **WHY:** Business impact quantification. Even though the experiment lost, showing "what would a 1pp improvement mean" demonstrates business thinking. Uses measures like `[Missed Conversions]` and `[Revenue Impact]` |

---

## Page 5: STRATEGIC BRIEF
**Story:** *"Here's the recommendation. Here's what to do next."*

### Purpose
The decision page. Answers: **"What should we do?"** — the only question executives ultimately care about.

### Layout Blueprint

```
y=60  ┌──────────────────────────────── HEADER ─────────────────────────────────────┐
      │  "Strategic Recommendation"                                                  │
      │  "Experiment conclusion and next steps"                                      │
y=110 └──────────────────────────────────────────────────────────────────────────────┘

y=118 ┌──────────────────────── RECOMMENDATION PANEL ───────────────────────────────┐
      │                                                                              │
      │   ⛔  RECOMMENDATION: DO NOT LAUNCH THE NEW PAGE                            │
      │                                                                              │
      │   The new landing page showed a -0.16 pp decline in conversion rate          │
      │   compared to the existing page. With 290,585 users and a balanced           │
      │   50/50 split, the experiment was properly designed. The result is            │
      │   clear: the new page does not improve conversions.                          │
      │                                                                              │
y=230 └──────────────────────────────────────────────────────────────────────────────┘

y=242 ┌──────── KEY FINDINGS ───────────┐  ┌────── NEXT STEPS ─────────────────────┐
      │                                  │  │                                        │
      │  ① Overall lift is negative     │  │  ① Keep the current landing page     │
      │     -0.16 pp / -1.31% relative  │  │     No deployment needed               │
      │                                  │  │                                        │
      │  ② No country showed a          │  │  ② Conduct UX research                │
      │     meaningful improvement       │  │     Understand what users actually     │
      │                                  │  │     want changed on the page           │
      │  ③ UK showed a slight positive  │  │                                        │
      │     signal worth investigating   │  │  ③ Redesign with hypothesis           │
      │                                  │  │     Test a more differentiated         │
      │  ④ Sample balance was perfect   │  │     design grounded in user feedback   │
      │     Result is trustworthy        │  │                                        │
      │                                  │  │  ④ Consider UK-specific test          │
      │  ⑤ Experiment design is sound   │  │     The slight positive signal may     │
      │     Proper randomization         │  │     warrant a targeted follow-up       │
      │                                  │  │                                        │
y=470 └──────────────────────────────────┘  └────────────────────────────────────────┘

y=482 ┌──────────────────────── IMPACT SUMMARY ─────────────────────────────────────┐
      │                                                                              │
      │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐                    │
      │  │ AVOIDED  │  │ POTENTIAL│  │EXPERIMENT│  │ DATA     │                    │
      │  │ RISK     │  │ UPSIDE  │  │ COST     │  │ QUALITY  │                    │
      │  │-230 conv │  │+1,453   │  │ ~290K    │  │ 98.7%    │                    │
      │  │saved     │  │if +1pp  │  │ users    │  │ clean    │                    │
      │  └──────────┘  └──────────┘  └──────────┘  └──────────┘                    │
      │                                                                              │
y=588 └──────────────────────────────────────────────────────────────────────────────┘

y=600 ┌──────────────── FOOTER ──────────────────────────────────────────────────────┐
      │  Analysis by: [Your Name]  ·  Platform: CATALYST  ·  Date: January 2022     │
y=640 └──────────────────────────────────────────────────────────────────────────────┘
```

### Component Specifications

| # | Component | Visual Type | Size | WHY |
|---|-----------|------------|------|-----|
| 1 | Recommendation Panel | Rounded Rectangle + Textboxes | `1232×112` | **WHY:** The most important element. Red accent strip on left border. Large, clear verdict text. This is what the VP reads first and maybe last |
| 2 | Key Findings | Textbox in rounded rectangle | `600×228` | **WHY:** Numbered findings are scannable. This format is used in real consulting deliverables (McKinsey, BCG). Not a chart — it's structured narrative |
| 3 | Next Steps | Textbox in rounded rectangle | `600×228` | **WHY:** Actionable recommendations. This is what separates analysis from advice. Each step is concrete and specific |
| 4 | Impact Summary | 4 KPI Cards in rounded rectangle | `1232×106` | **WHY:** Quantifies the decision. "Avoided risk" shows the cost of a wrong launch. "Potential upside" motivates the next experiment. Business-oriented framing |
| 5 | Footer | Textbox | `1232×40` | Professional attribution. Makes the dashboard feel like a produced deliverable |

---

## Tooltip Pages (3 Tooltip Pages)

### Tooltip 1: Country Detail Tooltip

**Size:** 320 × 280 px (Power BI tooltip preset)
**Background:** `#1E1714`
**Trigger:** Assigned to country visuals (bars, map, donut)

```
┌────────────────────────────────────┐
│  🇺🇸 UNITED STATES                 │  ← 13pt, `#C8956C`
│  ─────────────────────────         │
│  Users           203,620          │  ← `#E8C9A0`
│  Conversions      24,342          │
│  Conversion Rate  11.95%          │  ← `#F5EDE4`, bold
│  ─────────────────────────         │
│  Share of Users    70.1%          │  ← `#8A7560`
│  Contribution      70.0%          │
│  ─────────────────────────         │
│  Control CR       12.06%          │
│  Treatment CR     11.85%          │  ← `#D4574E` (negative)
│  Lift             -0.22 pp        │
│  ─────────────────────────         │
│  Signal: ⛔ Decline                │  ← conditional color
└────────────────────────────────────┘
```

**Measures needed:**
- `[Tooltip Country Name]` — dynamic label
- `[Tooltip Country Users]`, `[Tooltip Country Conversions]`, `[Tooltip Country CR]`
- `[Tooltip Country Share]`, `[Tooltip Country Contribution]`
- `[Tooltip Country Control CR]`, `[Tooltip Country Treatment CR]`, `[Tooltip Country Lift]`
- `[Tooltip Country Signal]`

### Tooltip 2: Group Detail Tooltip

**Size:** 320 × 220 px
**Background:** `#1E1714`
**Trigger:** Assigned to group comparison visuals

```
┌────────────────────────────────────┐
│  ● CONTROL GROUP                   │  ← 13pt, `#C8956C`
│  ─────────────────────────         │
│  Sample Size      145,274         │
│  Conversions       17,489         │
│  Conversion Rate   12.04%         │  ← bold
│  ─────────────────────────         │
│  Share of Users    50.0%          │
│  vs Treatment      +0.16 pp      │  ← `#7FB069`
│  ─────────────────────────         │
│  Status: ✅ Baseline               │
└────────────────────────────────────┘
```

### Tooltip 3: KPI Context Tooltip

**Size:** 320 × 200 px
**Background:** `#1E1714`
**Trigger:** Assigned to KPI cards

```
┌────────────────────────────────────┐
│  CONVERSION RATE                   │  ← 13pt, `#C8956C`
│  ─────────────────────────         │
│  Current Value     11.96%         │
│  Control           12.04%         │
│  Treatment         11.88%         │
│  ─────────────────────────         │
│  Absolute Lift     -0.16 pp       │
│  Relative Lift     -1.31%         │
│  ─────────────────────────         │
│  Verdict: Not significant          │
└────────────────────────────────────┘
```

### How to Create Tooltip Pages

1. **Insert → New Page** → Rename to "Tooltip - Country" (etc.)
2. **Format Pane → Page Information → Page type: Tooltip**
3. **Canvas Settings → Size: 320×280** (or custom per tooltip)
4. **Page background:** `#1E1714`
5. Add **Card visuals** and **Textboxes** per the specs above
6. On target visuals: **Format → Tooltip → Type: Report Page → Page: [tooltip name]**

---

## Visual Interaction Map

### Cross-filtering Rules

| Source Visual | Affects | Behavior |
|---------------|---------|----------|
| Country Slicer (all pages) | All visuals on same page | Filter |
| Group Slicer (Page 4) | All visuals on Page 4 | Filter |
| Country KPI Cards (Page 3) | Lift chart, Distribution donut, Matrix | Highlight |
| Conversion Comparison bars (Page 1) | Country Snapshot | None (independent) |
| Country Snapshot bars (Page 1) | Conversion Comparison | None (independent) |
| Lift by Country bars (Page 3) | User Distribution donut | Highlight |
| Group × Country columns (Page 4) | Landing Page bars | Highlight |

### How to Set Interactions

1. Select the source visual
2. Go to **Format → Edit Interactions**
3. For each target visual, choose: **Filter** (funnel icon), **Highlight** (bar icon), or **None** (⊘ icon)

### Drill-through Configuration

| Source Page | Target Page | Drill-through Field | Use Case |
|-------------|-------------|---------------------|----------|
| Page 3 (Geographic Lens) | Page 4 (Deep Dive) | `country` | Right-click a country to see its detailed breakdown |

---

## Bookmark Navigation (Advanced)

### Bookmarks to Create

| Bookmark Name | Page | State | Purpose |
|---------------|------|-------|---------|
| `bk_CommandCenter` | Page 1 | Default view, no filters | Nav button target |
| `bk_Experiment` | Page 2 | Default view | Nav button target |
| `bk_Geography` | Page 3 | Default view | Nav button target |
| `bk_DeepDive` | Page 4 | Default view | Nav button target |
| `bk_Strategic` | Page 5 | Default view | Nav button target |

> [!TIP]
> For the navigation buttons, **Page Navigation** action type is simpler and recommended. Bookmarks are useful if you later want to add animated transitions or toggle visibility of elements.

---

## Conditional Formatting Rules

| Visual | Condition | Format |
|--------|-----------|--------|
| Lift values (everywhere) | Value > 0 | Text color: `#7FB069` |
| Lift values (everywhere) | Value < 0 | Text color: `#D4574E` |
| Lift values (everywhere) | Value = 0 | Text color: `#A8845E` |
| Country Matrix - Lift column | Value > 0 | Background: `#1A2A1A` (dark green tint) |
| Country Matrix - Lift column | Value < 0 | Background: `#2A1515` (dark red tint) |
| Verdict banner (Page 1) | Lift < 0 | Background: `#2A1515` |
| Verdict banner (Page 1) | Lift > 0 | Background: `#1A2A1A` |
| KPI Cards - Absolute Lift | Value < 0 | Value color: `#D4574E` |
| KPI Cards - Absolute Lift | Value > 0 | Value color: `#7FB069` |
