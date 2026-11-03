# Obesity Checks Dashboard — README

## Project Overview

This Power BI project explores the **Obesity Dataset** using demographic, dietary, lifestyle, activity, and transportation variables. The dashboard is intended for descriptive analysis: it helps users explore how recorded characteristics vary across obesity-status classes.

**Dataset size:** 1,610 records and 15 columns.

> This report is for educational and exploratory data analysis only. It does not diagnose obesity or establish that a lifestyle factor causes a particular weight-status class.

## Project Files

- `Obesity_Dataset.xlsx` — source workbook, including the dataset and a code/value explanation sheet.
- `OB.pbit` — Power BI template/report.
- `README.md` — project documentation.

## Dataset Description

The main worksheet contains 1,610 rows and the following 15 attributes:

| Column | Description |
|---|---|
| `Sex` | Encoded sex category |
| `Age` | Age in years |
| `Height` | Height in centimeters |
| `Overweight_Obese_Family` | Whether overweight/obesity in the family is reported |
| `Consumption_of_Fast_Food` | Fast-food consumption indicator |
| `Frequency_of_Consuming_Vegetables` | Vegetable consumption frequency |
| `Number_of_Main_Meals_Daily` | Main meals per day category |
| `Food_Intake_Between_Meals` | Between-meal food intake frequency |
| `Smoking` | Smoking indicator |
| `Liquid_Intake_Daily` | Daily liquid-intake category |
| `Calculation_of_Calorie_Intake` | Whether calorie intake is calculated |
| `Physical_Excercise` | Physical exercise frequency (source spelling retained) |
| `Schedule_Dedicated_to_Technology` | Daily technology-use duration category |
| `Type_of_Transportation_Used` | Transportation category |
| `Class` | Weight-status class |

Many categorical fields are stored as numeric codes. Refer to the workbook's **Explanation** sheet to interpret them. For example, `Class` is coded as 1 = Underweight, 2 = Normal, 3 = Overweight, and 4 = Obesity. Do not interpret the codes as continuous measurements.

## Workflow

### 1. Import the Excel data

1. Open Power BI Desktop.
2. Select **Home → Get data → Excel**.
3. Select `Obesity_Dataset.xlsx`.
4. In Navigator, select the `Obesity_Dataset` worksheet.
5. Choose **Transform Data** to open Power Query Editor.

### 2. Clean and prepare data in Power Query Editor

The data was prepared in Power Query before creating the report. A typical cleaning workflow is:

1. **Verify headers and columns** — confirm the field names match the expected dataset attributes.
2. **Set data types**:
   - `Age`, `Height`, and encoded category fields: Whole Number
   - Keep category codes as whole numbers only if they will be decoded or used as categorical fields; otherwise convert them to Text or assign descriptive labels.
3. **Check missing values** — identify blanks and decide whether they are valid missing responses or data issues. Avoid replacing blanks with zero unless zero has a valid meaning.
4. **Check duplicates** — inspect repeated rows and remove them only when they are confirmed accidental duplicates.
5. **Validate ranges and codes** — check that age and height are plausible and category codes fall within the values listed in the workbook's Explanation sheet.
6. **Improve readability** — optionally rename columns for display and replace numeric codes with descriptive labels using Replace Values or conditional columns. Keep a clear mapping to the original values.
7. Select **Home → Close & Apply** to load the cleaned data into the model.

The precise applied steps are stored in Power Query in the PBIX/PBIT file. Update this section with any additional transformations you performed so the README reflects your exact process.

### 3. Create measures in Data Modeling

After loading the cleaned table, create measures from **Modeling → New measure**. Measures are calculated dynamically and respond to slicers and visual filters.

Example DAX measures (adapt the table name if yours differs):

```DAX
Total Records =
COUNTROWS('Obesity_Dataset')

Average Age =
AVERAGE('Obesity_Dataset'[Age])

Average Height (cm) =
AVERAGE('Obesity_Dataset'[Height])

Obesity Class Records =
CALCULATE(
    [Total Records],
    'Obesity_Dataset'[Class] = 4
)

Obesity Class Share =
DIVIDE([Obesity Class Records], [Total Records], 0)
```

These are examples for documentation and may not match the exact measures authored in your report. Add your actual measure names and formulas here for a complete technical record.

**Modeling note:** Do not sum encoded categories such as `Sex`, `Class`, exercise frequency, or transportation type and interpret the result as a meaningful total. For categorical analysis, use counts, percentages, or descriptive labels. Use averages only for genuinely quantitative fields such as age and height.

## Dashboard Explanation

The report page is titled **OBESITY CHECKS** and includes KPI cards, an age slicer, charts, and a map visual. The visuals are designed to support exploration of the dataset by age and lifestyle/demographic attributes.

- **KPI cards:** provide quick summaries of selected fields for the current filter context. Confirm each card's field and aggregation in the report before describing it as a count, average, or percentage.
- **Age slicer:** filters the visuals to the selected age or age range.
- **Bar chart:** compares selected lifestyle variables, including exercise and vegetable-consumption categories.
- **Column chart:** compares selected demographic and lifestyle fields.
- **Line chart:** presents selected fields across age values; use it to inspect patterns, not causal relationships.
- **Combo chart:** combines multiple measures/fields to compare patterns in one visual.
- **Filled map:** provides a geographic-style visual. The source dataset does not include a geographic location field, so verify the map's configured location and meaning before using it for geographic conclusions. If no valid geographic data is available, replace it with a suitable categorical chart.

### How to Use the Dashboard

1. Open the report and review the **OBESITY CHECKS** page.
2. Use the Age slicer to focus on a particular age or age group.
3. Select chart elements to cross-filter other visuals, if interactions are enabled.
4. Hover over data points to inspect tooltips.
5. Clear slicer selections to return to the full dataset.
6. Compare categories using counts or percentages rather than sums of category codes.

## Interpretation and Limitations

- The dataset is observational; associations do not demonstrate causation.
- Category codes are labels, not numeric scales. Their sum is generally not meaningful.
- The dataset's `Class` field represents the provided weight-status category; this README does not independently validate its calculation.
- The dataset contains no explicit geographic field in the listed 15 columns. Avoid geographic interpretations unless the report uses a valid additional location source.
- Review the report's visual-level aggregations and replace any inappropriate sums of coded categories with counts, percentages, or appropriate measures.

## Source

Dataset citation provided in the workbook:

Koklu, N., & Sulak, S.A. (2024). *Using artificial intelligence techniques for the analysis of obesity status according to the individuals' social and physical activities.* Sinop Üniversitesi Fen Bilimleri Dergisi, 9(1), 217–239. https://doi.org/10.33484/sinopfbd.1445215
