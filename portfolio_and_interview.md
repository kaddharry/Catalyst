# CATALYST — Portfolio, Resume & Interview Preparation

---

## 1. Resume Bullet Points

### Version A — Data Analyst / Business Analyst Resume (Recommended)

> **A/B Test Analytics Dashboard (Power BI)** — Designed and built a 5-page experiment intelligence platform analyzing a 290K-user landing page A/B test across 3 markets. Cleaned and joined datasets using Power Query, developed 60+ DAX measures across 8 organized folders, and created a custom dark-themed dashboard with rich hover tooltips and conditional formatting. Analysis revealed the new page showed a -0.16pp conversion decline, leading to a data-driven recommendation that prevented a losing product change. [GitHub ↗]

### Version B — Shorter (for tight resume space)

> **A/B Test Dashboard (Power BI)** — Built a portfolio-grade analytics dashboard for a 290K-user e-commerce experiment. Developed 60+ DAX measures, custom themes, and interactive tooltips. Identified no conversion improvement from the new landing page (-0.16pp lift), delivering actionable recommendation to stakeholders.

### Version C — Emphasis on BI Engineering

> **Experiment Intelligence Platform – CATALYST (Power BI)** — Architected a multi-page Power BI solution featuring custom Power Query ETL pipelines, a star schema data model, and 60+ organized DAX measures. Implemented custom JSON theme, dynamic labels, conditional formatting, and tooltip pages to create a stakeholder-ready experiment analysis dashboard.

### Version D — Emphasis on Product Analytics

> **Product Experiment Analysis (Power BI)** — Led end-to-end analysis of a landing page A/B test: data cleaning (removed 3,893 mismatched observations), experiment validation (confirmed 50/50 sample balance), country segmentation (US/UK/CA), and business impact quantification. Delivered strategic recommendation to retain the existing page, supported by a 5-page interactive dashboard.

---

## 2. Interview Explanation Script

### "Tell me about a project you're proud of" / "Walk me through your portfolio"

> *"I built an experiment intelligence platform called CATALYST in Power BI. The business scenario is: an e-commerce company ran an A/B test on their landing page with about 290,000 users across the US, UK, and Canada. My job was to clean the data, analyze the results, and present a recommendation.*
>
> *The first thing I did was inspect the raw data. I found about 3,900 rows where users were incorrectly assigned — a control group user seeing the new page, for example. These turned out to be duplicate user IDs, so I built a Power Query pipeline to filter out mismatches and deduplicate.*
>
> *After cleaning, I created over 60 DAX measures organized into folders like Core Metrics, Lift Analysis, and Country Segmentation. The key finding was that the new page actually performed slightly worse — 11.88% conversion versus 12.04% for the control. That's a -0.16 percentage point decline.*
>
> *I then segmented by country and found that UK showed a tiny positive signal, but it wasn't actionable. Canada showed the biggest decline, but with only 5% of the sample.*
>
> *The dashboard itself has 5 pages that tell a story: first the executive summary with a clear verdict, then the experiment mechanics, then geography, then a deep dive with hypotheses, and finally a strategic recommendation page. I also built 3 custom tooltip pages — when you hover over a country, you see a mini-dashboard with 10 data points.*
>
> *My recommendation was: do not launch the new page. And I quantified the business impact — if they had launched it, they would have lost approximately 230 conversions.*
>
> *I used a custom dark espresso theme I designed to make it look professional, and the whole thing is documented on GitHub with architecture diagrams, the theme JSON, and a build guide."*

### Key talking points to emphasize:

1. **Data cleaning was non-trivial** — you didn't just load CSVs. You found and handled a real data quality issue.
2. **The answer was "no"** — saying "don't launch" is often harder and more valuable than saying "launch." Emphasize this.
3. **You quantified the business impact** — "230 avoided lost conversions" shows business thinking.
4. **You segmented** — country analysis shows you think about heterogeneous treatment effects.
5. **You designed, not just built** — the custom theme, navigation, and tooltips show product sense.

---

## 3. Interviewer Questions & Answers

