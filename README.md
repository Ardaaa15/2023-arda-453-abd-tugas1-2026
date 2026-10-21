# Indonesian Regulation Dataset: Data Profiling and Cleaning.

## Project Overview

This project documents two stages of a Big Data workflow using the
**ID_REG_KG_2511: Indonesian Legal Regulation Knowledge Graph** dataset:
**Milestone 1 (Data Profiling)** and **Milestone 2 (Data Cleaning)**.
Exploratory Data Analysis (EDA) is outside the current scope.

## Dataset Source and Attribution.

-   **Dataset:** ID_REG_KG_2511: Indonesian Legal Regulation Knowledge
    Graph
-   **Publisher:** Azzindani
-   **Source:** [Hugging Face dataset
    repository](https://huggingface.co/datasets/Azzindani/ID_REG_KG_2511)
-   **Citation:** Azzindani. (2025). *ID_REG_KG_2511: Indonesian Legal
    Regulation Knowledge Graph*. Hugging Face Datasets.
-   **License stated by the publisher:** [Creative Commons Attribution
    4.0 International (CC BY
    4.0)](https://creativecommons.org/licenses/by/4.0/)

Retain attribution when reusing or redistributing the dataset and review
the original dataset card for full details.

## Dataset Overview

  Attribute                                       Value
  ---------------------------- ------------------------
  Local source Parquet files                         75
  Rows                                          748,558
  Columns                                            41
  Local source size              Approximately 7.37 GiB
  Cleaned output size            Approximately 2.92 GiB
  Format                                 Apache Parquet

The dataset includes regulatory metadata, text, and
knowledge-graph-related attributes. Fields inspected include
`global_id`, `regulation_type`, `enacting_body`, `regulation_number`,
`year`, `effective_date`, `chapter`, `article`, `about`, and `content`.

A row should not automatically be interpreted as one distinct
regulation. Multiple rows can share regulation metadata while
representing different records or chunks.

## Repository Structure

``` text
project/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   └── README.md
├── notebooks/
│   ├── 01_data_profiling.ipynb
│   └── 02_data_cleaning.ipynb
├── output/
│   └── data_quality_report.json
└── docs/
    └── milestone_summary.md
```

Large raw and cleaned Parquet files should remain outside Git by
default. The `.gitignore` excludes Parquet files to prevent accidentally
pushing several gigabytes of data.

## Milestone 1: Data Profiling

Notebook: `notebooks/01_data_profiling.ipynb`

The profiling work covered dataset size and schema, selected-column null
and empty-string checks, `global_id` uniqueness, year and category
distributions, text-length statistics, IQR-based candidate outliers,
date examples, and unusually long text values.

### Main findings

-   748,558 rows across 75 Parquet files and 41 columns.
-   Exact checks found 748,558 unique `global_id` values with no null or
    repeated IDs.
-   No nulls or empty strings were found in the selected fields audited
    initially. Literal `Unknown` values were checked separately.
-   567 rows had year `0000`; 2 rows had year `2030`.
-   `regulation_type` contained 79,764 literal `Unknown` values.
-   `enacting_body` contained 64,521 literal `Unknown` values.
-   89,877 rows were identified as candidate `content`-length outliers
    under the IQR rule used.
-   3,615 rows had `content` length exactly 32,767 characters, and 99
    rows had `about` length exactly 32,767 characters.

Unusually long text is an indicator for further inspection, not proof of
truncation.

## Milestone 2: Data Cleaning

Notebook: `notebooks/02_data_cleaning.ipynb`

The workflow created a separate cleaned copy and retained all rows.
Implemented rules include:

-   Trim surrounding whitespace from selected text fields and convert
    empty strings to null.
-   Convert literal `Unknown` values in `regulation_type` and
    `enacting_body` to null.
-   Convert year `0000` to null and record its status in `year_status`.
-   Retain year `2030` and flag it as `future_year_check`.
-   Convert missing, blank, or `Unknown` `effective_date` values to null
    and record `effective_date_status`.
-   Add `enacting_body_encoding_issue` for values containing `�`,
    without inventing replacement text.
-   Preserve row count and `global_id` values. No rows were
    intentionally deleted.

### Final validation

  Check                                                                 Result
  --------------------------------------------------- ------------------------
  Rows                                                                 748,558
  Unique `global_id` values                                            748,558
  Null `global_id` values                                                    0
  Repeated `global_id` values                                                0
  Null years after normalizing `0000`                                      567
  Remaining literal `Unknown` in `regulation_type`                           0
  Remaining literal `Unknown` in `enacting_body`                             0
  Remaining literal `Unknown` in `effective_date`                            0
  `effective_date` values marked unknown or missing                    259,467
  Rows flagged for `�` in `enacting_body`                                   61
  Year `2030` retained and flagged                                           2
  Year `0000` flagged                                                      567
  Cleaned output size                                   Approximately 2.92 GiB

All assertions in the final validation cell passed for the checks listed
above. The JSON report is saved as `data_quality_report.json` in the
cleaned output directory.

## Limitations and Data Integrity

-   Year `2030` was retained and flagged rather than assumed incorrect.
-   The replacement character `�` may indicate an upstream extraction or
    encoding issue. The intended text was not reconstructed without
    source verification.
-   The repeated maximum text length of 32,767 characters may indicate a
    length cap, but source-level comparison is needed to confirm
    truncation.
-   Long text values are not automatically errors because legal
    documents vary in length.
-   The source dataset remains separate from the transformed output for
    reproducibility.

## Environment Setup

Install dependencies:

``` bash
python -m pip install -r requirements.txt
```

Start JupyterLab:

``` bash
jupyter lab
```

Open the notebooks in `notebooks/` and update the dataset paths for the
local environment. Large Parquet files are not included in this
repository by default.

## Current Status

-   **Milestone 1 --- Data Profiling:** core profiling checks and
    findings documented.
-   **Milestone 2 --- Data Cleaning:** transformations and final
    validation completed for the local cleaned dataset.
-   **Milestone 3 --- EDA:** outside the current submission scope.


## Submission Scope

This submission covers Milestone 1 (Data Profiling) and Milestone 2 (Data Cleaning). Milestone 3 (Exploratory Data Analysis) is outside the current submission scope.

