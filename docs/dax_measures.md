# CATALYST — Complete DAX Measures Library

## Naming Convention

```
Pattern:    [Folder]_[Metric Name]
Examples:   Core_Total Users
            Lift_Absolute Lift
            Tooltip_Country Users

Display:    The folder prefix is NOT shown in visuals.
            It's only for organization in the Fields pane.

Naming:     Use spaces in measure names (Power BI convention).
            PascalCase for internal references.
```

## Folder Structure (Display Folders)

Set display folders in Model View → Select measure → Properties → Display Folder

| Folder | Purpose | Measures |
|--------|---------|----------|
| `📊 Core Metrics` | Primary KPIs | 6 measures |
| `🔬 Group Analysis` | Control vs Treatment breakdown | 8 measures |
| `📈 Lift Analysis` | Lift calculations and indicators | 7 measures |
| `🌍 Country Analysis` | Country-level metrics | 6 measures |
| `💬 Tooltip Measures` | Rich tooltip content | 10 measures |
| `🧮 What-If Analysis` | Business impact scenarios | 5 measures |
| `🏷️ Dynamic Labels` | Dynamic titles, subtitles, context | 6 measures |
| `🔧 Utility` | Formatting helpers | 4 measures |

---

## 📊 Core Metrics

```dax
// ═══════════════════════════════════════════════════════════════
// MEASURE: Total Users
// FOLDER: 📊 Core Metrics
// DESCRIPTION: Count of all users in the cleaned dataset
// USED ON: Page 1 KPI card, Tooltips, Tables
// ═══════════════════════════════════════════════════════════════

Total Users = 
    COUNTROWS('ab_data')


// ═══════════════════════════════════════════════════════════════
// MEASURE: Total Conversions
// FOLDER: 📊 Core Metrics
// DESCRIPTION: Count of users who converted (converted = 1)
// USED ON: Page 1 KPI card, Tooltips, Tables
// ═══════════════════════════════════════════════════════════════

Total Conversions = 
    SUM('ab_data'[converted])


// ═══════════════════════════════════════════════════════════════
// MEASURE: Non Converted Users
// FOLDER: 📊 Core Metrics
// DESCRIPTION: Users who did not convert
// USED ON: Funnel chart (Page 2)
// ═══════════════════════════════════════════════════════════════

Non Converted Users = 
    [Total Users] - [Total Conversions]


// ═══════════════════════════════════════════════════════════════
// MEASURE: Conversion Rate
// FOLDER: 📊 Core Metrics
// DESCRIPTION: Overall conversion rate as decimal
// USED ON: Internal reference (other measures use this)
// ═══════════════════════════════════════════════════════════════

Conversion Rate = 
    DIVIDE(
        [Total Conversions],
        [Total Users],
        0
    )


// ═══════════════════════════════════════════════════════════════
// MEASURE: Conversion Rate Display
// FOLDER: 📊 Core Metrics
// DESCRIPTION: Formatted conversion rate for card visuals
// USED ON: Page 1 KPI card, Page 3 country cards
// FORMAT: Percentage with 2 decimals
// ═══════════════════════════════════════════════════════════════

Conversion Rate Display = 
    FORMAT([Conversion Rate], "0.00%")


// ═══════════════════════════════════════════════════════════════
// MEASURE: Data Quality Score
// FOLDER: 📊 Core Metrics
// DESCRIPTION: Percentage of clean rows (after removing mismatches/dupes)
// USED ON: Page 5 Impact Summary card
// ═══════════════════════════════════════════════════════════════

Data Quality Score = 
    VAR _CleanRows = COUNTROWS('ab_data')
    VAR _OriginalRows = 294480
    RETURN
        FORMAT(DIVIDE(_CleanRows, _OriginalRows, 0), "0.0%")
```

---

## 🔬 Group Analysis