### Question 1: "Why did you choose Power BI for this project?"

> *"Power BI is the most widely used BI tool in enterprise environments, and I wanted to demonstrate proficiency in the tool that most hiring companies use. It also has excellent DAX capabilities, which let me build complex business logic measures, and its theming system let me create a polished, branded look. For this type of stakeholder-facing dashboard, Power BI is the right tool."*

### Question 2: "How did you identify the data quality issues?"

> *"I started by doing an exploratory data analysis in Python — checking for nulls, duplicates, and value distributions. I immediately noticed that the group-to-landing-page mapping wasn't always correct. I then cross-referenced: are the mismatched rows random, or are they correlated with something? Turns out, every single mismatched row belonged to a user who appeared more than once — they were double-bucketed. So removing mismatches also eliminated the duplicate problem."*

### Question 3: "How would you determine statistical significance?"

> *"In this project, I focused on the practical significance rather than running a formal hypothesis test, because the dashboard was designed for a business audience. However, if I were doing this in a professional setting, I would use a two-proportion z-test or chi-squared test. With 145K users per group and a base rate of 12%, the minimum detectable effect would be around 0.3-0.5 percentage points. Our observed difference of -0.16 pp is below that, which means even a statistical test would likely fail to reject the null hypothesis."*

### Question 4: "What was the most challenging part?"

> *"The most challenging part was designing the dashboard to tell a story rather than just show data. It's easy to put numbers on a page — it's hard to make a stakeholder understand the implication in 5 seconds. The verdict banner on Page 1 was a design decision I iterated on: I wanted someone to see the answer immediately, then explore the evidence on subsequent pages."*

### Question 5: "Why did you recommend against launching?"

> *"Three reasons. First, the treatment group performed worse, not better. Second, the magnitude of the difference (-0.16 pp) is too small to be actionable — it's within random noise territory. Third, no country segment showed a meaningfully positive result. When all the evidence points the same direction, the recommendation is clear."*

### Question 6: "What would you recommend as next steps?"

> *"I would recommend three things: (1) Conduct user research to understand what users actually want changed about the landing page — the redesign may have changed things users didn't care about. (2) Run a follow-up test in the UK specifically, since there was a slight positive signal there. (3) Test a more differentiated design — the current treatment might have been too similar to the control."*

### Question 7: "How would you handle this differently with more data?"

> *"If I had timestamp data (the dataset's timestamps were truncated), I would add time-series analysis to check for novelty effects or learning curves. If I had behavioral data like scroll depth or time on page, I could diagnose where users dropped off. And if I had revenue data, I could quantify the dollar impact of the conversion rate difference."*

### Question 8: "Explain your DAX measure for Absolute Lift."

> *"The Absolute Lift is simply the Treatment Conversion Rate minus the Control Conversion Rate. Both of those are calculated using DIVIDE and CALCULATE with a filter on the group column. The key design decision was storing the raw decimal version for calculations and creating a separate 'display' version with formatting — sign prefix, multiplied by 100, and a 'pp' suffix. This separation means my chart measures stay numerical while my card measures are human-readable."*

### Question 9: "Why did you use a star schema?"

> *"The country information is a dimension — it's a descriptive attribute of users. The ab_data table is the fact table — it contains the measurable events (conversions). By separating them into a star schema with a Many-to-One relationship, I ensure that filters from the country slicer correctly propagate to all metrics, and I avoid duplicate storage of country names across 290K rows."*

### Question 10: "What design principles influenced your dashboard?"

> *"Three main principles: (1) Progressive disclosure — start with the verdict, let users explore deeper. (2) Information hierarchy — the most important data gets the largest visual real estate. (3) Analyst voice — every page includes a written interpretation, not just charts. I took inspiration from modern SaaS products like Linear and Stripe, which use dark themes, generous spacing, and minimal decoration."*

### Question 11: "How do your tooltips work?"

