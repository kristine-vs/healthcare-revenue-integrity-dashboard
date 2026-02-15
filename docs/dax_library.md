# DAX Measure Library

This library documents the DAX formulas used to drive the insights in the Healthcare Revenue Integrity Dashboard.

---

## 1. Core Base Measures

Foundational aggregations used as building blocks for advanced calculations.

### Total Claims
Counts the total volume of claims processed.
```
Total Claims = COUNT(Fact_Claims[Claim_ID])
```
### Total Billed
The sum of all gross charges submitted to payers.
```
Total Billed = SUM(Fact_Claims[Billed_Amount])
```
### Total Insurance Paid
The actual cash collected from payers.
```
Total Insurance Paid = SUM(Fact_Claims[Paid_Amount])
```
### Total Patient Paid
The amount collected directly from patients.
```
Total Patient Paid = SUM('Fact_Claims'[Patient_Paid])
```
### Total Patient Liability
Total patient responsibility (deductible + copay + coinsurance).
```
Total Patient Liability = SUM('Fact_Claims'[Patient_Total_Resp])
```
### Total Patients
Distinct count of unique patients serviced.
```
Total Patients = DISTINCTCOUNT(Fact_Claims[Patient_ID])
```
### Total Contractual Write-Offs
The difference between billed charges and allowed amounts.
```
Total Contractual Write-Offs = SUM(Fact_Claims[Contractual_Adj])
```
### Total Bad Debt
Uncollected patient balance.
```
Total Bad Debt = SUM('Fact_Claims'[Patient_Balance_Due])
```
---

## 2. Revenue Cycle Efficiency

Metrics tracking velocity and financial realization.

### Clean Claim Rate
Percentage of claims processed without denial.
```
Clean Claim Rate = 1 - [Denial Rate]
```
### Insurance Collection Rate
Percentage of billed charges collected from insurance.
```
Insurance Collection Rate = DIVIDE([Total Insurance Paid], [Total Billed], 0)
```
### Net Collection Rate
True collection score vs. Allowed Amount.
```
Net Collection Rate = 
DIVIDE(
    [Total Insurance Paid] + [Total Patient Paid],
    SUM('Fact_Claims'[Allowed_Amount]),
    0
)
```
### Avg Processing Time
Average days to process a claim.
```
Avg Processing Time = AVERAGE(Fact_Claims[Days_to_Process])
```
### Avg Paid per Claim
Average revenue realized per claim.
```
Avg Paid per Claim = DIVIDE(
    [Total Insurance Paid],
    [Total Claims],
    0
)
```
---

## 3. Denial Management & Risk

KPIs for identifying revenue leakage and aging risk.

### Denial Count
Total number of denied claims.
```
Denial Count = CALCULATE(COUNT(Fact_Claims[Claim_ID]), Fact_Claims[Status] = "Denied")
```
### Denial Rate
Percentage of total claims that are denied.
```
Denial Rate = DIVIDE([Denial Count], COUNT(Fact_Claims[Claim_ID]), 0)
```
### % High Risk Denials
Denials aging >30 days.
```
% High Risk Denials = 
DIVIDE(
    CALCULATE(COUNTROWS(Fact_Claims), Fact_Claims[Status] = "Denied" && Fact_Claims[Days_to_Process] > 30),
    [Denial Count], 
    0
)
```
### Total Denied Charges
Total billed amount tied to denied claims.
```
Total Denied Charges = 
CALCULATE(
    SUM(Fact_Claims[Billed_Amount]),
    Fact_Claims[Status] = "Denied"
)
```
### Denial Impact %
Percentage of total billed revenue tied up in denials.
```
Denial Impact % = 
DIVIDE(
    [Total Denied Charges], 
    [Total Billed], 
    0
)
```
### Top Denial Reason
Most frequent reason for denial in the current context.
```
Top Denial Reason = 
CALCULATE(
    SELECTEDVALUE(Fact_Claims[Denial_Reason]),
    TOPN(1, VALUES(Fact_Claims[Denial_Reason]), [Denial Count], DESC)
)
```
---

## 4. Patient Analytics

Metrics focused on utilization and patient financial behavior.

### Claims per Patient
Average number of claims per unique patient.
```
Claims per Patient = 
DIVIDE(
    COUNT(Fact_Claims[Claim_ID]),
    DISTINCTCOUNT(Fact_Claims[Patient_ID]),
    0
)
```
### Avg Bill per Patient
Average billed amount per patient.
```
Avg Bill per Patient = AVERAGE(Fact_Claims[Billed_Amount])
```
### Avg Patient Liability
Average financial responsibility per patient.
```
Avg Patient Liability = AVERAGE('Fact_Claims'[Patient_Total_Resp])
```
### Patient Collection Rate
Percentage of patient liability successfully collected.
```
Patient Collection Rate = 
DIVIDE(
    [Total Patient Paid],
    [Total Patient Liability],
    0
)
```
---

## 5. Calculated Columns (Data Enrichment)

Row-level segmentation logic.

### Age Group
Segments patients into life-stage categories.
```
Age Group = 
SWITCH(
    TRUE(),
    Dim_Patients[Age] <= 18, "Pediatric (0-18)",
    Dim_Patients[Age] <= 35, "Young Adult (19-35)",
    Dim_Patients[Age] <= 50, "Adult (36-50)",
    Dim_Patients[Age] <= 64, "Pre-Senior (51-64)",
    "Senior (65+)"
)
```
### Member Tenure
Calculates years of membership based on reference year 2026.
```
Member_Tenure = 2026 - 'Dim_Patients'[Member_Since]
```
### Tenure Group
Segments patients based on loyalty/longevity.
```
Tenure Group = 
SWITCH(
    TRUE(),
    'Dim_Patients'[Member_Tenure] <= 3, "New (0-3 yrs)",
    'Dim_Patients'[Member_Tenure] <= 6, "Mid-Term (4-6 yrs)",
    'Dim_Patients'[Member_Tenure] <= 10, "Long-Term (7-10 yrs)",
    'Dim_Patients'[Member_Tenure] <= 14, "Veteran (11-14 yrs)",
    "Legacy (15+ yrs)"
)
```
---

## Author

Kristine Soliman  
Data & Operations Analyst | Chandler, AZ
