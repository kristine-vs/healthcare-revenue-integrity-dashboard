
# RCM Financial Logic & Waterfall

This document explains the business logic used to simulate the Revenue Cycle Management (RCM) waterfall within the dashboard.

## The Revenue Equation
The dashboard follows standard healthcare accounting principles to reconcile revenue from "Gross" to "Net."

### 1. Gross Charges (Total Billed)
* **Definition:** The "Sticker Price" for services rendered.
* **Context:** This number is artificially high and rarely collected in full due to insurance contracts.

### 2. Contractual Adjustments
* **Definition:** The difference between *Billed Amount* and *Allowed Amount*.
* **Logic:** `Billed_Amount - Allowed_Amount = Contractual Adjustment`
* **Insight:** This is "Paper Loss" agreed upon in payer contracts, not actual operational leakage.

### 3. Net Revenue (Allowed Amount)
* **Definition:** The maximum collectable amount based on the contract.
* **Logic:** This is the denominator for the **Net Collection Rate**.
* **Formula:** `Allowed_Amount = Insurance Responsibility + Patient Responsibility`

### 4. Cash Collections
* **Insurance Paid:** Payment received from the insurance.
* **Patient Paid:** Payment received from the patient.

### 5. Revenue Leakage (The Gap)
Leakage occurs when *Allowed Amount* > *Total Paid* (Total Insurance Paid/Total Patient Paid). <br>
Note: Contractual adjustments are excluded from leakage calculations, as they represent negotiated discounts rather than operational losses. <br>
This dashboard categorizes leakage into two buckets:
1.  **Denial Write-offs:** Revenue lost because the insurance company refused to pay.
2.  **Bad Debt:** Revenue lost because the patient failed to pay their portion.<br>
   
Note: Denied charges may overlap with patient responsibility and bad debt, as some denied claims are shifted to patients rather than fully written off. In these cases, a denied claim may partially convert into patient liability and subsequent bad debt rather than representing a complete revenue loss.


## Visualizing the Waterfall
The "Revenue Realization Funnel" on Tab 3 validates this logic:
1.  Start: **$77.8M** (Billed)
2.  Minus: **$34M** (Contractual Adj)
3.  Equals: **Allowed Amount**
4.  Result: **Collected** OR **Bad Debt/Denied**

---

## Author

Kristine Soliman  
Data & Operations Analyst | Chandler, AZ
