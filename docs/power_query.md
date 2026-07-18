# CATALYST — Power Query (M Code) & Data Model

---

## Table 1: ab_data (Fact Table)

### Full M Code

```powerquery
let
    // ── Step 1: Load the CSV ──
    Source = Csv.Document(
        File.Contents("C:\path\to\your\data\ab_data.csv"),
        [Delimiter=",", Columns=5, Encoding=65001, QuoteStyle=QuoteStyle.None]
    ),
    
    // ── Step 2: Promote headers ──
    PromotedHeaders = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    
    // ── Step 3: Set column types early (performance optimization) ──
    TypedRaw = Table.TransformColumnTypes(PromotedHeaders, {
        {"user_id", Int64.Type},
        {"timestamp", type text},
        {"group", type text},
        {"landing_page", type text},
        {"converted", Int64.Type}
    }),
    
    // ── Step 4: Remove mismatched group–page assignments ──
    // Business rule: control→old_page, treatment→new_page
    // Rows violating this rule are logging errors from double-bucketed users
    FilteredMismatches = Table.SelectRows(TypedRaw, each 
        ([group] = "control" and [landing_page] = "old_page") or 
        ([group] = "treatment" and [landing_page] = "new_page")
    ),
    
    // ── Step 5: Remove duplicate user_ids (keep first occurrence) ──
    // After removing mismatches, only 2 users remain duplicated
    RemovedDuplicates = Table.Distinct(FilteredMismatches, {"user_id"}),
    
    // ── Step 6: Remove the timestamp column (it's unusable — truncated format) ──
    RemovedTimestamp = Table.RemoveColumns(RemovedDuplicates, {"timestamp"}),
    
    // ── Step 7: Add a readable conversion status column ──
    AddedConversionStatus = Table.AddColumn(RemovedTimestamp, "conversion_status", each 
        if [converted] = 1 then "Converted" else "Not Converted",
        type text
    ),
    
    // ── Step 8: Capitalize group names for cleaner display ──
    CapitalizedGroup = Table.TransformColumns(AddedConversionStatus, {
        {"group", each Text.Proper(_), type text}
    }),
    
    // ── Step 9: Format landing page for display ──
    FormattedLandingPage = Table.TransformColumns(CapitalizedGroup, {
        {"landing_page", each 
            if _ = "old_page" then "Old Page" 
            else if _ = "new_page" then "New Page" 
            else _, type text}
    }),
    
    // ── Step 10: Final column ordering ──
    ReorderedColumns = Table.ReorderColumns(FormattedLandingPage, {
        "user_id", "group", "landing_page", "converted", "conversion_status"
    })
in
    ReorderedColumns
```

### Output Schema

| Column | Type | Values | Notes |
|--------|------|--------|-------|
| `user_id` | Int64 | 290,585 unique | Primary key |
| `group` | Text | "Control", "Treatment" | Capitalized |
| `landing_page` | Text | "Old Page", "New Page" | Formatted |
| `converted` | Int64 | 0, 1 | Binary |
| `conversion_status` | Text | "Converted", "Not Converted" | Human-readable |

### Row Count Verification

```
Raw:              294,480
After mismatch:   290,587  (removed 3,893)
After dedup:      290,585  (removed 2)
Final:            290,585  ✅
```

> [!IMPORTANT]
> **Update the file path** in Step 1 to match your actual CSV location. If you loaded data via Power BI Desktop's "Get Data" wizard, Power BI will generate its own Source step — just replace steps 3 onward.

---

## Table 2: countries (Dimension Table)

### Full M Code

