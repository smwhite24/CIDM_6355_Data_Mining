# CIDM 6355 — Data Mining Methods

Coursework repository. West Texas A&M University, Fall 2026, Dr. Liang (Leon) Chen.

Maintained as portfolio evidence for **CIDM 6395 Capstone**. The course does not require version control; this repository exists so the work is citable.

**Tools:** Altair RapidMiner AI Studio 2026.1.1 (Student), R and RStudio, MySQL, Excel.

---

## Class 2 — Data Preparation in RapidMiner and R

The same cleaning and integration job performed twice, once in each tool, so the two approaches can be compared directly on the same data.

**Data.** `census2000.csv`, US census figures keyed by ZIP code (RegionID, Longitude, Latitude, RegionDensityPercentile, RegionPopulation, MedianHouseholdIncome, AverageHouseholdSize), integrated with `US.txt`, a postal code reference from the geonames.org Free Postal Code Data set.

**Data quality problems addressed.**

| Problem | Cause | RapidMiner | R |
|---|---|---|---|
| Wrong data type | `MedianHouseholdIncome` read as polynominal because of thousands separators | Escape character set at import | `gsub(",","")` then `as.numeric()` |
| Missing values | `RegionDensityPercentile` holds `NaN` where land area is zero, making density undefined | Replace errors with missing values, then Filter Examples on `no_missing_attributes` | `na.omit()` |
| Duplicates | Repeated records in the source | Remove Duplicates operator, all attributes | `!duplicated()` |
| Noise | Clerical errors left letters in numeric ZIP codes | Replace operator, regex `[A-Z]` to `0` | `gsub("[A-Z]","0")` |
| Integration | Key type mismatch between `RegionID` and the postal code field | Join operator, inner, after type reconciliation | `dplyr::inner_join()` after `as.character()` |

**Finding worth recording.** Before deleting records with missing values, the dropped set was routed to a separate output and inspected. `RegionPopulation`, `MedianHouseholdIncome`, and `AverageHouseholdSize` were zero in every dropped record, meaning those regions had no residents. Deletion was therefore the right call and mean imputation would have invented population that did not exist.

**Record counts.**

| Stage | R | RapidMiner |
|---|---|---|
| Raw import | | |
| After duplicate removal | | |
| After missing-value removal | | |
| Combined (inner join) | | |

> Note: the two labs apply these steps in different orders. RapidMiner removes duplicates before filtering missing values; the R script runs `na.omit()` before `duplicated()`. Intermediate counts are therefore measured against different base sets and are not expected to match.

---

## Class 3 — Classification (Decision Tree)

Decision tree built in RapidMiner against `iris_train` and applied to `iris_predict` with Apply Model. Splits on `Petal_width` at the root, then `Petal_width` and `Petal_length`, separating Setosa cleanly and distinguishing Versicolor from Virginica in the lower branches.

---

## A note on course materials

Lab instructions, slide decks, and datasets distributed by the instructor are **not** included in this repository. The course syllabus reserves copyright on all course materials and prohibits redistribution. Only my own scripts, exported processes, and result screenshots appear here.