> *"I created dedicated tooltip pages in Power BI — hidden pages set to 'tooltip' type with a specific size (320×280px). Each tooltip page contains multiple card visuals showing context-dependent data: users, conversions, rates, lift, contribution percentage, and a signal indicator. When a visual is configured to use a tooltip page, Power BI passes the filter context (e.g., which country is being hovered) to the tooltip page, and all the DAX measures evaluate within that context."*

### Question 12: "Walk me through your data cleaning pipeline."

> *"Three steps in Power Query: (1) Filter out rows where the group-page mapping is wrong — control users should see old_page, treatment should see new_page. This removes 3,893 logging errors. (2) Deduplicate on user_id, keeping the first occurrence. (3) Remove the timestamp column since it's truncated and unusable. I also enriched the countries table with full names, emoji flags, and a sort order column. All transformations are documented and reproducible."*

### Question 13: "How would you present this to a non-technical stakeholder?"

> *"I would start with the strategic brief page — the recommendation. 'We tested a new landing page with 290,000 users. The new page did not improve conversions. We recommend keeping the current page and redesigning before retesting.' If they ask for evidence, I'd show the command center page with the verdict banner and KPI cards. I'd avoid showing the deep dive page unless they specifically ask about methodology."*

### Question 14: "What's the difference between practical significance and statistical significance?"

> *"Statistical significance tells you whether a result is likely due to chance. Practical significance tells you whether the result matters for the business. A -0.16 percentage point difference with 290K users might actually be statistically significant (because the sample is so large), but it's not practically significant — no business would change its product strategy over a 0.16 pp difference. This is why I focused on business impact: 'Would launching lose conversions?' rather than just p-values."*

### Question 15: "How would you improve this project?"

> *"Four improvements: (1) Add a statistical significance calculation — p-value, confidence interval, and maybe a Bayesian approach. (2) Add session-level data if available — page views, time on page, bounce rate. (3) Build a Power BI dataflow for automated refresh if this were a live experiment. (4) Add a 'power analysis' page showing the minimum detectable effect for this sample size, so stakeholders understand what effect sizes we could have detected."*

### Question 16: "Why organize measures into display folders?"

> *"As the number of measures grows, the Fields pane becomes unmanageable. Display folders make the project maintainable — a new analyst can open the file and immediately understand where to find country-specific measures vs. tooltip measures. It's the same principle as organizing code into modules. In a team environment, this prevents confusion and reduces onboarding time."*

### Question 17: "How did you handle the timestamp issue?"

> *"I was transparent about it. The timestamps were truncated to just minutes and seconds — no dates, no hours. Rather than fabricating trends or guessing, I removed the column entirely and noted the limitation in my analysis. I designed the dashboard to work without time-series data, which is actually a useful skill — showing you can deliver insights with imperfect data."*

### Question 18: "Can you explain your conditional formatting approach?"

> *"I created a dedicated `[Lift Color]` measure that returns a hex color string based on whether the lift is positive, negative, or neutral. This measure is then used in conditional formatting rules across the dashboard — bar colors, text colors, and background tints. This centralized approach means if the color palette changes, I update one measure instead of reformatting every visual individually."*

### Question 19: "What's your approach to building dashboards generally?"

> *"I follow a four-step process: (1) Understand the business question — what decision needs to be made? (2) Design the data model — clean data, proper relationships, organized measures. (3) Sketch the layout before building — I plan the visual hierarchy and storytelling flow on paper first. (4) Build and iterate — start with the most important page, get feedback, refine. The key principle is: every visual should answer a specific business question. If it doesn't, it doesn't belong on the dashboard."*

### Question 20: "What makes this project different from other Power BI projects on GitHub?"

> *"Most portfolio projects use default themes, basic card visuals, and no narrative. This project has a custom design system with specific color tokens and typography rules, it tells a coherent story across 5 pages, it includes analyst interpretation (not just data display), it quantifies business impact, and it's documented with architecture diagrams, a build guide, and organized DAX measures. It's designed to look like something an internal analytics team would actually build and use."*

---

## 4. GitHub Portfolio Strategy

### Repository Setup

