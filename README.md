# Healthcare Revenue Integrity & Risk Intelligence Dashboard
Power BI dashboard analyzing healthcare revenue cycle risk, denials, collections, and patient financial responsibility.

### Data Disclaimer
**IMPORTANT:** All data utilized in this project is **synthetic/dummy data**. It has been programmatically generated for demonstration purposes and does not represent real patient records, actual healthcare facilities, or proprietary insurance contract details. This project demonstrates RCM analytical capabilities while maintaining strict HIPAA-compliance standards.

## Assumptions & Limitations
* **Single-Line Claims:** This model assumes one service line per claim ID for simplified granularity.
* **Payer Landscape:** Primary insurance logic only; does not currently model secondary coordination of benefits (COB).
* **Recoupments:** Focuses on initial denial/payment; does not model complex takebacks or retroactive adjustments.
  
---

## 1. Project Overview
**What problem are we solving?**
Healthcare organizations lose millions annually to "Revenue Leakage", money earned clinically but never collected due to administrative denials, poor payer realization, or patient bad debt.

This project analyzes a **$77.8M billing portfolio** to simulate a full Revenue Cycle Management (RCM) funnel. It moves beyond simple reporting to provide **Revenue Integrity Intelligence**, pinpointing exactly where friction occurs between the service date and the bank deposit.

## 2. Business Questions Answered
* **Leakage Detection:** Where is the revenue eroding - contractual write-offs, payer denials, or patient non-payment?
* **Clinical Risk:** Which diagnosis categories (e.g., Cardiac vs. MSK) generate the highest concentration of bad debt?
* **Insurance Scorecard:** Which insurance payers have strong operational efficiency but weak financial realization?
* **Patient Behavior:** How does patient tenure (<3 years vs. 10+ years) correlate with collection reliability?

## 3. KPI Summary (Snapshot)
| Metric | Value | Context |
| :--- | :--- | :--- |
| **Total Billed Volume** | **$77.83M** | Gross charges generated |
| **Net Collection Rate** | **86.04%** | Cash collected vs. Allowed Amount (True Yield) |
| **Clean Claim Rate** | **82.16%** | Claims paid on first pass (Efficiency) |
| **Denial Rate** | **17.84%** | Claims requiring rework/appeal |
| **Total Bad Debt** | **$6.07M** | Uncollectible patient liability |

## 4. Dashboard Structure

### Executive Overview (Strategic)
Designed for the CFO/VP of Revenue Cycle.
* **Features:** Frequency Toggles (Year/Month/Week), Operational Velocity tracking.
* **Goal:** Monitor the high-level pulse of the revenue cycle and identify macro-trends in processing time vs. collection yield.
  
![Executive Overview](screenshots/Executive%20Overview.png)
*(Click image to view full resolution)*

### Insurance Performance (Tactical)
Designed for Payer Relations & Billing Managers.
* **Features:** Denial composition analysis, heat-mapped Payer Realization Matrix.
* **Goal:** Identify low-performing contracts and high-friction denial reasons (e.g., "Authorization Missing") to guide front-end process improvements.

![Insurance Performance](screenshots/Insurance%20Performance.png)
*(Click image to view full resolution)*

### Patient Finance (Operational)
Designed for Collections & Financial Counselors.
* **Features:** Revenue Realization Funnel, Clinical Category Risk Profiling.
* **Goal:** Drill through from high-risk clinical categories (e.g., Cardiac) to specific patient account lists for targeted intervention.

![Patient Finance](screenshots/Patient%20Finance.png)
*(Click image to view full resolution)*

![Patient Details Drill-Through](screenshots/patient_detail_drill_through.png)

## Key Insights & Findings
* **The "UnitedHealth" Outlier:** Analysis identifies UnitedHealth as a primary driver of operational friction, showing significantly higher processing times compared to other payers, impacting cash liquidity.
* **Clinical Bad Debt Drivers:** **Cardiac** and **Metabolic** diagnosis categories account for the highest concentration of bad debt, driven by high deductible plans and "sticker shock."
* **Denial Root Causes:** "Coverage Lapsed" and "Patient Ineligible" are the dominant denial reasons, indicating a breakdown in front-office eligibility verification rather than clinical coding errors.
* **Operational Efficiency:** The analysis establishes **Clean Claim Rate** as a critical *leading indicator*, where higher first-pass acceptance reduces the administrative burden of appeals and accelerates cash velocity.

## 6. Data Model & Technical Stack
* **Architecture:** Star Schema (Fact_Claims linked to Dim_Patient and Dim_Diagnosis).
* **Tools:** Power BI, DAX, Python (Data Generation), GitHub.
* **Advanced Logic:** Uses a "Waterfall" calculation methodology to track Billed $\to$ Allowed $\to$ Paid $\to$ Balance.

*(For full technical details, measures, and logic, see the `/docs` folder).*

---

## 📂 Folder Structure
```text
healthcare-revenue-integrity-dashboard/
│
├── data/              # Raw synthetic datasets
├── dashboards/        # Power BI (.pbix) files
├── docs/              # Technical documentation (DAX, Dictionary, Logic)
├── screenshots/       # Dashboard images for review
├── README.md          # Project overview                
└── LICENSE            # MIT License
```

---
## Author

**Kristine Soliman**  
Data & Operations Analyst | Chandler, AZ  