```dax
// ═══════════════════════════════════════════════════════════════
// MEASURE: Control Users
// FOLDER: 🔬 Group Analysis
// ═══════════════════════════════════════════════════════════════

Control Users = 
    CALCULATE(
        COUNTROWS('ab_data'),
        'ab_data'[group] = "control"
    )


// ═══════════════════════════════════════════════════════════════
// MEASURE: Treatment Users
// FOLDER: 🔬 Group Analysis
// ═══════════════════════════════════════════════════════════════

Treatment Users = 
    CALCULATE(
        COUNTROWS('ab_data'),
        'ab_data'[group] = "treatment"
    )


// ═══════════════════════════════════════════════════════════════
// MEASURE: Control Conversions
// FOLDER: 🔬 Group Analysis
// ═══════════════════════════════════════════════════════════════

Control Conversions = 
    CALCULATE(
        SUM('ab_data'[converted]),
        'ab_data'[group] = "control"
    )


// ═══════════════════════════════════════════════════════════════
// MEASURE: Treatment Conversions
// FOLDER: 🔬 Group Analysis
// ═══════════════════════════════════════════════════════════════

Treatment Conversions = 
    CALCULATE(
        SUM('ab_data'[converted]),
        'ab_data'[group] = "treatment"
    )


// ═══════════════════════════════════════════════════════════════
// MEASURE: Control Conversion Rate
// FOLDER: 🔬 Group Analysis
// ═══════════════════════════════════════════════════════════════

Control Conversion Rate = 
    DIVIDE(
        [Control Conversions],
        [Control Users],
        0
    )


// ═══════════════════════════════════════════════════════════════
// MEASURE: Treatment Conversion Rate
// FOLDER: 🔬 Group Analysis
// ═══════════════════════════════════════════════════════════════

Treatment Conversion Rate = 
    DIVIDE(
        [Treatment Conversions],
        [Treatment Users],
        0
    )


// ═══════════════════════════════════════════════════════════════
// MEASURE: Control Conversion Rate Display
// FOLDER: 🔬 Group Analysis
// USED ON: Page 1 KPI card, Page 2 scorecard
// ═══════════════════════════════════════════════════════════════

Control CR Display = 
    FORMAT([Control Conversion Rate], "0.00%")


// ═══════════════════════════════════════════════════════════════
// MEASURE: Treatment Conversion Rate Display
// FOLDER: 🔬 Group Analysis
// USED ON: Page 1 KPI card, Page 2 scorecard
// ═══════════════════════════════════════════════════════════════

Treatment CR Display = 
    FORMAT([Treatment Conversion Rate], "0.00%")


// ═══════════════════════════════════════════════════════════════
// MEASURE: Control Non Converted
// FOLDER: 🔬 Group Analysis
// USED ON: Funnel chart (Page 2)
// ═══════════════════════════════════════════════════════════════

Control Non Converted = 
    [Control Users] - [Control Conversions]


// ═══════════════════════════════════════════════════════════════
// MEASURE: Treatment Non Converted
// FOLDER: 🔬 Group Analysis
// USED ON: Funnel chart (Page 2)
// ═══════════════════════════════════════════════════════════════

Treatment Non Converted = 
    [Treatment Users] - [Treatment Conversions]
```

---

## 📈 Lift Analysis

