
***

# 📊 Contract Prolongation Analysis (Test Assignment)

## Project Overview

This project is a solution to a complex data analysis test assignment for an Analyst position. The goal was to evaluate the performance of Account Managers in the Customer Support Department regarding **contract prolongations (renewals)** for the year 2023.

The core challenge of this task was not just calculating metrics, but **disentangling the complex relationships between two datasets** that could not be simply merged. The data structure reflected real-world messiness: projects could end and restart multiple times a year, financial records were fragmented, and status tracking required deep custom logic.

## 🎯 Business Objectives

The stakeholder (Head of Department) needed actionable insights on:
*   **Retention Efficiency:** How well managers renew contracts in the **1st month** (K1) and **2nd month** (K2) after expiration.
*   **Manager Performance:** A weighted rating of managers based on their renewal success rates.

### Key Metrics (KPIs)
*   **K1 (Retention Month 1):** Revenue from projects prolonged immediately vs. revenue of all expiring projects.
*   **K2 (Retention Month 2):** Revenue from delayed prolongations vs. revenue of projects lost in the first month.

*All Yearly KPIs are calculated as **Weighted Averages** to ensure statistical accuracy.*

## 🧩 Data Engineering Challenges & Logic

The primary complexity lay in the **non-standard relationships** between the `prolongations` (events) and `financial_data` (cash flow) tables:

1.  **One-to-Many Project Lifecycles:**
    *   A single `Project ID` could appear multiple times in the `prolongations` table.
    *   *Scenario:* A project might end in March, be renewed, end again in July, and be renewed again. The code had to correctly map financial data to specific expiration events, not just the project ID globally.

2.  **Unmarked Terminations in Finance:**
    *   The `financial_data` table contained monthly revenue streams but **lacked explicit end dates**.
    *   *Challenge:* We had to infer the "base" month for calculation solely from the `prolongations` table and dynamically link it to the corresponding financial column, handling cases where a project had revenue in some months but not others.

3.  **Fragmented Financial Records:**
    *   Financial data for a single project was often split across multiple rows (e.g., "Part 1 payment", "Part 2 payment").
    *   *Solution:* A robust aggregation step was implemented to sum these partial payments by month **before** applying any business logic (like "Zero" value handling).

4.  **"Zero" Value Logic:**
    *   The data contained flags like `'в ноль'` (Zero). Business logic dictated using the *previous month's* revenue in these cases, but only if the *aggregated* total for the month was truly zero. This required strict ordering of operations (Aggregation $\rightarrow$ Zero Logic).

## 🚀 Features

*   **Cloud Data Loading:** Fetches data directly from Google Drive via `requests`.
*   **Advanced Data Cleaning:** Normalizes currency strings, handles text flags ('stop', 'end', 'zero'), and sanitizes headers.
*   **Complex Aggregation:** Solves the "partial payments" issue by grouping and summing financial rows while preserving metadata.
*   **Dynamic KPI Calculation:** Calculates K1 and K2 for every month using a rolling window approach, respecting the many-to-one project lifecycle.
*   **Visualization:**
    *   Monthly Coefficient Dynamics (Line Chart).
    *   Manager Performance Ratings (Horizontal Bar Charts with smart label placement).

## 💻 Technology Stack

*   **Python 3.x**
*   **Pandas** (Complex data manipulation, grouping, rolling logic)
*   **NumPy**
*   **Matplotlib & Seaborn** (Data Visualization)
*   **Requests** (Data fetching)

## 📂 File Structure

*   `Contract_Prolongation_Analysis.ipynb`: The main Jupyter Notebook containing the end-to-end analysis pipeline.
*   `requirements.txt`: List of dependencies.

## ⚙️ How to Run

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-username/prolongation-analysis.git
    ```
2.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
3.  **Run the Notebook:**
    Open `Contract_Prolongation_Analysis.ipynb` in Jupyter Notebook or Google Colab. The script will automatically download the dataset and generate the visualizations.

---

**Author:** Elena Bezrukova
*Data Analyst*
