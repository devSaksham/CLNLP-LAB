# Experiment-1: Data Analysis on Employee Dataset

## Overview

This experiment performs exploratory data analysis (EDA) on an employee
dataset using **Python, pandas, and matplotlib**. The goal is to load
structured tabular data, summarize it with grouping and aggregation, filter
it based on conditions, and visualize key patterns (salary, department,
gender, age, and experience) using bar charts, pie charts, histograms, and
scatter plots.

## Dataset

- **Name:** `employee_information_100`
- **Format:** CSV
- **Rows:** 100 employee records
- **Columns:**

| Column | Description |
|---|---|
| `Employee_ID` | Unique identifier for each employee (e.g. `E001`) |
| `Name` | Employee name |
| `Department` | Department the employee works in (e.g. HR, Operations) |
| `Age` | Employee's age |
| `Gender` | Male / Female |
| `Salary` | Annual salary |
| `Experience_Years` | Total years of work experience |

## What This Experiment Covers

| Task | Description | Technique Used |
|---|---|---|
| 1.1 | Average salary of each department | `groupby()` + `mean()` → bar chart |
| 1.2 | Number of employees per department | `value_counts()` |
| 1.3 | Percentage of male vs female employees | `value_counts()` → pie chart |
| 1.4 | Salary distribution | Histogram |
| 1.5 | Relationship between experience and salary | Scatter plot |
| 1.6 | Top 10 highest-paid employees | `sort_values()` + `head()` |
| 1.7 | Highest salary in every department | `groupby()` + `max()` |
| 1.8 | Employees earning above the overall average salary | Boolean indexing/filtering |
| 1.9 | Average years of experience per department | `groupby()` + `mean()` |
| 1.10 | Distribution of employee ages | Histogram |

## Files

- `Experiment-1_Employee_Data_Analysis.ipynb` — the Jupyter notebook with all 10 tasks, each in its own labeled markdown + code cell.
- `Viva_Explanation_Guide.md` — a line-by-line explanation of every cell, for exam/viva prep.
- `employee_information_100.csv` — the dataset (place this in the same folder as the notebook, or update the `DATA_PATH` variable in the "Load Dataset" cell).

## How to Run

1. Make sure Python 3 is installed, along with `pandas`, `matplotlib`, and `jupyter`:
   ```bash
   pip install pandas matplotlib jupyter
   ```
2. Place `employee_information_100.csv` in the same directory as the notebook
   (or update `DATA_PATH` in the "Load Dataset" cell to point to its location).
3. Launch Jupyter and open the notebook:
   ```bash
   jupyter notebook Experiment-1_Employee_Data_Analysis.ipynb
   ```
4. Run all cells from top to bottom (**Cell → Run All**, or `Shift+Enter`
   through each cell in order).

## Learning Outcomes

By completing this experiment, you should be able to:
- Load and inspect a tabular dataset with pandas (`read_csv`, `head`, `info`).
- Group and aggregate data by category (`groupby`, `mean`, `max`).
- Count occurrences of categorical values (`value_counts`).
- Filter rows using conditional/boolean indexing.
- Sort and rank data (`sort_values`, `head`).
- Choose the right chart type for the question being asked:
  - **Bar chart** — comparing values across categories.
  - **Pie chart** — showing proportions/percentages of a whole.
  - **Histogram** — showing the distribution of a continuous numeric variable.
  - **Scatter plot** — showing the relationship between two numeric variables.

## Tools Used

- **Python 3**
- **pandas** — data loading, grouping, filtering, aggregation
- **matplotlib** — data visualization
- **Jupyter Notebook** — interactive execution environment
