# Attrition Risk Analysis Dashboard

A Power BI project that goes beyond historical attrition reporting to **predict which active employees are at risk of leaving, diagnose why, and translate that into retention actions business leaders can act on.**

Built as a workforce analytics case study covering the full pipeline: requirements → data modeling → predictive risk scoring → diagnostic driver analysis → prescriptive recommendations.

---

## 📊 Overview

| | |
|---|---|
| **Tool** | Power BI Desktop |
| **Records** | 1,500 employees |
| **Pages** | 5 |
| **Attrition rate** | 16.4% |
| **Active employees flagged High risk** | 33 (of 1,254 active) |

The dashboard is built around one core idea: don't just report who already left — score who is *likely to leave next*, so the business can intervene before it happens.

---

## 🗂️ Dashboard Pages

### 1. Executive Workforce Overview
High-level snapshot: total headcount, active employees, new hires, total attrition, and 12-month hiring/attrition trends, broken down by business unit, country, and job level. The page a leader opens first to gauge overall workforce health.

### 2. Attrition
Descriptive view of exits: voluntary vs. involuntary split, attrition by business unit/location/gender, top exit reasons, and tenure/year slicers. Answers **"what happened."**

### 3. Attrition Risk Analysis
Predictive view. Every *active* employee is scored on attrition risk (High / Medium / Low) using a composite of satisfaction and tenure signals. Risk is broken down by business unit, job level, tenure, manager, location, and gender — so any manager can immediately see if risk is concentrated in their team. Answers **"who is likely to leave next."**

### 4. Attrition Risk Drivers
Diagnostic view. Surfaces the measurable factors most correlated with risk score — job satisfaction, manager satisfaction, work-life balance, and time since last promotion — with counts of active employees currently exposed to each driver. Answers **"why."**

### 5. Recommendations & Retention Strategies
Prescriptive view. Translates the analysis into plain-English, business-ready actions: what's driving attrition, who's most exposed, and what a manager or HRBP should do about it this quarter.

---

## 🔑 Key Insights

- **Promotion stagnation is the #1 driver of risk** (correlation +0.42 with risk score) — stronger than manager relationship or job satisfaction. **59% of active employees** haven't been promoted in 4+ years.
- **Risk rises with seniority and tenure, not the reverse.** Directors/Partners and Senior Managers carry the highest average risk scores; employees with 11+ years tenure are the most exposed group — the opposite of where most retention budgets typically go.
- **Work-life balance is the second-strongest driver** (-0.36 correlation) and the #3 stated reason employees actually resigned.
- **Risk concentrates in specific business units** — Consulting and Tax & Legal show meaningfully higher risk than Audit & Assurance or Enabling Areas.
- **A handful of managers** have teams with disproportionately high average risk scores, pointing to a leadership/team-culture pattern rather than isolated individual cases.
- **Historical exit reasons validate the model** — the top reasons people actually left (better career opportunity, better salary, no work-life balance, lack of progression) line up closely with the statistical risk drivers, confirming the risk score reflects real behavior, not just theory.

---

## 🛠️ Data Model

Single fact table (`Dataset`, 1,500 rows, 42 columns) including:

- **Demographics & org structure:** Gender, Age, Business Unit, Department, Job Level, Region/Country/State/City, Reporting Manager
- **Tenure & lifecycle:** Employee Start/End Date, Years of Service, Tenure Bucket, Last Promotion Date, Years Since Promotion, Internal Transfers
- **Attrition fields:** Exit Type (Voluntary/Involuntary), Exit Reason, Is Leaver, Is Active
- **Engagement signals:** Job Satisfaction, Manager Satisfaction, Work-Life Balance, Career Growth Satisfaction (1–5 scale)
- **Derived risk fields:** Risk Score, Risk Band (High/Medium/Low)

Plus supporting date tables for time intelligence.

### Key DAX Measures

```dax
Attrition Rate = DIVIDE([Total Leavers], [Total Headcount], 0)

High Risk % = DIVIDE([High Risk Employees], [Active Headcount], 0)

Average Risk Score =
CALCULATE(
    AVERAGE('Dataset'[Risk Score]),
    'Dataset'[Is Active] = TRUE()
)

Employees with No Promotion in 4+ Years =
CALCULATE(
    DISTINCTCOUNT('Dataset'[Employee ID]),
    'Dataset'[Is Active] = TRUE(),
    'Dataset'[Years Since Promotion] >= 4
)
```

---

## 📁 Repository Contents

```
├── Attrition_Risk_Analysis_Dashboard.pbix   # Full Power BI project
├── README.md                                # This file
└── /screenshots                             # (optional) page exports for quick preview
```

---

## 🚀 How to Use

1. Clone/download this repository.
2. Open `Attrition_Risk_Analysis_Dashboard.pbix` in **Power BI Desktop** (2023+ recommended).
3. Navigate the pages left to right — the report is designed to be read in narrative order: **Overview → Attrition → Risk Analysis → Risk Drivers → Recommendations.**
4. Slicers on the Attrition page (Tenure, Year) can be used to filter the whole report.

---

## 💡 Methodology Notes

- **Risk Score** is a rules-based composite built from satisfaction scores and tenure/promotion signals — not a trained ML model. This was a deliberate choice for transparency and explainability to business stakeholders, who need to trust and act on the score.
- Recommendations were derived by correlating risk score components against actual historical exit reasons, to ensure the drivers being surfaced are grounded in real attrition behavior rather than assumption.

---

## 📌 Recommended Next Steps (Roadmap)

- [ ] Layer in compensation benchmarking data to test pay as an attrition driver
- [ ] Add manager-level drill-through page for 1:1 stay-interview prep
- [ ] Automate a monthly refresh + high-risk watchlist export for HRBPs
- [ ] Explore a logistic regression / ML-based risk model as a v2 enhancement

---

## 👤 Author

Built as a workforce analytics portfolio project demonstrating the full analytics lifecycle — from requirements gathering through predictive risk scoring to business-ready recommendations.
