# 📊 Student Performance Factor Analysis — Power BI Dashboard

> An interactive 5-page Power BI dashboard analysing the key factors influencing student academic performance across academic habits, socioeconomic background, lifestyle factors, and at-risk student identification.

---

## 📌 Project Overview

This project was developed as an end-semester academic submission for **Chandigarh University**. It explores a dataset of **6,607 student records** to uncover what drives — and what hinders — academic performance, and presents the findings through a fully interactive Power BI report.

The dashboard is designed for **non-technical stakeholders** such as teachers, school administrators, and academic counsellors, enabling them to explore multi-dimensional performance insights without requiring any programming knowledge.

---

## 📂 Dataset

| Property | Details |
|---|---|
| **Source** | [Kaggle — Student Performance Factors] |
| **Records** | 6,607 students |
| **Original Columns** | 20 attributes |
| **Added Columns** | 4 (Pass_Fail, Score_Grade, Study_Category, Attendance_Category) |
| **Target Variable** | `Exam_Score` (0–100) |

### Original Columns

| Category | Columns |
|---|---|
| Academic | `Hours_Studied`, `Attendance`, `Previous_Scores`, `Tutoring_Sessions` |
| Behavioral | `Motivation_Level`, `Peer_Influence`, `Extracurricular_Activities` |
| Socioeconomic | `Family_Income`, `Parental_Education_Level`, `Parental_Involvement`, `Access_to_Resources`, `Internet_Access`, `School_Type` |
| Lifestyle | `Sleep_Hours`, `Physical_Activity`, `Learning_Disabilities` |
| Other | `Gender`, `Distance_from_Home`, `Teacher_Quality` |

### Added Calculated Columns (via Python & DAX)

| Column | Logic |
|---|---|
| `Pass_Fail` | `"Pass"` if Exam_Score ≥ 60, else `"Fail"` |
| `Score_Grade` | A (≥80), B (≥70), C (≥60), D (≥50), F (<50) |
| `Study_Category` | Low (0–10h), Medium (11–20h), High (21–30h), Very High (30h+) |
| `Attendance_Category` | Poor (<60%), Average (60–75%), Good (75–90%), Excellent (90%+) |

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **Microsoft Power BI Desktop** | Dashboard development & visualisation |
| **Python (pandas, openpyxl)** | Data cleaning & preprocessing |
| **DAX (Data Analysis Expressions)** | Measures & calculated columns |
| **Kaggle** | Dataset source |

---

## 📐 DAX Measures

All measures are defined in the Power BI data model. Key measures used across the dashboard:

```dax
-- Average exam score across all students
Avg Exam Score = AVERAGE('StudentPerformanceFactors'[Exam_Score])

-- Total student count (responds to slicer filters)
Total Students = COUNTROWS('StudentPerformanceFactors')

-- Count of students who passed (score >= 60)
Pass Count = COUNTROWS(
    FILTER('StudentPerformanceFactors',
    'StudentPerformanceFactors'[Exam_Score] >= 60)
)

-- Pass rate as a percentage
Pass Rate % = DIVIDE([Pass Count], [Total Students]) * 100

-- Count of at-risk students (score < 60)
At Risk Count = COUNTROWS(
    FILTER('StudentPerformanceFactors',
    'StudentPerformanceFactors'[Exam_Score] < 60)
)

-- Average score among at-risk students only
Avg Score At Risk = CALCULATE(
    AVERAGE('StudentPerformanceFactors'[Exam_Score]),
    'StudentPerformanceFactors'[Exam_Score] < 60
)

-- Average attendance among at-risk students only
Avg Attendance At Risk = CALCULATE(
    AVERAGE('StudentPerformanceFactors'[Attendance]),
    'StudentPerformanceFactors'[Exam_Score] < 60
)
```

---

## 📊 Dashboard Pages

### Page 1 — Overview Dashboard
High-level summary of the entire dataset with KPI cards, Pass/Fail donut chart, grade distribution bar chart, and exam score distribution column chart. Includes slicers for Gender, School Type, and Motivation Level.