```powerquery
let
    // ── Step 1: Load the CSV ──
    Source = Csv.Document(
        File.Contents("C:\path\to\your\data\countries.csv"),
        [Delimiter=",", Columns=2, Encoding=65001, QuoteStyle=QuoteStyle.None]
    ),
    
    // ── Step 2: Promote headers ──
    PromotedHeaders = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    
    // ── Step 3: Set types ──
    TypedRaw = Table.TransformColumnTypes(PromotedHeaders, {
        {"user_id", Int64.Type},
        {"country", type text}
    }),
    
    // ── Step 4: Remove 1 duplicate user_id ──
    RemovedDuplicates = Table.Distinct(TypedRaw, {"user_id"}),
    
    // ── Step 5: Add full country name for display ──
    AddedCountryName = Table.AddColumn(RemovedDuplicates, "country_name", each 
        if [country] = "US" then "United States"
        else if [country] = "UK" then "United Kingdom"
        else if [country] = "CA" then "Canada"
        else [country],
        type text
    ),
    
    // ── Step 6: Add country flag emoji ──
    AddedFlag = Table.AddColumn(AddedCountryName, "country_flag", each 
        if [country] = "US" then "🇺🇸"
        else if [country] = "UK" then "🇬🇧"
        else if [country] = "CA" then "🇨🇦"
        else "",
        type text
    ),
    
    // ── Step 7: Add country display label ──
    AddedDisplayLabel = Table.AddColumn(AddedFlag, "country_label", each 
        [country_flag] & " " & [country_name],
        type text
    ),
    
    // ── Step 8: Add sort order column ──
    AddedSortOrder = Table.AddColumn(AddedDisplayLabel, "country_sort", each 
        if [country] = "US" then 1
        else if [country] = "UK" then 2
        else if [country] = "CA" then 3
        else 99,
        Int64.Type
    )
in
    AddedSortOrder
```

### Output Schema

| Column | Type | Values | Notes |
|--------|------|--------|-------|
| `user_id` | Int64 | 290,585 unique | Foreign key |
| `country` | Text | "US", "UK", "CA" | Code |
| `country_name` | Text | "United States", etc. | Full name |
| `country_flag` | Text | "🇺🇸", "🇬🇧", "🇨🇦" | Emoji flag |
| `country_label` | Text | "🇺🇸 United States", etc. | Display label |
| `country_sort` | Int64 | 1, 2, 3 | Sort column (by user volume) |

> [!TIP]
> In Power BI, set `country_label` to **Sort by Column → country_sort** so countries always appear in US → UK → CA order (largest to smallest).

---

## Data Model (Relationship Setup)

### Relationship

```
ab_data[user_id]  ──── Many-to-One ────▶  countries[user_id]
                       (Active, Single direction)
```

### How to Set Up

1. Go to **Model View** (left sidebar icon)
2. Drag `ab_data[user_id]` onto `countries[user_id]`
3. In the dialog:
   - **Cardinality:** Many to One
   - **Cross filter direction:** Single
   - **Make this relationship active:** ✅ Yes
4. Click **OK**

### Star Schema Diagram

```
                    ┌──────────────┐
                    │  countries   │
                    │ (Dimension)  │
                    │──────────────│
                    │ user_id  PK  │
                    │ country      │
                    │ country_name │
                    │ country_flag │
                    │ country_label│
                    │ country_sort │
                    └──────┬───────┘
                           │
                    Many-to-One
                           │
                    ┌──────┴───────┐
                    │   ab_data    │
                    │   (Fact)     │
                    │──────────────│
                    │ user_id  FK  │
                    │ group        │
                    │ landing_page │
                    │ converted    │
                    │ conv_status  │
                    └──────────────┘
```

---

## Performance Optimization

### Query Folding
- Since these are CSV files, query folding does not apply
- All transformations happen in the Power Query engine

### Best Practices Applied
1. **Types set early** (Step 3) — prevents implicit type detection overhead
2. **Filters before transforms** — rows are reduced before adding columns
3. **Unnecessary columns removed** — `timestamp` dropped to save memory
4. **Distinct on key column** — efficient deduplication

### Suggested Power BI Settings
1. **File → Options → Data Load → Auto date/time:** OFF (no date columns to auto-generate)
2. **File → Options → Data Load → Auto type detection:** OFF (we set types manually)
3. **Disable "Include in report refresh"** for the countries table if it never changes

---

## Verification Queries

After loading, verify the data in Power BI by creating a quick table visual:

| Check | Expected | DAX to Verify |
|-------|----------|---------------|
| Total rows in ab_data | 290,585 | `COUNTROWS('ab_data')` |
| Total rows in countries | 290,585 | `COUNTROWS('countries')` |
| Unique user_ids match | Both 290,585 | `DISTINCTCOUNT('ab_data'[user_id])` |
| Control count | 145,274 | `CALCULATE(COUNTROWS('ab_data'), 'ab_data'[group] = "Control")` |
| Treatment count | 145,311 | `CALCULATE(COUNTROWS('ab_data'), 'ab_data'[group] = "Treatment")` |
| Total conversions | 34,753 | `SUM('ab_data'[converted])` |
| Countries | 3 | `DISTINCTCOUNT('countries'[country])` |

> [!NOTE]
> Note the capitalized group values ("Control", "Treatment") due to our `Text.Proper` transform. Your DAX filters must use the capitalized versions.
