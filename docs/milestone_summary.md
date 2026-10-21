# Milestone 1--2 Summary

## Milestone 1: Data Profiling

The local dataset contained 75 Parquet files, 748,558 rows, and 41
columns, with a measured source size of approximately 7.37 GiB. Exact
checks found 748,558 unique `global_id` values, with no null or repeated
IDs.

Profiling found 567 rows with year `0000`, 2 rows with year `2030`,
79,764 literal `Unknown` values in `regulation_type`, and 64,521 literal
`Unknown` values in `enacting_body`. The IQR rule identified 89,877
candidate outliers by `content` length. A total of 3,615 rows had
`content` length exactly 32,767 characters and 99 rows had `about`
length exactly 32,767 characters.

## Milestone 2: Data Cleaning

The cleaning workflow produced a separate output copy and retained all
748,558 rows. It normalized selected empty strings and `Unknown` values,
converted year `0000` to null, retained and flagged year `2030`,
normalized unknown or missing `effective_date` values, and added an
encoding-issue flag for `enacting_body` values containing `�`.

Final validation reported 748,558 unique `global_id` values, zero null
or repeated IDs, 567 null years, zero remaining literal `Unknown` values
in the three normalized columns, 259,467 `effective_date` values marked
unknown or missing, and 61 rows flagged for the replacement character in
`enacting_body`. All assertions in the final validation cell passed.

## Limitations

The 61 encoding flags were not repaired because the intended original
text was not verified. The maximum text length of 32,767 characters may
indicate truncation, but does not prove it. The two rows with year
`2030` were retained for review. No rows were intentionally deleted.

## Current Scope

This submission documents Milestone 1 (Data Profiling) and Milestone 2
(Data Cleaning). Milestone 3 (EDA) is outside the current scope.
