# Analyzing Students' Mental Health

![Illustration of silhouetted heads](mentalhealth.jpg)

## Overview

Does going to university in another country affect your mental health? A Japanese international university surveyed its student body in 2018 and published a study the following year, approved by several ethics committees and regulatory bodies.

The study found that international students face a **higher risk of mental health difficulties** than the general population, and that **social connectedness** (belonging to a social group) and **acculturative stress** (the stress associated with integrating into a new culture) are predictors of depression.

This project explores the `students` dataset using **PostgreSQL** to verify whether similar conclusions can be drawn for international students, and whether the **length of stay** is a contributing factor.

## Dataset

The dataset contains survey responses from **285 students**, stored in `students.csv` (50 columns), covering demographics, language proficiency, and validated mental health and acculturation test scores.

### Key Fields

| Field Name      | Description                                            |
| --------------- | ------------------------------------------------------ |
| `inter_dom`     | Type of student (international or domestic)            |
| `japanese_cate` | Japanese language proficiency                          |
| `english_cate`  | English language proficiency                           |
| `academic`      | Current academic level (undergraduate or graduate)     |
| `age`           | Current age of the student                             |
| `stay`          | Current length of stay in years                        |
| `todep`         | Total depression score (PHQ-9 test)                    |
| `tosc`          | Total social connectedness score (SCS test)            |
| `toas`          | Total acculturative stress score (ASISS test)          |

## Analysis

The main analysis focuses on international students, grouping them by length of stay and comparing their average test scores:

```sql
SELECT stay, COUNT(*) AS count_int,
       ROUND(AVG(todep), 2) AS average_phq,
       ROUND(AVG(tosc), 2) AS average_scs,
       ROUND(AVG(toas), 2) AS average_as
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay, inter_dom
ORDER BY stay DESC
LIMIT 9;
```

### Results

| stay | count_int | average_phq | average_scs | average_as |
| ---- | --------- | ----------- | ----------- | ---------- |
| 10   | 1         | 13.00       | 32.00       | 50.00      |
| 8    | 1         | 10.00       | 44.00       | 65.00      |
| 7    | 1         | 4.00        | 48.00       | 45.00      |
| 6    | 3         | 6.00        | 38.00       | 58.67      |
| 5    | 1         | 0.00        | 34.00       | 91.00      |
| 4    | 14        | 8.57        | 33.93       | 87.71      |
| 3    | 46        | 9.09        | 37.13       | 78.00      |
| 2    | 39        | 8.28        | 37.08       | 77.67      |
| 1    | 95        | 7.48        | 38.11       | 72.80      |

### Key Findings

- The majority of international students are in their **first year** (95 of 201), with sample sizes dropping sharply for longer stays.
- Average depression scores (PHQ-9) remain in a similar range (roughly 7.5–9) across the first four years, with a slight peak around year 3.
- Acculturative stress (ASISS) tends to be **higher for students who have stayed 3–5 years** compared to those in their first year, suggesting that stress does not necessarily decrease with time.
- Social connectedness (SCS) scores stay relatively stable across lengths of stay.
- Due to the small number of students with stays longer than 4 years, conclusions for those groups should be treated with caution.

## Repository Contents

| File             | Description                                        |
| ---------------- | -------------------------------------------------- |
| `notebook.ipynb` | Jupyter notebook with the SQL analysis             |
| `students.csv`   | The student survey dataset                         |
| `mentalhealth.jpg` | Illustration used in the notebook                |
| `README.md`      | Project documentation                             |

## Tools & Skills

- **SQL (PostgreSQL)** — filtering, grouping, aggregation (`COUNT`, `AVG`, `ROUND`)
- **Jupyter Notebook** — exploratory data analysis
- Data analysis of mental health and survey data