```dax
// ═══════════════════════════════════════════════════════════════
// MEASURE: Absolute Lift
// FOLDER: 📈 Lift Analysis
// DESCRIPTION: Treatment CR minus Control CR in percentage points
// ═══════════════════════════════════════════════════════════════

Absolute Lift = 
    [Treatment Conversion Rate] - [Control Conversion Rate]


// ═══════════════════════════════════════════════════════════════
// MEASURE: Relative Lift
// FOLDER: 📈 Lift Analysis
// DESCRIPTION: Percentage change from control to treatment
// ═══════════════════════════════════════════════════════════════

Relative Lift = 
    DIVIDE(
        [Absolute Lift],
        [Control Conversion Rate],
        0
    )


// ═══════════════════════════════════════════════════════════════
// MEASURE: Absolute Lift Display
// FOLDER: 📈 Lift Analysis
// DESCRIPTION: Formatted with sign and "pp" suffix
// USED ON: Page 1 KPI card, Page 1 verdict banner, Tooltips
// ═══════════════════════════════════════════════════════════════

Absolute Lift Display = 
    VAR _Lift = [Absolute Lift]
    VAR _Sign = IF(_Lift > 0, "+", "")
    RETURN
        _Sign & FORMAT(_Lift * 100, "0.00") & " pp"


// ═══════════════════════════════════════════════════════════════
// MEASURE: Relative Lift Display
// FOLDER: 📈 Lift Analysis
// ═══════════════════════════════════════════════════════════════

Relative Lift Display = 
    VAR _Lift = [Relative Lift]
    VAR _Sign = IF(_Lift > 0, "+", "")
    RETURN
        _Sign & FORMAT(_Lift, "0.00%")


// ═══════════════════════════════════════════════════════════════
// MEASURE: Lift Direction
// FOLDER: 📈 Lift Analysis
// DESCRIPTION: Returns text for conditional formatting rules
// ═══════════════════════════════════════════════════════════════

Lift Direction = 
    SWITCH(
        TRUE(),
        [Absolute Lift] > 0.001, "Positive",
        [Absolute Lift] < -0.001, "Negative",
        "Neutral"
    )


// ═══════════════════════════════════════════════════════════════
// MEASURE: Lift Color
// FOLDER: 📈 Lift Analysis
// DESCRIPTION: Returns hex color for conditional formatting
// USE: In conditional formatting → Rules → Based on: Lift Color
// ═══════════════════════════════════════════════════════════════

Lift Color = 
    SWITCH(
        TRUE(),
        [Absolute Lift] > 0.001, "#7FB069",
        [Absolute Lift] < -0.001, "#D4574E",
        "#A8845E"
    )


// ═══════════════════════════════════════════════════════════════
// MEASURE: Lift Arrow
// FOLDER: 📈 Lift Analysis
// DESCRIPTION: Unicode arrow + formatted lift for compact display
// ═══════════════════════════════════════════════════════════════

Lift Arrow = 
    VAR _Lift = [Absolute Lift]
    VAR _Arrow = IF(_Lift > 0, "▲ ", IF(_Lift < 0, "▼ ", "● "))
    VAR _Value = FORMAT(ABS(_Lift) * 100, "0.00")
    RETURN
        _Arrow & _Value & " pp"


// ═══════════════════════════════════════════════════════════════
// MEASURE: Sample Balance Ratio
// FOLDER: 📈 Lift Analysis
// DESCRIPTION: Control / Treatment ratio (ideal = 1.0)
// ═══════════════════════════════════════════════════════════════

Sample Balance Ratio = 
    DIVIDE([Control Users], [Treatment Users], 0)


// ═══════════════════════════════════════════════════════════════
// MEASURE: Sample Balance Display
// FOLDER: 📈 Lift Analysis
// ═══════════════════════════════════════════════════════════════

Sample Balance Display = 
    VAR _CtrlPct = FORMAT(DIVIDE([Control Users], [Total Users], 0), "0.0%")
    VAR _TreatPct = FORMAT(DIVIDE([Treatment Users], [Total Users], 0), "0.0%")
    VAR _Diff = ABS([Control Users] - [Treatment Users])
    RETURN
        "Control: " & _CtrlPct & "  |  Treatment: " & _TreatPct & 
        "  |  Δ = " & FORMAT(_Diff, "#,0") & " users"
```

---

## 🌍 Country Analysis

