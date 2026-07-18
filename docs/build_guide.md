# CATALYST — Step-by-Step Build Guide

> This guide assumes you have Power BI Desktop installed and the two CSV files extracted.
> Follow each step in order. Estimated total build time: 3–4 hours.

---

## Phase 1: Project Setup (15 min)

### Step 1.1 — Create the Power BI File

1. Open **Power BI Desktop**
2. **File → Save As** → Save as `Catalyst.pbix` in your project folder

### Step 1.2 — Import the Theme

1. Go to **View → Themes → Browse for themes**
2. Navigate to `theme/Catalyst.json`
3. Click **Open** → the theme applies immediately
4. You should see the dark background and warm color palette

### Step 1.3 — Set Report Page Size

1. Go to **Format → Canvas settings** (on the Visualizations pane)
2. Set **Type: Custom**
3. **Width: 1280**, **Height: 720**
4. Repeat this for every page you create

### Step 1.4 — Set Power BI Options

1. **File → Options and Settings → Options**
2. **Data Load:**
   - ☐ Uncheck "Auto date/time" 
   - ☐ Uncheck "Auto type detection"
3. **Report Settings:**
   - ☑ Persistent filters: ON
4. Click **OK**

---

## Phase 2: Load & Clean Data (20 min)

### Step 2.1 — Load ab_data.csv

1. **Home → Get Data → Text/CSV**
2. Navigate to `data/ecommerce_ab_testing_2022_dataset1/ab_data.csv`
3. In the preview, click **Transform Data** (do NOT click "Load")
4. You're now in **Power Query Editor**

### Step 2.2 — Apply ab_data Cleaning Steps

