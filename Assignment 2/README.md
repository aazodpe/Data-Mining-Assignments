# Automated Data Analysis System

This repository contains a compact automated data analysis system implemented as a Jupyter Notebook (`demo.ipynb`). The core is the `analyze_dataset()` function which performs a full exploratory data analysis (EDA) pipeline end-to-end and generates human-readable insights and visualizations automatically.

---

## What this project does

- Loads a dataset (pandas DataFrame) and prints a dataset overview (shape, dtypes, preview).
- Produces a missing values summary and basic data quality checks (duplicates, single-value columns, high-missing columns).
- Computes descriptive statistics for numeric and categorical columns (mean, median, std, IQR, outliers via 1.5×IQR rule).
- Generates multiple visualizations (histogram, boxplot, bar chart, scatterplot, correlation heatmap, optional additional categorical bar).
- Writes 10 concise, actionable insights and a short limitations/bias section for the dataset.
- Saves visualizations to `output/{Dataset_Name}_analysis.png`.

The notebook demonstrates the system on two example datasets included or referenced in the notebook:
- Titanic dataset (remote CSV)
- Fruit Prices 2023 (local CSV: `data/fruit-prices-2023.csv`)

---

## Files

- `demo.ipynb` — main notebook including the `analyze_dataset()` function and example runs.
- `requirements.txt` — Python dependencies required to run the notebook.
- `data/fruit-prices-2023.csv` — example dataset (provided in `data/`).
- `output/` — directory where generated PNG visualizations are saved.

---

## Installation

1. (Optional) Create and activate a virtual environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Open `demo.ipynb` in VS Code / Jupyter and run the cells.

---

## How to use

Open `demo.ipynb` and run the cells in order (imports, load data, function definition, then analysis calls). Two sample calls in the notebook are:

```python
# Analyze Titanic with custom plot columns
analyze_dataset(titanic_df, "Titanic Dataset", plot_columns={'numeric': ['Age', 'Fare'], 'categorical': ['Sex', 'Pclass']})

# Analyze Fruit Prices with auto-detected columns
analyze_dataset(fruit_df, "Fruit_Prices_2023")
```

### `analyze_dataset(df, dataset_name, plot_columns=None)`

- `df` (pandas.DataFrame): The dataset to analyze.
- `dataset_name` (str): Friendly name used in printed output and saved image filename.
- `plot_columns` (dict or None): Optional override to select columns for plotting. Format:
  - `{'numeric': ['col1', 'col2', ...], 'categorical': ['cat1', ...]}`
  - If a requested column is missing or wrong dtype it will be ignored and a note will be printed.
  - If `plot_columns` is not provided, the function auto-detects numeric and categorical columns.

What the function prints and saves:
- Dataset overview (shape, dtypes, head)
- Missing values summary and data-quality checks
- Descriptive statistics for numeric and categorical features
- Visualizations (saved to `output/{dataset_name}_analysis.png`)
- 10 human-readable insights
- Limitations & potential bias notes


### Visualization behavior

The function computes the exact number of plots to generate and creates a grid large enough to hold them. Visualizations include:
- Histogram (first numeric column)
- Boxplot (first two numeric columns)
- Bar chart (first categorical column)
- Scatterplot (first 2 numeric columns, if present)
- Correlation heatmap (for selected numeric columns if >= 2 present)
- Additional categorical bar chart (if >= 2 categorical columns requested/detected)

All plots are shown inline and saved to the `output` directory.

---

## Examples and interpretation (short)

- Titanic: Right-skewed age distribution; fare outliers indicate first-class passengers; weak age-fare correlation suggests class/cabin drive price.
- Fruit Prices: Price distribution clustered around $1-$3; strong negative size-price correlation (smaller items cost more per unit); fresh form dominates.

(Full textual insights are printed by the function and also stored in the notebook output.)

---

## Troubleshooting

- If plots attempt to place more subplots than the calculated grid, re-run the function definition cell to ensure the latest plotting logic is loaded.
- If a column you requested in `plot_columns` is ignored, check the dtype: numeric columns must be numeric and categorical columns must be non-numeric (object/string-like).
- If the notebook cannot find `data/fruit-prices-2023.csv`, verify your working directory and that `data/` exists relative to the notebook.

---

## Extending the system

- Add more plot types (pairplot, violin plot, KDE plots) in the visualization section.
- Persist insights to a JSON or Markdown file for programmatic consumption.
- Add CLI wrapper to call `analyze_dataset` on arbitrary CSV files from the command line.

---

## Contributing

Contributions and suggestions welcome. Open an issue or submit a pull request describing the change and a short rationale.

---

## License

This repository does not include a license file by default. Add an appropriate license file such as MIT or Apache-2.0 if you intend to share publicly.

---

If you'd like, I can also:
- add a small CLI wrapper script to run `analyze_dataset` on arbitrary CSVs,
- export the generated textual insights to a Markdown file beside the PNG, or
- add unit tests for the plotting logic.

Which of those would you like next?