```dax
// ═══════════════════════════════════════════════════════════════
// MEASURE: Country Users
// FOLDER: 🌍 Country Analysis
// DESCRIPTION: User count that responds to country filter context
// ═══════════════════════════════════════════════════════════════

Country Users = 
    COUNTROWS('ab_data')


// ═══════════════════════════════════════════════════════════════
// MEASURE: Country Conversion Rate
// FOLDER: 🌍 Country Analysis
// ═══════════════════════════════════════════════════════════════

Country Conversion Rate = 
    DIVIDE(
        SUM('ab_data'[converted]),
        COUNTROWS('ab_data'),
        0
    )


// ═══════════════════════════════════════════════════════════════
// MEASURE: Country User Share
// FOLDER: 🌍 Country Analysis
// DESCRIPTION: What % of total users this country represents
// ═══════════════════════════════════════════════════════════════

Country User Share = 
    DIVIDE(
        COUNTROWS('ab_data'),
        CALCULATE(COUNTROWS('ab_data'), ALL('countries')),
        0
    )


// ═══════════════════════════════════════════════════════════════
// MEASURE: Country Conversion Contribution
// FOLDER: 🌍 Country Analysis
// DESCRIPTION: What % of total conversions this country drives
// ═══════════════════════════════════════════════════════════════

Country Conversion Contribution = 
    DIVIDE(
        SUM('ab_data'[converted]),
        CALCULATE(SUM('ab_data'[converted]), ALL('countries')),
        0
    )


// ═══════════════════════════════════════════════════════════════
// MEASURE: Country Lift
// FOLDER: 🌍 Country Analysis
// DESCRIPTION: Treatment CR - Control CR within the current
//              country filter context
// ═══════════════════════════════════════════════════════════════

Country Lift = 
    VAR _ControlCR = 
        CALCULATE(
            DIVIDE(SUM('ab_data'[converted]), COUNTROWS('ab_data'), 0),
            'ab_data'[group] = "control"
        )
    VAR _TreatmentCR = 
        CALCULATE(
            DIVIDE(SUM('ab_data'[converted]), COUNTROWS('ab_data'), 0),
            'ab_data'[group] = "treatment"
        )
    RETURN
        _TreatmentCR - _ControlCR


// ═══════════════════════════════════════════════════════════════
// MEASURE: Country Lift Display
// FOLDER: 🌍 Country Analysis
// ═══════════════════════════════════════════════════════════════

Country Lift Display = 
    VAR _Lift = [Country Lift]
    VAR _Arrow = IF(_Lift > 0, "▲ ", IF(_Lift < 0, "▼ ", "● "))
    VAR _Sign = IF(_Lift > 0, "+", "")
    RETURN
        _Arrow & _Sign & FORMAT(_Lift * 100, "0.00") & " pp"


// ═══════════════════════════════════════════════════════════════
// MEASURE: Country Signal
// FOLDER: 🌍 Country Analysis
// DESCRIPTION: Text recommendation per country
// ═══════════════════════════════════════════════════════════════

Country Signal = 
    VAR _Lift = [Country Lift]
    VAR _Users = [Total Users]
    RETURN
        SWITCH(
            TRUE(),
            _Lift > 0.001 && _Users > 50000, "🔍 Investigate",
            _Lift > 0.001 && _Users <= 50000, "⚠️ Low sample",
            _Lift < -0.001, "⛔ Decline",
            "● Flat"
        )
```

---

## 💬 Tooltip Measures

