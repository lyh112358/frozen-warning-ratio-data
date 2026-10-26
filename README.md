# Frozen warning-ratio modeling tables

These tables are the inputs of one study: a frozen warning ratio for cross-semester academic risk prediction. Each source has its own directory. Nothing from other studies is included.

The identifier in the institutional tables is a one-way hash. The tables do not contain names, student numbers, class, college, or gender.

## Files

`private/T{t}_{year}.csv` is one cohort of an undergraduate programming course.

`oulad/T{t}_{presentation}.csv` pools the Open University offerings used in the study. `presentation` is `2013B`, `2013J`, `2014B`, or `2014J`.

`t` is the observation window and is 2, 3, 4, or 5. The term is divided into six equal stages. Window `t` uses the first `t` stages.

Both files have the same roles for the first and last columns:

| Column | Meaning |
|---|---|
| `SID` | Row key. Institutional values are 32-hex hashes. OULAD values are the published student key, then `::`, then the offering code such as `BBB_2013J`. |
| `grade_label` | 1 = fail, 0 = pass. Withdrawn students are not in the tables. |

The middle columns are process features: counts and score summaries, learning engagement, learning speed, learning acceleration, relative progress, completion rate, and cross-stage differences of those quantities. A missing score is left missing. It is not filled with zero.

Columns that carry no training information are already removed. On the institutional course, a column is removed when the training years contain no finite value. On OULAD, a column is removed when it is constant on the pooled training presentations. The resulting widths are:

| Window | Institutional course | OULAD |
|---|---:|---:|
| T=2 | 116 | 115 |
| T=3 | 232 | 231 |
| T=4 | 389 | 376 |
| T=5 | 581 | 548 |

## Splits

Institutional course. Training is 2020–2023 together (3,633 students, 140 fails). The warning ratio is frozen on 2024 (871 students, 85 fails). 2025 is the single test cohort (910 students, 93 fails).

OULAD. Training pools `2013B` (3,336 rows, 1,241 fails) and `2013J` (6,153 rows, 1,956 fails). The ratio is frozen on `2014B` (4,153 rows, 1,458 fails). `2014J` is the single test presentation (5,714 rows, 1,525 fails). The offerings inside those presentations are BBB, DDD, and FFF, plus EEE and GGG where that presentation has them.

The same student can appear in more than one OULAD offering. The offering code in `SID` keeps those rows distinct.

## Checksums

`MANIFEST.sha256` lists SHA-256 for every csv. The paths use forward slashes.

## Provenance

The OULAD tables are derived from the Open University Learning Analytics dataset (Kuzilek, Hlosta, and Zdrahal, Scientific Data, 2017), which is distributed under CC BY 4.0. The institutional tables are a research release of hashed course records for this study only.

## What this release is not

It is not a dump of every experiment in the private working repository. Knowledge tracing, transfer runs, and other tasks are absent. Raw click logs are absent.
