# Dataset Documentation

## Source and Attribution

-   **Dataset:** ID_REG_KG_2511: Indonesian Legal Regulation Knowledge
    Graph
-   **Publisher:** Azzindani
-   **Source page:**
    https://huggingface.co/datasets/Azzindani/ID_REG_KG_2511
-   **Citation:** Azzindani. (2025). *ID_REG_KG_2511: Indonesian Legal
    Regulation Knowledge Graph*. Hugging Face Datasets.
-   **License stated by publisher:** CC BY 4.0,
    https://creativecommons.org/licenses/by/4.0/

## Local Source Dataset

The local source copy consists of 75 Parquet files with 748,558 rows and
41 columns. Its measured size was approximately 7.37 GiB.

  Column                Role observed during profiling
  --------------------- ------------------------------------------------
  `global_id`           Unique row identifier in the inspected dataset
  `regulation_type`     Regulation type
  `enacting_body`       Issuing authority text
  `regulation_number`   Regulation number
  `year`                Year recorded in the metadata
  `effective_date`      Effective-date text
  `chapter`             Chapter text or metadata
  `article`             Article text or metadata
  `about`               Description or summary text
  `content`             Regulation content text

A row is not necessarily one distinct regulation. Several rows may share
regulation metadata.

## Local Directories

The notebooks used these local development paths:

-   Source dataset: `C:\Users\raihan bhakti\id_reg_kg_2511`
-   Cleaned dataset: `C:\Users\raihan bhakti\id_reg_kg_2511_cleaned`

These paths are machine-specific. Update notebook variables when running
the project elsewhere.

The cleaned output consists of 75 Parquet files, approximately 2.92 GiB
in total. The JSON quality report is saved as `data_quality_report.json`
inside the cleaned output directory.

## Data Handling

The source files were kept separate from the cleaned output. Cleaning
was applied to a derived copy, and no rows were intentionally deleted.

The cleaned data includes normalized null values and audit flags for
selected anomalies, Potential encoding issues were flagged rather than
reconstructed. Texts of unusual length were retained because character
count alone does not establish that a text is invalid.

## Repository Policy

Do not commit raw or cleaned Parquet files to Git by default because
they occupy several gigabytes. Commit this documentation and the
notebooks instead. If required for submission, copy
`data_quality_report.json` into the project's `output/` directory.