```dax
// ═══════════════════════════════════════════════════════════════
// TOOLTIP MEASURES — Used on tooltip report pages
// These respond to whatever filter context the hovering
// visual provides (country, group, landing_page, etc.)
// ═══════════════════════════════════════════════════════════════

// ── Country Tooltip ──

Tooltip Country Name = 
    IF(
        HASONEVALUE('countries'[country]),
        SWITCH(
            VALUES('countries'[country]),
            "US", "🇺🇸 United States",
            "UK", "🇬🇧 United Kingdom",
            "CA", "🇨🇦 Canada",
            VALUES('countries'[country])
        ),
        "All Countries"
    )

Tooltip Country Users = 
    FORMAT([Total Users], "#,0")

Tooltip Country Conversions = 
    FORMAT([Total Conversions], "#,0")

Tooltip Country CR = 
    FORMAT([Conversion Rate], "0.00%")

Tooltip Country Share = 
    "Share of users: " & FORMAT([Country User Share], "0.0%")

Tooltip Country Contribution = 
    "Conversion contribution: " & FORMAT([Country Conversion Contribution], "0.0%")

Tooltip Country Control CR = 
    "Control: " & FORMAT([Control Conversion Rate], "0.00%") & 
    " (" & FORMAT([Control Users], "#,0") & ")"

Tooltip Country Treatment CR = 
    "Treatment: " & FORMAT([Treatment Conversion Rate], "0.00%") & 
    " (" & FORMAT([Treatment Users], "#,0") & ")"

Tooltip Country Lift = 
    [Country Lift Display]


// ── Group Tooltip ──

Tooltip Group Name = 
    IF(
        HASONEVALUE('ab_data'[group]),
        UPPER(VALUES('ab_data'[group])) & " GROUP",
        "ALL GROUPS"
    )

Tooltip Group Users = 
    "Sample Size: " & FORMAT([Total Users], "#,0")

Tooltip Group Conversions = 
    "Conversions: " & FORMAT([Total Conversions], "#,0")

Tooltip Group CR = 
    "Conversion Rate: " & FORMAT([Conversion Rate], "0.00%")

Tooltip Group Share = 
    "Share of total: " & FORMAT(
        DIVIDE([Total Users], CALCULATE(COUNTROWS('ab_data'), ALL('ab_data'[group])), 0),
        "0.0%"
    )


// ── KPI Tooltip ──

Tooltip KPI Summary = 
    "Users: " & FORMAT([Total Users], "#,0") & 
    UNICHAR(10) &
    "Conversions: " & FORMAT([Total Conversions], "#,0") &
    UNICHAR(10) &
    "Rate: " & FORMAT([Conversion Rate], "0.00%") &
    UNICHAR(10) &
    "Lift: " & [Absolute Lift Display]

Tooltip KPI Verdict = 
    VAR _Lift = [Absolute Lift]
    RETURN
        IF(
            _Lift < -0.001,
            "⛔ Treatment underperformed control",
            IF(
                _Lift > 0.001,
                "✅ Treatment outperformed control",
                "● No meaningful difference detected"
            )
        )
```

---

## 🧮 What-If Analysis

```dax
// ═══════════════════════════════════════════════════════════════
// WHAT-IF MEASURES — Business impact quantification
// Used on Page 4 (Deep Dive) and Page 5 (Strategic Brief)
// ═══════════════════════════════════════════════════════════════

// ── Missed Conversions (if treatment matched control) ──

Missed Conversions = 
    VAR _TreatUsers = [Treatment Users]
    VAR _CtrlCR = [Control Conversion Rate]
    VAR _TreatConv = [Treatment Conversions]
    VAR _ExpectedConv = _TreatUsers * _CtrlCR
    RETURN
        ROUND(_ExpectedConv - _TreatConv, 0)


// ── Upside if +1pp improvement ──

Upside 1pp Conversions = 
    VAR _TreatUsers = [Treatment Users]
    VAR _CtrlCR = [Control Conversion Rate]
    VAR _UpsideCR = _CtrlCR + 0.01
    VAR _CurrentConv = _TreatUsers * _CtrlCR
    VAR _UpsideConv = _TreatUsers * _UpsideCR
    RETURN
        ROUND(_UpsideConv - _CurrentConv, 0)


// ── Revenue Impact (assumes $10 per conversion) ──

Revenue Impact Per Conversion = 10

Revenue Impact 1pp = 
    [Upside 1pp Conversions] * [Revenue Impact Per Conversion]

Revenue Impact Display = 
    "+" & FORMAT([Revenue Impact 1pp], "$#,0")


// ── Experiment Cost (users consumed) ──

Experiment Cost Display = 
    FORMAT([Total Users], "#,0") & " users tested"


// ── Avoided Risk ──

Avoided Risk = 
    VAR _MissedConv = [Missed Conversions]
    RETURN
        IF(
            _MissedConv > 0,
            FORMAT(_MissedConv, "#,0") & " conversions saved by not launching",
            "No risk — treatment performed at or above control"
        )
```

---

## 🏷️ Dynamic Labels

