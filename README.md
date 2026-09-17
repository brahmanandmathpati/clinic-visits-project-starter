# Mini-Project 2 — MediCare Outpatient Visits: End-to-End Data Analysis
# 🏥 Clinic Visits Analysis

**[One sentence: what this project does. Example: Analyzes clinic visit data to find busy days, common reasons for visits, and patient trends.]**

![Python](https://img.shields.io/badge/Python-3.10-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

🔗 **Live Demo:** (coming soon)


You are the data analyst for MediCare's outpatient clinic. The administrator has one year of visit records and needs answers she can act on in 2026. Your job: clean the data honestly, answer five guided questions with charts, investigate one question of your own, and present a recommendation.

## Files

| File | What it is |
|---|---|
| `clinic_visits_2025.csv` | Raw export — ~2,400 visits, deliberately messy |
| `clinic_visits_analysis_student.ipynb` | Your notebook. Fill every `# YOUR CODE HERE` and every *Reading:* line |
| `README.md` | This file — becomes the README of your GitHub repo |

## Rules

- **No `sklearn`, no models.** Insight, not prediction. Module 16 is next.
- Every cleaning decision goes in the **Data-Quality Log** with rows affected and *why this fix rather than another*.
- Every question ends in a **chart with a title, axis labels, and a one-sentence reading**.
- Notebook must run top-to-bottom in a fresh kernel (Runtime → Restart and run all) before submission.
- Use AI assistants for syntax, not for decisions. In the viva you will be asked to explain any line.

## Definition of done

- [ ] Data-Quality Log has 7 rows, each with rows affected and a reason
- [ ] Final clean shape printed and matches the acceptance number given in class
- [ ] Q1–Q5 each have a chart and a *Reading:* sentence
- [ ] Own question: one `groupby`, one chart, one defended statistic, one "what I'd need to be sure"
- [ ] Recommendation paragraph, 120–180 words, addressed to the administrator
- [ ] Five slides (template below) as PDF or PPTX
- [ ] README: cleaning-log summary + answers to the five interview questions
- [ ] Notebook runs clean from a fresh kernel

## The five-slide template

| Slide | Title | Content |
|---|---|---|
| 1 | The question | One sentence: what the administrator asked. One line: the data (rows, period, columns). |
| 2 | Data & cleaning | The 7-row log as a compact table. Bold the one decision you're proudest of. |
| 3 | Three findings | Three charts, three one-line readings. One must be from your own question. |
| 4 | Recommendation | The paragraph, cut to 3 bullets + the one number that proves it. |
| 5 | What I'd do next | What you'd check with more data · one thing you'd change · "Next: predict consult time from age + department + doctor". |

**Two-minute pitch order:** slide 1 (15 s) → slide 3 (60 s) → slide 4 (30 s) → slide 5 (15 s). Skip slide 2 unless asked.

## The five interview questions (answer these in your README)

-> 5 -Interview question :

**1. Why keep the ₹50,000 fees when the outlier rule flagged them?**

> Because they showed the signature of **real data, not errors**. All 13 were **exactly ₹50,000** — errors are random, fixed prices are not — and all appeared only in **Orthopedics and Cardiology**, the two surgical departments. That pattern says "procedure price list," not "typo." Deleting them would have erased **₹6.5 lakh** of genuine revenue. I flagged them with `is_procedure` instead, so I could report consultation and procedure revenue separately without losing either. The IQR fence tells you a value is *unusual*; it cannot tell you it is *wrong* — that judgement needs domain knowledge.
> 

**2. How did you catch the 83 Sunday visits?**

> I applied a **real-world check**: the clinic is closed on Sundays, so any Sunday visit must be a parsing error. The cause was `format="mixed", dayfirst=True` — correct for `30/06/2025`, but on ISO dates like `2025-05-12` it **swapped month and day**, turning 12 May into 5 December. I fixed it by parsing each of the three formats **explicitly** with `errors="coerce"` and chaining `.fillna()`, then re-verified: **zero unparsed dates, zero Sundays**. The lesson: the code ran without error and still produced wrong dates — only domain knowledge caught it.
> 

**3. Why per-department median for missing fees?**

Because department fees differ enormously — **₹520 in General Medicine versus ₹1,260 in Cardiology**. The overall median of ₹730 would have **undercharged every Cardiology visit by ₹530** and **overcharged General Medicine by ₹210**, distorting both ends. I used the **median rather than the mean** because the ₹50,000 procedures inflate Cardiology's mean to ₹2,021 — filling a routine consultation with that figure would have been badly wrong. Per-department median respects the real structure of the pricing.


**4. Cardiology mean ₹2,021 vs median ₹1,260 — which goes in the annual report?**

**Both, clearly labelled.** The **median (₹1,260)** is the honest answer to "what does a Cardiology consultation cost" — it's what a typical patient actually pays, and it matches the procedure-excluded mean of ₹1,258 almost exactly, which confirms it. The **mean (₹2,021)** is not wrong, but it answers a different question: total revenue divided by visits. Reporting it as "average consultation fee" would mislead, since six surgeries lifted it by ₹763 for 383 patients who never had one. My recommendation: report **median consultation fee** and **total procedure revenue** as two separate lines.


**5. Which decision would you change with more data?**

> The **negative wait times**. I repaired 5 rows with `abs()`, assuming a sign-entry typo — reasonable, because the magnitudes (up to 37 minutes) were plausible. But it is still an assumption. With **appointment booking timestamps** I could compute the true wait as `seen_time − arrival_time` and verify each one instead of guessing. If they turned out to be something else — a system clock error, or a different field entirely — `abs()` would have quietly preserved five wrong numbers in my wait-time medians.

## Grading (20 marks)

| Criterion | Marks |
|---|---|
| Data-Quality Log — problems found, fixes justified | 5 |
| Q1–Q5 correct, charted, read | 5 |
| Own question — depth and defended statistic | 3 |
| Recommendation paragraph — specific, numeric, actionable | 3 |
| Reproducibility & hygiene — fresh-kernel run, README, slides | 2 |
| Viva & pitch | 2 |

## Setup

Works in Google Colab (upload the CSV) or locally:

```bash
pip install numpy pandas matplotlib jupyter
jupyter notebook clinic_visits_analysis_student.ipynb
```
