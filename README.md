# SCI-110-Academic-Impact-Analysis

## 📌 Project Overview
This project provides an end-to-end data pipeline and interactive business intelligence dashboard to measure the academic impact of an intervention course (**SCI 110**). By analyzing student transcript data, this tool calculates the shift in a student's average GPA before taking the course versus after, providing clear visibility into which academic programs benefit the most.

## 🛠️ Tech Stack
* **Data Processing:** Python (Pandas, NumPy, OpenPyXL)
* **Visualization:** Power BI Desktop
* **Calculations:** DAX (Data Analysis Expressions)

## 🗂️ Repository Contents
* `SCI_Performance_Processing.py`: The Python script that extracts, cleans, and transforms the raw Excel student records into structured analytical tables.
* `SCI_Performance_Dashboard.pbix`: The Power BI dashboard file containing the data model, measures, and interactive visuals.
* `README.md`: Project documentation.

## ⚙️ Data Pipeline Logic
The Python script handles complex temporal transcript data to ensure accurate GPA tracking. Key processing steps include:
1. **Grade Standardization:** Converts letter grades (A, B-, C+, etc.) to a standard 4.0 numerical GPA scale. Withdrawals ('W') are safely ignored as `NaN` to prevent skewing averages.
2. **Chronological Sorting:** Converts alpha-numeric academic terms (e.g., 'FA15', 'SP16') into a sortable numeric format with weighted seasonal values (Spring = 0.1, Summer = 0.2, Fall = 0.3).
3. **Intervention Tagging:** Identifies the exact term a student took `SCI 110` (the "Target Term") and categorizes all other courses dynamically as `Pre` or `Post` intervention.
4. **Performance Delta:** Aggregates the courses into an average GPA for both the Pre and Post periods and calculates the exact `GPA_Difference` (Δ GPA).
5. **Multi-Sheet Export:** Generates an Excel workbook with two structured sheets: 
   - `GPA_Analysis`: The core mathematical differences.
   - `Student_Records`: The granular course-by-course details for dashboard slicing.

## 📊 Power BI Dashboard Features
The dashboard is designed for institutional leadership, using concise terminology and clear visual indicators to highlight academic trends.

**Key Dashboard Metrics (Short Names):**
* **N (Cohort Size):** Total unique students analyzed.
* **Pre-GPA & Post-GPA:** Average GPA before and after the intervention.
* **Δ GPA (Net Shift):** The overall average performance change.
* **Gains / Drops:** Total count of students whose performance improved versus declined.

**Core Visuals:**
* **Diverging Bar Chart:** Highlights the average GPA shift separated by academic Major to identify which departments see the most benefit.
* **Detailed Matrix:** A conditional-formatted table showing exact student-level shifts.

## 💻 Setup & Installation

### 1. Running the Python Script
1. Ensure Python 3.x is installed along with the required libraries:
   ```bash
   pip install pandas numpy openpyxl
Update the file_path and output_path variables in SCI_Performance_Processing.py to match your local directory structure.

Run the script. It will generate SCI_Performance_Analysis.xlsx.

2. Updating Power BI
Open the .pbix file.

Go to Home > Transform Data to open Power Query.

In the "Applied Steps" on the right for both the GPA_Analysis and Student_Records queries, click the gear icon next to "Source" and update the file path to point to your newly generated Excel file.

Click Close & Apply.

📝 Key DAX Measures
For reference, here are the core DAX formulas powering the dashboard counts:

Code snippet
Count Improved = 
CALCULATE(
    COUNTROWS('GPA_Analysis'),
    FILTER('GPA_Analysis', 'GPA_Analysis'[GPA_Difference] > 0)
)

Average GPA Pre = AVERAGE('GPA_Analysis'[Pre])