```dax
// ═══════════════════════════════════════════════════════════════
// DYNAMIC LABELS — For dynamic titles and subtitles
// Makes the dashboard feel responsive and intelligent
// ═══════════════════════════════════════════════════════════════

// ── Page 1 Dynamic Title ──

Dynamic Title Page 1 = 
    VAR _Country = 
        IF(
            ISFILTERED('countries'[country]),
            CONCATENATEX(VALUES('countries'[country]), 'countries'[country], ", "),
            "All Markets"
        )
    RETURN
        "A/B Test Results — " & _Country


// ── Page 3 Dynamic Subtitle ──

Dynamic Subtitle Page 3 = 
    VAR _TotalUsers = FORMAT([Total Users], "#,0")
    VAR _Countries = DISTINCTCOUNT('countries'[country])
    RETURN
        "Analyzing " & _TotalUsers & " users across " & _Countries & " markets"


// ── Verdict Text (dynamic) ──

Verdict Text = 
    VAR _Lift = [Absolute Lift]
    RETURN
        IF(
            _Lift < -0.001,
            "⛔  NEW PAGE DID NOT OUTPERFORM",
            IF(
                _Lift > 0.001,
                "✅  NEW PAGE SHOWS IMPROVEMENT",
                "●  NO MEANINGFUL DIFFERENCE DETECTED"
            )
        )


// ── Verdict Detail ──

Verdict Detail = 
    "Absolute Lift: " & [Absolute Lift Display] & 
    "  |  Relative Lift: " & [Relative Lift Display]


// ── Recommendation Text ──

Recommendation Text = 
    VAR _Lift = [Absolute Lift]
    RETURN
        IF(
            _Lift < -0.001,
            "RECOMMENDATION: DO NOT LAUNCH THE NEW PAGE" & UNICHAR(10) & UNICHAR(10) &
            "The new landing page showed a " & [Absolute Lift Display] & 
            " decline in conversion rate compared to the existing page. " &
            "With " & FORMAT([Total Users], "#,0") & " users and a balanced 50/50 split, " &
            "the experiment was properly designed. The result is clear: " &
            "the new page does not improve conversions.",
            IF(
                _Lift > 0.001,
                "RECOMMENDATION: CONSIDER LAUNCHING THE NEW PAGE" & UNICHAR(10) & UNICHAR(10) &
                "The new page showed a " & [Absolute Lift Display] & " improvement.",
                "RECOMMENDATION: INCONCLUSIVE — GATHER MORE DATA" & UNICHAR(10) & UNICHAR(10) &
                "The difference is too small to act on."
            )
        )


// ── Page Footer ──

Footer Text = 
    "Analysis Platform: CATALYST  ·  Dataset: 290,585 users  ·  Markets: US, UK, CA"
```

---

## 🔧 Utility

```dax
// ═══════════════════════════════════════════════════════════════
// UTILITY MEASURES — Helpers and formatting
// ═══════════════════════════════════════════════════════════════

// ── Format Large Numbers ──

Format Users = 
    VAR _N = [Total Users]
    RETURN
        IF(
            _N >= 1000000, FORMAT(_N / 1000000, "0.0") & "M",
            IF(
                _N >= 1000, FORMAT(_N / 1000, "0.0") & "K",
                FORMAT(_N, "#,0")
            )
        )

Format Conversions = 
    VAR _N = [Total Conversions]
    RETURN
        IF(
            _N >= 1000000, FORMAT(_N / 1000000, "0.0") & "M",
            IF(
                _N >= 1000, FORMAT(_N / 1000, "0.0") & "K",
                FORMAT(_N, "#,0")
            )
        )


// ── Conditional Border Color (for card formatting) ──

Border Color = 
    IF([Absolute Lift] > 0, "#7FB069", IF([Absolute Lift] < 0, "#D4574E", "#A8845E"))


// ── Blank Measure (for placeholder visuals during build) ──

_Blank = BLANK()
```

---

## Calculated Column (Add to ab_data table)

```dax
// ═══════════════════════════════════════════════════════════════
// CALCULATED COLUMN: Experiment Result
// TABLE: ab_data
// DESCRIPTION: Human-readable conversion status
// ═══════════════════════════════════════════════════════════════

Experiment Result = 
    IF(
        'ab_data'[converted] = 1,
        "Converted",
        "Not Converted"
    )
```

