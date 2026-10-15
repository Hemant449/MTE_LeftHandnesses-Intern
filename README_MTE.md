# Left-Handedness, Birth Cohorts & Age at Death

### MedTourEasy — Data Analytics Project

This notebook investigates whether the apparent difference in age at death between left- and right-handed groups can emerge from historical changes in reported left-handedness across birth cohorts.

The analysis combines a historical handedness survey, a U.S. mortality distribution, exploratory data analysis (EDA), and Bayes' theorem to estimate age-at-death distributions conditional on handedness.

> **Important interpretation:** The 2018 scenario is a modeled comparison, not an analysis of 2018 mortality records. The notebook keeps the same mortality distribution and changes how age at death is mapped to birth year. Therefore, the outputs describe a modeled apparent age-at-death gap—not a causal estimate of life expectancy or proof that handedness affects mortality.

## Project objectives

- Inspect and prepare historical handedness and mortality data using Pandas and NumPy.
- Explore handedness rates by age, sex, and inferred birth cohort.
- Examine the distribution of deaths by age and convert death counts into age probabilities.
- Estimate conditional probabilities using Bayes' theorem.
- Compare modeled 1990 and 2018 study-year scenarios.
- Evaluate how the modeled age-at-death gap changes across study years.

## Data sources

The notebook loads the source files directly from the URLs below, so an internet connection is required when running the data-loading section.

