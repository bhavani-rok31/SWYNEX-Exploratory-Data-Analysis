# Task 2 – Exploratory Data Analysis (Employee Dataset)

EDA on the cleaned employee dataset from Task 1 (60 employees, 11 columns: Age, Gender, Department, City, Join_Date, Salary, Rating, Active, etc.).
Tools: Python (pandas, seaborn, matplotlib, scipy).

## Project structure
```
data/cleaned_data.csv     cleaned dataset from Task 1
eda.py                    full analysis script (stats, tests, charts)
charts/                   10 PNG charts
summary_stats.csv         descriptive statistics
requirements.txt
```
Run: `pip install -r requirements.txt && python eda.py`

## Key statistics
| Metric | Age | Salary | Rating |
|---|---|---|---|
| Non-null count | 35 | 44 | 44 |
| Mean | 37.6 | 74,470 | 3.49 |
| Median | 36 | 77,939 | 3.5 |
| Std dev | 11.4 | 28,121 | 1.41 |
| Min – Max | 22 – 57 | 26,504 – 114,889 | 1 – 5 |
| Missing | 41.7% | 26.7% | 26.7% |

IQR outlier check found **no outliers** in Age, Salary or Rating. All three are roughly symmetric (|skew| < 0.5).

## Insights

**1. Data completeness is the biggest limitation.**
Only 13 of 60 rows are fully complete. Age is 42% missing and Salary and Rating are 27% each. Every finding below uses the available rows only and is directional, not conclusive. (`01_missing_values.png`)

**2. Men earn ~22% more on average than women, with no rating difference.**
Mean salary is 80.8k for men (n=22) and 66.1k for women (n=21). Average ratings are almost identical (3.46 vs 3.45). The gap is not statistically significant at 5% (Welch t-test p = 0.084), but it is worth an HR pay-equity review. (`03_salary_by_gender.png`)

**3. Pay is only weakly linked to performance.**
Salary and Rating have a Spearman correlation of 0.25 (p = 0.18, n = 31). Examples: 3 employees earn above 90k with a rating of 2 or below, and 3 earn under 45k with a rating of 4.5 or above. Pay bands do not appear to reward performance. (`07_salary_vs_rating.png`, `09_correlation.png`)

**4. Newer cohorts are paid more but rated lower (pay compression).**
Average salary rises with each join year: 65k (2019), 69k (2020), 77k (2021), 85k (2022). Average rating falls from 4.1 for the 2019 cohort to about 3.3 for the later three. Recent hires appear to be brought in at higher pay than longer-serving staff. (`06_join_cohort_trend.png`)

**5. Department and city differ noticeably.**
- HR has the highest average salary (83.7k) and Sales the lowest (66.4k).
- IT has the top rating (4.8, only 5 people) and Finance the lowest (2.9).
- Delhi (91k) and Mumbai (86k) pay the most, while Ahmedabad pays the least (46k, n=3).
- Bengaluru has the lowest city rating (2.7) despite an above-average salary of 76k.

Sales (4.1 rating, lowest pay) and Surat (4.5 rating, below-average pay of 66k) are rated highly despite low pay, so pay does not track rating across groups either. Small group sizes (n = 3–14) mean these are indicative only. (`04_department_salary_rating.png`, `05_city_salary.png`)

**6. More than half of employees are marked inactive, and it varies by department.**
30 of 55 employees with a known status (55%) are inactive. Rates are highest in HR (75%) and Sales (71%) and lowest in Finance (33%). Inactive employees are younger on average (34.7 vs 40.5 years) and have slightly higher ratings (3.6 vs 3.3, not significant, p = 0.46). This suggests attrition may be losing younger, well-rated staff. (`08_inactive_by_department.png`)

## Anomalies and data-quality flags
- **Join_Date looks batch-loaded:** only 4 distinct dates exist, and 24 of 60 employees (40%) share 2021-05-14, which may be a default or import date. (`10_join_date_batches.png`)
- **Age has a spike at 25:** 6 employees are aged 25 (the mode), which may be imputed or defaulted values.
- **Gender and name mismatches:** e.g. "Aditya Verma" is Female and "Neha Gupta" is Male, so the Gender field should be verified with the source before gender conclusions are relied on.
- **Abbreviated names** (e.g. "N. Kumar", "S. Rao") remain, although the email addresses reveal the full name (nikhil.kumar98@…). There are also 2 employees named "Sneha Joshi" with different IDs, so they are not duplicates.
- **Wide salary range:** the minimum is 26.5k and the maximum 114.9k, with no true statistical outliers.

## Recommendations
1. Fix the source-data gaps (Age, Salary, Rating, Join_Date) before deeper modelling.
2. Audit pay by gender and by cohort.
3. Investigate why HR and Sales have high inactivity.
4. Introduce performance-linked pay bands.