---

## Measure Summary Table

| # | Measure Name | Folder | Type | Format |
|---|-------------|--------|------|--------|
| 1 | Total Users | 📊 Core Metrics | Aggregate | `#,0` |
| 2 | Total Conversions | 📊 Core Metrics | Aggregate | `#,0` |
| 3 | Non Converted Users | 📊 Core Metrics | Calculated | `#,0` |
| 4 | Conversion Rate | 📊 Core Metrics | Ratio | `0.00%` |
| 5 | Conversion Rate Display | 📊 Core Metrics | Display | Text |
| 6 | Data Quality Score | 📊 Core Metrics | Static | Text |
| 7 | Control Users | 🔬 Group Analysis | Filtered | `#,0` |
| 8 | Treatment Users | 🔬 Group Analysis | Filtered | `#,0` |
| 9 | Control Conversions | 🔬 Group Analysis | Filtered | `#,0` |
| 10 | Treatment Conversions | 🔬 Group Analysis | Filtered | `#,0` |
| 11 | Control Conversion Rate | 🔬 Group Analysis | Ratio | `0.00%` |
| 12 | Treatment Conversion Rate | 🔬 Group Analysis | Ratio | `0.00%` |
| 13 | Control CR Display | 🔬 Group Analysis | Display | Text |
| 14 | Treatment CR Display | 🔬 Group Analysis | Display | Text |
| 15 | Control Non Converted | 🔬 Group Analysis | Calculated | `#,0` |
| 16 | Treatment Non Converted | 🔬 Group Analysis | Calculated | `#,0` |
| 17 | Absolute Lift | 📈 Lift Analysis | Calculated | `0.00%` |
| 18 | Relative Lift | 📈 Lift Analysis | Ratio | `0.00%` |
| 19 | Absolute Lift Display | 📈 Lift Analysis | Display | Text |
| 20 | Relative Lift Display | 📈 Lift Analysis | Display | Text |
| 21 | Lift Direction | 📈 Lift Analysis | Conditional | Text |
| 22 | Lift Color | 📈 Lift Analysis | Conditional | Hex |
| 23 | Lift Arrow | 📈 Lift Analysis | Display | Text |
| 24 | Sample Balance Ratio | 📈 Lift Analysis | Ratio | `0.0000` |
| 25 | Sample Balance Display | 📈 Lift Analysis | Display | Text |
| 26 | Country Users | 🌍 Country Analysis | Context | `#,0` |
| 27 | Country Conversion Rate | 🌍 Country Analysis | Context | `0.00%` |
| 28 | Country User Share | 🌍 Country Analysis | Ratio | `0.0%` |
| 29 | Country Conversion Contribution | 🌍 Country Analysis | Ratio | `0.0%` |
| 30 | Country Lift | 🌍 Country Analysis | Calculated | `0.00%` |
| 31 | Country Lift Display | 🌍 Country Analysis | Display | Text |
| 32 | Country Signal | 🌍 Country Analysis | Conditional | Text |
| 33–42 | Tooltip Country * | 💬 Tooltip Measures | Display | Text |
| 43–47 | Tooltip Group * | 💬 Tooltip Measures | Display | Text |
| 48–49 | Tooltip KPI * | 💬 Tooltip Measures | Display | Text |
| 50 | Missed Conversions | 🧮 What-If Analysis | Calculated | `#,0` |
| 51 | Upside 1pp Conversions | 🧮 What-If Analysis | Calculated | `#,0` |
| 52 | Revenue Impact 1pp | 🧮 What-If Analysis | Calculated | `$#,0` |
| 53 | Revenue Impact Display | 🧮 What-If Analysis | Display | Text |
| 54 | Avoided Risk | 🧮 What-If Analysis | Display | Text |
| 55–60 | Dynamic * | 🏷️ Dynamic Labels | Dynamic | Text |
| 61–64 | Format/Utility * | 🔧 Utility | Helper | Various |