In the Power Query Editor, open the **Advanced Editor** (Home → Advanced Editor) and paste the full M code from [power_query.md](file:///Users/hardikkadd/.gemini/antigravity-ide/brain/d9e49980-d427-49b8-977c-c024cb426759/power_query.md) — Table 1 section.

> [!IMPORTANT]
> Update the file path in the `Source` step to match your actual file location. If Power BI already loaded the Source step, keep it and paste the remaining steps after it.

### Step 2.3 — Load countries.csv

1. In Power Query Editor, click **Home → New Source → Text/CSV**
2. Navigate to `countries.csv`
3. Open **Advanced Editor** and paste the countries M code from the Power Query document

### Step 2.4 — Apply & Close

1. Click **Home → Close & Apply**
2. Wait for both tables to load
3. Verify in the **Fields pane** (right sidebar) that you see both tables

### Step 2.5 — Create Relationship

1. Click the **Model View** icon (left sidebar — looks like a database diagram)
2. Drag `ab_data[user_id]` → `countries[user_id]`
3. Set: **Many-to-One**, **Single direction**, **Active**
4. Click OK

### Step 2.6 — Verify Data

Switch back to **Report View**. Insert a temporary table visual:
- Drag `ab_data[user_id]` → check row count shows 290,585
- Drag `countries[country]` → verify US, UK, CA appear
- Delete the temporary table when done

---

## Phase 3: Create All DAX Measures (30 min)

### How to Create a Measure

1. In the **Fields pane**, right-click on the `ab_data` table
2. Click **New Measure**
3. Paste the DAX formula
4. Press **Enter** or click the checkmark
5. In the **Properties pane**, set the **Display Folder** (e.g., "📊 Core Metrics")

### Measure Entry Order

Create measures in this order (dependencies flow downward):

#### Batch 1: Core Metrics (create first — other measures depend on these)

| # | Measure | Formula Reference |
|---|---------|-------------------|
| 1 | Total Users | See [dax_measures.md](file:///Users/hardikkadd/.gemini/antigravity-ide/brain/d9e49980-d427-49b8-977c-c024cb426759/dax_measures.md) → 📊 Core Metrics |
| 2 | Total Conversions | " |
| 3 | Non Converted Users | " |
| 4 | Conversion Rate | " |
| 5 | Conversion Rate Display | " |
| 6 | Data Quality Score | " |

#### Batch 2: Group Analysis

| # | Measure |
|---|---------|
| 7 | Control Users |
| 8 | Treatment Users |
| 9 | Control Conversions |
| 10 | Treatment Conversions |
| 11 | Control Conversion Rate |
| 12 | Treatment Conversion Rate |
| 13 | Control CR Display |
| 14 | Treatment CR Display |
| 15 | Control Non Converted |
| 16 | Treatment Non Converted |

#### Batch 3: Lift Analysis (depends on Group Analysis)

| # | Measure |
|---|---------|
| 17 | Absolute Lift |
| 18 | Relative Lift |
| 19 | Absolute Lift Display |
| 20 | Relative Lift Display |
| 21 | Lift Direction |
| 22 | Lift Color |
| 23 | Lift Arrow |
| 24 | Sample Balance Ratio |
| 25 | Sample Balance Display |

#### Batch 4: Country Analysis

| # | Measure |
|---|---------|
| 26–32 | All Country measures |

#### Batch 5: Tooltip, What-If, Dynamic Labels, Utility

| # | Measure |
|---|---------|
| 33–64 | Remaining measures |

#### Add the Calculated Column

1. In **Data View** (left sidebar), select the `ab_data` table
2. Click **New Column** (top ribbon)
3. Paste: `Experiment Result = IF('ab_data'[converted] = 1, "Converted", "Not Converted")`

### Set Display Folders

After creating all measures:
1. Go to **Model View**
2. Select each measure in the Fields pane
3. In the **Properties** panel (bottom-right), type the folder name in **Display Folder**
4. Use the exact folder names from the DAX document (with emojis)

> [!TIP]
> You can multi-select measures (Ctrl+Click) and set the Display Folder for all of them at once.

---

## Phase 4: Build Page 1 — Command Center (45 min)

### Step 4.1 — Rename the Page

1. Right-click the page tab at the bottom → **Rename** → Type "Command Center"

### Step 4.2 — Build the Navigation Bar

1. **Insert → Shapes → Rounded Rectangle**
   - Position: `x=0, y=0`, Size: `1280×48`
   - Fill: `#1E1714`, Border: None, Corner Radius: 0
2. **Insert → Text Box** (logo)
   - Position: `x=24, y=6`, Size: `200×20`
   - Text: "◈ CATALYST", 13pt Segoe UI Semibold, color `#C8956C`
3. **Insert → Text Box** (subtitle)
   - Position: `x=24, y=26`, Size: `200×16`
   - Text: "Experiment Intelligence", 8pt Segoe UI, color `#8A7560`
4. **Insert → Buttons → Blank** (×5, one per page)
   - Create 5 buttons with text: "Command Center", "Experiment", "Geography", "Deep Dive", "Strategic Brief"
   - Size: ~`130×32` each, starting at `x=340`
   - For each button: **Action → Type: Page Navigation → Destination: [corresponding page]**
   - Default fill: `#2C1F1A`, text color: `#8A7560`
   - For THIS page's button: fill `#3D2B22`, text `#C8956C`
5. **Insert → Shapes → Rounded Rectangle** (active indicator)
   - Position: below the "Command Center" button, Size: `130×3`
   - Fill: `#C8956C`
6. **Insert → Buttons → Blank** (Prev/Next)
   - Position: `x=1130, y=10` and `x=1190, y=10`, Size: `50×28`
   - Text: "◀" and "▶"
   - Action: Page Navigation to previous/next pages

> [!TIP]
> After building the nav bar on Page 1, **select all nav elements → Ctrl+C → go to Page 2 → Ctrl+V**. Then just update which button is "active" on each page. This ensures pixel-perfect consistency.

### Step 4.3 — Title Section

1. **Insert → Text Box**: "A/B Test Results"
   - Position: `x=24, y=60`, 22pt Segoe UI Semibold, `#F5EDE4`
2. **Insert → Text Box**: Use `[Dynamic Title Page 1]` measure or static text
   - Position: `x=24, y=86`, 11pt, `#8A7560`

### Step 4.4 — Verdict Banner

1. **Insert → Shapes → Rounded Rectangle**
   - Position: `x=24, y=118`, Size: `1232×42`
   - Fill: `#2A1515` (dark red tint), Corner Radius: 8
2. **Insert → Card** visual (or textbox)
   - Place the `[Verdict Text]` measure inside
   - Font: 14pt Segoe UI Semibold, `#D4574E`
3. **Insert → Card** (secondary)
   - Place `[Verdict Detail]` measure
   - Font: 11pt, `#BFA98E`

### Step 4.5 — KPI Cards (6 cards)

For each KPI card:

1. **Insert → Shapes → Rounded Rectangle**
   - Size: `196×100`, Corner Radius: 10, Fill: `#1E1714`
   - Space them 8px apart starting at `x=24, y=172`
   - Positions: x=24, x=228, x=432, x=636, x=840, x=1044
2. **Insert → Card** visual on top of each rectangle
   - Drag the appropriate measure into "Fields"
   - Set background to transparent
   - Font: 28pt Segoe UI Bold, `#F5EDE4`
3. **Insert → Text Box** for label
   - Below the card value, 9pt, `#8A7560`
   - Labels: "Total Users", "Total Conversions", "Overall Rate", "Control Rate", "Treatment Rate", "Absolute Lift"
4. For the "Absolute Lift" card:
   - Add a sub-label with the `[Relative Lift Display]` measure
   - Color: `#D4574E` (conditional — use `[Lift Color]` measure)

### Step 4.6 — Conversion Comparison Chart (Left)

1. **Insert → Clustered Bar Chart**
   - Position: `x=24, y=284`, Size: `600×176`
2. **Configure:**
   - Y-Axis: `ab_data[group]`
   - X-Axis (Values): `[Conversion Rate]`
   - Title: "Conversion Rate by Group"
3. **Format:**
   - Data labels: ON, 10pt, `#E8C9A0`
   - Reference line: at 12.00% (average), color `#5C4033`, dashed
   - Colors: Control bar `#C8956C`, Treatment bar `#A8845E`
4. **Tooltip:** Assign Tooltip Page "Tooltip - Group"

### Step 4.7 — Country Snapshot Chart (Right)

1. **Insert → Clustered Bar Chart**
   - Position: `x=636, y=284`, Size: `620×176`
2. **Configure:**
   - Y-Axis: `countries[country_label]`
   - X-Axis (Values): `[Country Conversion Rate]`
   - Title: "Country Snapshot"
   - Sort by `country_sort`
3. **Format:** Same as above
4. **Tooltip:** Assign "Tooltip - Country"

### Step 4.8 — Sample Balance Indicator

1. **Insert → Shapes → Rounded Rectangle**
   - Position: `x=24, y=472`, Size: `1232×38`
   - Fill: `#1A2A1A` (dark green tint), Corner Radius: 8
2. **Insert → Card** with `[Sample Balance Display]` measure
   - Font: 10pt, `#7FB069`
   - Prefix: "✅ Sample Balanced  |  "

### Step 4.9 — Country Slicer

1. **Insert → Slicer**
   - Position: `x=24, y=518`, Size: `200×36`
   - Field: `countries[country_label]`
   - Style: Dropdown
   - Title: "Country"

### Step 4.10 — Edit Interactions

1. Select the Country Slicer → **Format → Edit Interactions**
2. Set all visuals to **Filter** (funnel icon)
3. Select the Conversion Comparison chart → set Country Snapshot to **None** (independent)

---

## Phase 5: Build Page 2 — Experiment Pulse (40 min)

### Step 5.1 — Create the Page

1. Click "+" at the bottom to add a new page
2. Rename to "Experiment"
3. Set canvas size: 1280×720
4. **Paste the navigation bar** from Page 1 (Ctrl+C → Ctrl+V)
5. Update the active button to "Experiment" (change its fill/text color + move the gold bar)

### Step 5.2 — Build Components

Follow the same pattern as Page 1 for the header area, then:

**Conversion Rate by Group (Clustered Bar — Horizontal)**
- Same as Page 1's left chart but larger: `600×210`
- Add a constant line at 12.00% (the overall rate) for reference
- Title: "Conversion Rate by Group"

**Experiment Scorecard (Table/Matrix)**
- Position: right column, `600×210`
- Columns: Metric, Control, Treatment
- Rows: Users, Conversions, Conversion Rate, Lift
- Use a Matrix visual with `group` as columns and measures as rows
- Conditional formatting: Lift row background → use `[Lift Color]`

**Conversion Funnel (100% Stacked Bar — Horizontal)**
- Position: left column, `600×176`
- Y-Axis: `group`
- Values: `[Total Conversions]`, `[Non Converted Users]`
- Title: "Conversion Funnel"
- Colors: Converted `#C8956C`, Not Converted `#2C1F1A`

**Sample Balance (Donut Chart)**
- Position: right column, `600×176`
- Values: `[Control Users]`, `[Treatment Users]`
- Legend shows "Control" and "Treatment"
- Title: "Sample Distribution"
- Inner label: "50/50"

**Key Takeaway (Textbox)**
- Rounded rectangle container, `1232×68`, fill `#1E1714`
- Text: the analyst's interpretation paragraph
- Font: 11pt Segoe UI, `#BFA98E`

---

## Phase 6: Build Page 3 — Geographic Lens (40 min)

### Build Components

**3 Country KPI Cards**
- Rounded rectangle `400×90` each, arranged horizontally
- Each contains: Flag + country name, conversion rate (large), user share (small)
- US card at `x=24`, UK card at `x=432`, CA card at `x=840`

**Lift by Country (Bar Chart — Horizontal)**
- Y-Axis: `countries[country_label]`
- Values: `[Country Lift]`
- Conditional formatting: positive bars `#7FB069`, negative bars `#D4574E`
- Add a constant line at 0

**User Distribution (Donut Chart)**
- Values: `[Total Users]` by `countries[country_label]`
- Colors: US `#C8956C`, UK `#A8845E`, CA `#8B6F5C`

**Country Performance Matrix (Table)**
- Full width, `1232×148`
- Columns: Country Label | Control Users | Treatment Users | Control CR | Treatment CR | Lift | Signal
- Conditional formatting on Lift column

**Analyst Note (Textbox)**
- Interpretation of geographic patterns

**Country Slicer**

---

## Phase 7: Build Page 4 — Deep Dive (40 min)

### Build Components

**Landing Page Performance (Bar Chart)**
- Y-Axis: `ab_data[landing_page]`
- Values: `[Conversion Rate]`

**Group × Country (Clustered Column Chart)**
- X-Axis: `countries[country_label]`
- Legend: `ab_data[group]`
- Values: `[Conversion Rate]`
- Colors: Control `#C8956C`, Treatment `#A8845E`

**Hypothesis Explorer (Textbox)**
- Rich formatted text with numbered hypotheses
- Background: `#1E1714`, rounded corners

**What-If Analysis (KPI Cards + Textbox)**
- Use `[Missed Conversions]`, `[Upside 1pp Conversions]`, `[Revenue Impact Display]`
- Layout: 2-3 cards inside a container

**Slicers**
- Group slicer (tile style) and Country slicer (dropdown)

---

## Phase 8: Build Page 5 — Strategic Brief (30 min)

This page is primarily textboxes and cards — minimal charts.

### Build Components

**Recommendation Panel**
- Large rounded rectangle, `1232×112`
- Left accent strip (thin rectangle, 4px wide, `#D4574E`)
- Use `[Recommendation Text]` measure in a card or textbox

**Key Findings (Textbox, left)**
**Next Steps (Textbox, right)**

**Impact Summary (4 KPI Cards)**
- Avoided Risk, Potential Upside, Experiment Cost, Data Quality
- Use `[Avoided Risk]`, `[Revenue Impact Display]`, `[Experiment Cost Display]`, `[Data Quality Score]`

**Footer (Textbox)**
- Use `[Footer Text]` measure

---

## Phase 9: Build Tooltip Pages (20 min)

### Tooltip — Country

1. **Insert → New Page** → Rename to "Tooltip - Country"
2. **Format → Page Information → Allow use as tooltip: ON**
3. **Canvas Settings → Type: Tooltip** (auto-sets to 320×240, resize to 320×280)
4. **Page background:** `#1E1714`
5. Add Card visuals using the `Tooltip Country *` measures:
   - `[Tooltip Country Name]` — 13pt, `#C8956C`, at top
   - `[Tooltip Country Users]`, `[Tooltip Country Conversions]`, `[Tooltip Country CR]`
   - `[Tooltip Country Share]`, `[Tooltip Country Contribution]`
   - `[Tooltip Country Control CR]`, `[Tooltip Country Treatment CR]`
   - `[Tooltip Country Lift]`
   - `[Country Signal]`
6. Add thin divider lines (1px rectangles, `#3D2B22`) between sections

### Tooltip — Group

Same pattern, using `Tooltip Group *` measures. Size: 320×220.

### Tooltip — KPI

Same pattern, using `Tooltip KPI *` measures. Size: 320×200.

### Assign Tooltips to Visuals

For each chart that should show a tooltip:
1. Select the visual
2. **Format → Tooltip → Type: Report Page**
3. **Page:** Select the appropriate tooltip page

---

## Phase 10: Polish & Finalize (30 min)

### Alignment Check
- Select multiple visuals → **Format → Align → Align Left / Distribute Horizontally**
- Ensure consistent spacing between all elements

### Conditional Formatting
For each visual with lift values:
1. Select the visual → **Format → Data Colors → Advanced Controls → fx**
2. **Format Style: Rules**
3. Rule 1: If value ≥ 0, color `#7FB069`
4. Rule 2: If value < 0, color `#D4574E`

### Hide Tooltip Pages
1. Right-click each tooltip page tab
2. Select **Hide Page**

### Tab Order (Accessibility)
1. **View → Selection Pane** (opens for current page)
2. Click **Tab Order** tab
3. Arrange elements in logical reading order (left-to-right, top-to-bottom)

### Mobile Layout (Optional)
1. **View → Mobile Layout**
2. Drag key visuals onto the mobile canvas
3. Focus on: Verdict Banner, KPI Cards, Conversion Comparison
4. Skip navigation (mobile layout auto-handles page tabs)

### Performance Check
1. **View → Performance Analyzer → Start Recording**
2. Click through each page
3. Check that each visual loads in under 500ms
4. If any are slow, simplify (reduce data, simplify DAX)

---

## Phase 11: Export & Save

### Save
- **File → Save** (saves the `.pbix` file)

### Export to PDF
- **File → Export → PDF** (all pages export as a multi-page PDF)
- Great for sharing with stakeholders who don't have Power BI

### Publish (Optional)
- **Home → Publish → Select workspace** (requires Power BI Pro or Premium)

---

## Screenshots Plan (for GitHub Portfolio)

Take these screenshots for your README:

| # | Screenshot | What to Capture | Suggested Filename |
|---|-----------|----------------|-------------------|
| 1 | Command Center (full page) | Page 1 with all KPIs visible | `01_command_center.png` |
| 2 | Command Center (with tooltip) | Hovering over a country bar | `02_command_center_tooltip.png` |
| 3 | Experiment Page | Page 2 showing scorecard + funnel | `03_experiment_pulse.png` |
| 4 | Geographic Lens | Page 3 with country matrix | `04_geographic_lens.png` |
| 5 | Deep Dive | Page 4 showing hypothesis explorer | `05_deep_dive.png` |
| 6 | Strategic Brief | Page 5 with recommendation | `06_strategic_brief.png` |
| 7 | Tooltip Detail | Close-up of a rich tooltip | `07_tooltip_detail.png` |
| 8 | Data Model | Model view showing star schema | `08_data_model.png` |
| 9 | DAX Measures | Fields pane showing organized folders | `09_dax_measures.png` |

> [!TIP]
> Use **Windows → Snipping Tool** or **Win+Shift+S** for clean screenshots. Crop tightly around the dashboard — remove any Power BI chrome for a cleaner look.