```
Repository name: catalyst-experiment-dashboard
Description: "A/B Test Experiment Intelligence Platform built in Power BI — 290K users, 60+ DAX measures, custom dark theme, 5-page interactive dashboard"
Topics: power-bi, dax, data-analysis, ab-testing, business-intelligence, dashboard, data-visualization, portfolio
```

### README Screenshots

Capture these 9 screenshots and save to `/screenshots/`:

| Priority | Screenshot | Filename | Why |
|----------|-----------|----------|-----|
| 🔴 P0 | Page 1 full view | `01_command_center.png` | First impression — recruiters see this first |
| 🔴 P0 | Page 5 recommendation | `06_strategic_brief.png` | Shows business thinking |
| 🟡 P1 | Country tooltip on hover | `07_tooltip_detail.png` | Demonstrates advanced Power BI skills |
| 🟡 P1 | Page 3 with matrix | `04_geographic_lens.png` | Shows segmentation ability |
| 🟡 P1 | Data model view | `08_data_model.png` | Shows data engineering skill |
| 🟢 P2 | Page 2 scorecard | `03_experiment_pulse.png` | Complete page set |
| 🟢 P2 | Page 4 deep dive | `05_deep_dive.png` | Complete page set |
| 🟢 P2 | DAX folder structure | `09_dax_measures.png` | Shows organization |
| 🟢 P2 | Nav bar close-up | `10_navigation.png` | Shows custom UX |

### Pinning the Repository

On GitHub:
1. Go to your profile
2. Click "Customize your pins"
3. Pin this repository in position 1 or 2

---

## 5. LinkedIn Post Template

```
🚀 Just shipped my latest analytics project: CATALYST

An Experiment Intelligence Platform built in Power BI, 
analyzing a 290K-user A/B test across 3 markets.

What it does:
✅ Cleans & validates experiment data  
✅ Computes 60+ business KPIs in DAX  
✅ Tells a story across 5 dashboard pages  
✅ Delivers a clear recommendation  

The answer? The new landing page didn't win.
And saying "don't launch" is just as valuable as saying "launch."

Tech: Power BI · DAX · Power Query · Star Schema · Custom Themes

What I learned building this:
→ Data cleaning is 40% of the work
→ Every chart should answer a question
→ Dashboards should tell stories, not just show numbers
→ Quantifying "what we avoided" is as important as "what we gained"

Link in comments 👇

#PowerBI #DataAnalytics #ABTesting #Portfolio #BusinessIntelligence
```

---

## 6. Suggestions to Make This Project Senior-Level

### Already Senior-Level ✅
1. ✅ Custom theme JSON (not default)
2. ✅ Organized DAX measures in display folders
3. ✅ Dynamic labels and titles
4. ✅ Rich tooltip pages
5. ✅ Conditional formatting with centralized color measures
6. ✅ Data cleaning pipeline with documented decisions
7. ✅ Business impact quantification
8. ✅ Analyst interpretation on every page

### To Take It Even Further 🚀

**9. Add a statistical significance page**
- Calculate z-score, p-value, and confidence interval in DAX
- Show a visual "meter" of statistical significance
- This shows you understand the math, not just the tools

**10. Add a "Power Analysis" section**
- Calculate the minimum detectable effect for this sample size
- Show that 290K users can detect a ~0.3 pp difference
- This demonstrates experimental design knowledge

**11. Create a data dictionary page**
- A hidden page with a table listing every measure, its formula, and folder
- Shows documentation discipline

**12. Add row-level security (RLS)**
- Create roles by country so different users see only their data
- Even if you don't deploy it, documenting that you designed it is impressive

**13. Build a refresh indicator**
- A card showing "Last refreshed: [date]" using `NOW()` or `UTCNOW()`
- Signals you think about data freshness

**14. Add export-friendly formatting**
- Ensure every page prints cleanly as a single PDF page
- Test with File → Export → PDF

**15. Record a 2-minute walkthrough video**
- Screen-record yourself walking through the dashboard
- Post on LinkedIn alongside the project
- This is the highest-impact thing you can do for recruiter visibility