### Page 2 — Academic Habits
Scatter plots showing correlations between study hours/attendance and exam scores (with trend lines). Bar charts comparing average scores by study category, tutoring sessions, and sleep hours.

### Page 3 — Student Background
Analysis of socioeconomic factors: family income, parental education, parental involvement, school type (by gender), access to resources, and internet access impact on average scores.

### Page 4 — Lifestyle Factors
Motivation level and peer influence bar charts, extracurricular activities comparison, physical activity line chart, learning disabilities impact, and a **Motivation × Peer Influence heatmap matrix** as the highlight visual.

### Page 5 — At-Risk Students
Dedicated page for students scoring below 60. Includes at-risk KPI cards, a sortable student detail table (with drill-through from Page 1), and breakdown charts by motivation level, peer influence, and parental involvement.

---

## 🔍 Key Findings

| Finding | Detail |
|---|---|
| 📚 **Study hours matter most** | High-study students (20+ hrs/week) score 5–7 points higher on average |
| 🎯 **Motivation is the #1 lifestyle factor** | High motivation students outperform low motivation by ~5.4 points |
| 👪 **Parental involvement beats wealth** | High involvement adds 4.4 points vs 1.5 points from high income |
| ⚠️ **At-risk students share a clear profile** | 75% low motivation · 68% negative peers · 70% low parental involvement |
| 🏃 **Optimal physical activity = 3 days/week** | Non-linear relationship — too little or too much both hurt performance |
| 🔗 **Social factors interact multiplicatively** | High motivation + positive peers = 71.8 avg vs low motivation + negative peers = 62.3 avg |

---

## 🚀 How to Run

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/Student-Performance-PowerBI.git
   cd Student-Performance-PowerBI
   ```

2. **Run the preprocessing script** *(optional — cleaned file already included)*
   ```bash
   pip install pandas openpyxl
   python scripts/data_prep.py
   ```

3. **Open Power BI**
   - Open `dashboard/StudentPerformance.pbix` in Power BI Desktop
   - If prompted, update the data source path to point to your local `dataset/` folder
   - Click **Refresh** to reload data

4. **Explore the dashboard**
   - Use the page tabs at the bottom to navigate between the 5 pages
   - Use slicers to filter by Gender, School Type, Motivation Level, etc.
   - Right-click the **Fail** segment in the donut chart on Page 1 → **Drill through → At-Risk Students**

---

## 📋 Prerequisites

- [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
- Python 3.8+ with `pandas` and `openpyxl` *(only needed to re-run preprocessing)*

---

## 📁 Project Report

The full academic project report is available in `report/Student_Performance_Project_Report.docx` and follows the Chandigarh University end-semester project format, including:

- Chapter 1: Introduction & Problem Identification
- Chapter 2: Literature Review & Problem Definition
- Chapter 3: Design Flow & Methodology
- Chapter 4: Results Analysis & Validation
- Chapter 5: Conclusion & Future Work
- Appendix: All DAX measures with explanations

---

## 🔮 Future Enhancements

- [ ] Predictive ML model integration using Python visuals in Power BI
- [ ] Real-time database connectivity via DirectQuery
- [ ] Personalised intervention recommendation engine
- [ ] Mobile-optimised dashboard layout
- [ ] Multi-institution comparative analysis with row-level security
- [ ] Natural language Q&A interface using Power BI Q&A feature

---

## 🏫 Institution

**Chandigarh University**
Department of Computer Science & Engineering
BE in CSE
April 2026

---

## 📄 License

This project is submitted for academic purposes at Chandigarh University. The dataset is sourced from Kaggle and is subject to its original license terms.

---

## 🙏 Acknowledgements

- Dataset: [Student Performance Factors — Kaggle](https://www.kaggle.com/)
- Tool: [Microsoft Power BI Desktop](https://powerbi.microsoft.com/)
- Institution: Chandigarh University
