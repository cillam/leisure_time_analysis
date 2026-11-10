# Describing the Relationship Between Leisure Time and Age

Does the time people spend on leisure change with age? This project uses linear regression on the 2022 American Time Use Survey to describe how daily leisure time varies across ages, alongside gender, children, and family income.

Graduate project for DATASCI 203 (Statistics for Data Science), UC Berkeley Master of Information and Data Science.

**Team:** Alice Robinson, Carolina Gonzalez, Fan Lin, Priscilla Miller

## Files

- `DescribingTheRelationshipBetweenLeisureTimeAndAge.pdf` — final report
- `final_report_draft.Rmd` — R Markdown source for the analysis and report

## Data

The data come from the 2022 [American Time Use Survey (ATUS)](https://timeuse.ipums.org/), accessed through IPUMS. The ATUS is a nationally representative U.S. time diary survey; respondents are drawn from households that completed the Current Population Survey. The final analysis sample has 7,597 respondents.

**Outcome:** leisure time, measured as minutes per day spent socializing, on hobbies, relaxing, playing passive games, watching TV, listening to music, and reading or writing for personal interest. Exercise, personal care, religious and volunteer activities, chores, and childcare are excluded.

**Main predictor:** age in years (respondents 15 and older).

**Covariates considered:** gender, children in the household, family income, marital status, region, and school enrollment.

## Data preparation

- Dropped respondents aged 80 and over, because the source codes them in grouped ranges (80–84, 85+) rather than single years.
- Converted family income from category codes to the midpoint of each bracket, in thousands of dollars.
- Recoded missing-value codes for children and region, then removed rows with missing values.

## Method

A scatterplot showed leisure time falling from the late teens to the mid-30s and rising after about 40, so the team modeled age as a second-degree polynomial (age and age²).

The team fit a sequence of linear regressions, starting from an intercept-only model and adding age terms, then gender, children, income, marital status, and region, comparing adjusted R² across specifications. The final model is:

```
act_social ~ age + age² + female + children + family_income
```

Covariates left out of the final model:

- **Marital status** was not significant once children were included.
- **Region** had small effects relative to the other candidates.
- **School enrollment** had small subsamples for high school categories, and college enrollment had no significant coefficients. These models appear in the appendix.

## Findings

- Leisure time reaches its minimum around **age 38** and rises on either side of that point.
- Being female, having children, and higher family income each predict less leisure time. All coefficients in the final model are significant at p < 0.01.
- Respondents at the 10th income percentile report about **77 more minutes** of leisure per day than those at the 90th.
- The report separates statistical from practical significance. Measured against the standard deviation of leisure time (217 minutes), no single feature is practically significant. Using a 30-minute threshold for a difference a person would notice, age, gender, children, and income all are.

## Limitations

- The model explains a small share of the variance in leisure time, likely because people define leisure differently and the predictor set is limited.
- Results depend on the team's definition of leisure.
- Several candidate covariates were excluded after exploratory analysis, and some enrollment categories had small samples.
- The independence assumption relies on the ATUS sampling design, which selects one respondent per household.

## Tools

R and R Markdown, with tidyverse, dplyr, ipumsr, sandwich, lmtest, stargazer, scales, and knitr.