**16. Add a "methodology" appendix page**
- Hidden page explaining: hypothesis, test design, cleaning rules, assumptions
- If a technical interviewer asks, you can show this

**17. Write a blog post**
- "How I Analyzed a 290K-User A/B Test in Power BI"
- Publish on Medium or your personal site
- Link from the GitHub README

**18. Add interactivity with Field Parameters**
- Let users switch between "Conversion Rate" and "User Count" on the same chart
- Shows advanced Power BI technique

**19. Create a comparative analysis section**
- "What if we had seen a +1 pp lift?" scenario with revenue impact
- Shows business modeling skills

**20. Add an anomaly detection callout**
- Flag if any country's conversion rate is 2+ standard deviations from the mean
- Even simple statistical concepts applied practically are impressive

---

## 7. Project Folder Structure (Final)

```
Catalyst/
│
├── README.md                           ← GitHub README with badges + screenshots
├── .gitignore                          ← Excludes CSVs and PBIX
├── Catalyst.pbix                       ← Power BI file (not in git)
│
├── theme/
│   └── Catalyst.json                   ← Custom Power BI theme (Dark Espresso)
│
├── docs/
│   ├── dashboard_architecture.md       ← Page layouts, visual specs, interactions
│   ├── dax_measures.md                 ← All 60+ measures with folders
│   ├── power_query.md                  ← M code for both tables + data model
│   └── build_guide.md                  ← Step-by-step construction guide
│
├── assets/
│   ├── icons/                          ← SVG icons for nav + KPIs (12 files)
│   │   ├── nav_home.svg
│   │   ├── nav_experiment.svg
│   │   ├── nav_geography.svg
│   │   ├── nav_deepdive.svg
│   │   ├── nav_strategic.svg
│   │   ├── kpi_users.svg
│   │   ├── kpi_clock.svg
│   │   ├── kpi_trend_up.svg
│   │   ├── kpi_trend_down.svg
│   │   ├── kpi_check.svg
│   │   ├── kpi_x.svg
│   │   └── section_grid.svg
│   └── backgrounds/                    ← Background images if needed
│
├── data/                               ← Source data (not in git)
│   └── ecommerce_ab_testing_2022_dataset1/
│       ├── ab_data.csv
│       └── countries.csv
│
└── screenshots/                        ← Dashboard screenshots for README
    ├── 01_command_center.png
    ├── 02_command_center_tooltip.png
    ├── 03_experiment_pulse.png
    ├── 04_geographic_lens.png
    ├── 05_deep_dive.png
    ├── 06_strategic_brief.png
    ├── 07_tooltip_detail.png
    ├── 08_data_model.png
    └── 09_dax_measures.png
```

---

## 8. Accessibility Improvements

| Area | Recommendation |
|------|----------------|
| **Color contrast** | All text meets WCAG AA (4.5:1 ratio). `#F5EDE4` on `#131010` = 14.2:1 ✅ |
| **Tab order** | Set logical left-to-right, top-to-bottom reading order in Selection Pane |
| **Alt text** | Add alt text to every chart via Format → General → Alt Text |
| **Screen reader** | Use descriptive visual titles (not "Chart 1") |
| **Color independence** | Don't rely solely on red/green — use arrows (▲▼) and text labels |
| **Keyboard navigation** | Ensure slicers are keyboard-accessible (they are by default) |

---

## 9. Mobile Layout Recommendations

| Priority | Component | Include on Mobile? |
|----------|-----------|-------------------|
| 🔴 Must | Verdict Banner | Yes — top of screen |
| 🔴 Must | KPI Cards (top 4) | Yes — stacked vertically |
| 🟡 Nice | Conversion Comparison | Yes — single bar chart |
| 🟡 Nice | Country Slicer | Yes — dropdown |
| 🟢 Skip | Navigation bar | No — mobile uses native page tabs |
| 🟢 Skip | Tooltips | No — tooltips don't work on touch |
| 🟢 Skip | Hypothesis text | No — too much reading for mobile |