| Dataset | Description | Source |
|---|---|---|
| Historical handedness rates | Left-handedness rates by age and sex, derived from the 1986 National Geographic survey | [`lh_data.csv`](https://gist.githubusercontent.com/mbonsma/8da0990b71ba9a09f7de395574e54df1/raw/aec88b30af87fad8d45da7e774223f91dad09e88/lh_data.csv) |
| U.S. mortality distribution | Death distribution by age for the United States, used by the original analysis | [`cdc_vs00199_table310.tsv`](https://gist.githubusercontent.com/mbonsma/2f4076aab6820ca1807f4e29f75f18ec/raw/62f3ec07514c7e31f5979beeca86f19991540796/cdc_vs00199_table310.tsv) |

The notebook uses the `Age`, `Male`, and `Female` fields from the handedness data, and `Age` and `Both Sexes` from the mortality data.

## Methodology

### 1. Data preparation and EDA

- Inspect columns, data types, missing values, duplicate rows, and descriptive statistics.
- Convert survey age to an approximate birth year using:

  \[
  \text{Birth year} = 1986 - \text{Age in survey}
  \]

- Calculate the mean of the reported male and female handedness rates for each age/cohort.
- Plot handedness rates by age and sex, the mean rate by age and birth year, and the difference between male and female rates.
- Explore the mortality distribution by age, its normalized probability distribution, and broad age bands.

### 2. Probability model

Let `LH` denote left-handedness, `RH` denote right-handedness, and `A` denote age at death.

The notebook estimates the probability of being left-handed conditional on age at death, \(P(LH\mid A)\), using the historical birth-cohort handedness curve. It then calculates the overall left-handedness probability as a mortality-weighted average:

\[
P(LH)=\sum_A P(LH\mid A)P(A)
\]

Bayes' theorem is then used to estimate the age-at-death distribution for left-handed and right-handed groups:

\[
P(A\mid LH)=\frac{P(LH\mid A)P(A)}{P(LH)}
\]

\[
P(A\mid RH)=\frac{P(RH\mid A)P(A)}{P(RH)}
\]

The expected age at death is calculated as the probability-weighted average age:

\[
E[A\mid LH]=\sum_A A\,P(A\mid LH)
\]

\[
E[A\mid RH]=\sum_A A\,P(A\mid RH)
\]

### 3. Study-year comparison

For each modeled study year, the notebook uses:

\[
\text{Birth year}=\text{Study year}-A
\]

The 1990 and 2018 scenarios therefore associate the same age at death with different birth cohorts. The mortality distribution remains fixed; only the handedness-to-birth-cohort mapping changes. Where a required cohort is outside the survey's observed range, the notebook uses a boundary estimate based on the mean of the nearest 10 observations, consistent with the approach in the original implementation.

The notebook also calculates a year-by-year sensitivity analysis for modeled study years from 1990 through 2018.

## Main results

The notebook's expected outputs are approximately:

| Modeled scenario | Mortality distribution | Apparent age-at-death gap |
|---|---|---:|
| 1990 | Fixed U.S. mortality distribution used by the analysis | 5.5 years |
| 2018 | Same fixed mortality distribution | 2.3 years |

These values may vary slightly with data or implementation details. They are **model outputs**, not direct measurements of life expectancy in 1990 or 2018. The key observation is that the apparent gap changes when the handedness rates assigned to birth cohorts change, even though the mortality distribution is held constant.

## Notebook contents

The notebook is organized into the following sections:

1. Imports and plotting setup
2. Data sources and initial inspection
3. Data preparation
4. EDA — handedness data
5. EDA — mortality distribution
6. Bayesian framework
7. Estimating \(P(LH\mid A)\)
8. Overall probability \(P(LH)\)
9. Conditional age-at-death distributions
10. Expected age at death
11. Modeled 2018 scenario
12. Study-year sensitivity analysis
13. Final results
14. Interpretation and limitations
15. Reproducibility checklist
16. References

## Requirements

- Python 3.9 or later (recommended)
- Jupyter Notebook or JupyterLab
- Internet access to retrieve the source datasets

Python packages:

- `numpy`
- `pandas`
- `matplotlib`
- `ipython`

Install the packages with:

```bash
pip install numpy pandas matplotlib ipython jupyter
```

## How to run

1. Download `MedTourEasy_Left_Handedness_Analysis_Rebuilt_MathFixed.ipynb` and this `README.md` into the same project folder.
2. Install the dependencies listed above.
3. Open a terminal in that folder and start Jupyter:

   ```bash
   jupyter notebook
   ```

4. Open the notebook and run the cells from top to bottom.
5. Make sure you have an active internet connection when the notebook loads the two datasets from the source URLs.

## Tools used

- **Python** — analysis and modeling
- **Pandas** — data loading, cleaning, transformation, and summaries
- **NumPy** — vectorized calculations and probability computations
- **Matplotlib** — exploratory and model-result visualizations
- **Jupyter Notebook** — documented, reproducible analysis

## Limitations and interpretation

- The handedness curve is derived from a single historical survey conducted in 1986.
- The mortality distribution is held fixed for the modeled study-year comparison; the notebook does not use a separate 2018 mortality dataset.
- Handedness rates outside the observed birth-cohort range are extrapolated using boundary averages.
- The notebook estimates conditional age-at-death distributions. These are not the same as a directly observed life-expectancy estimate.
- A changing modeled gap demonstrates sensitivity to cohort structure; it does not, by itself, establish causation or prove that handedness has no biological relationship with mortality.

## References

- **Original project implementation:** [Do Left-Handed People Really Die Young?](https://github.com/shukkkur/Do-Left-Handed-People-Die-Young)
- **Handedness data:** [`lh_data.csv`](https://gist.githubusercontent.com/mbonsma/8da0990b71ba9a09f7de395574e54df1/raw/aec88b30af87fad8d45da7e774223f91dad09e88/lh_data.csv)
- **Mortality data:** [`cdc_vs00199_table310.tsv`](https://gist.githubusercontent.com/mbonsma/2f4076aab6820ca1807f4e29f75f18ec/raw/62f3ec07514c7e31f5979beeca86f19991540796/cdc_vs00199_table310.tsv)
- **Related research:** [Modeling to explore and challenge inherent assumptions in the handedness and longevity controversy](https://pmc.ncbi.nlm.nih.gov/articles/PMC10369838/)

---

*This notebook is intended for educational and interview-preparation purposes. When describing the project, refer to the result as a modeled apparent age-at-death gap and be explicit that the 2018 scenario does not use 2018 mortality data.*
