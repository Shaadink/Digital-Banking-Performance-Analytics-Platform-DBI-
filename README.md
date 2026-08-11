# 🏦 Digital Banking Performance & Analytics Platform (DBI)

![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-00758F?style=for-the-badge&logo=powerbi&logoColor=white)
![Excel](https://img.shields.io/badge/Excel-217346?style=for-the-badge&logo=microsoftexcel&logoColor=white)

An enterprise-grade, multi-page **Power BI Analytics Platform** built for a **Digital Banking & Innovation (DBI)** department. This project tracks **26,270 financial transactions** (27.09M SAR total volume), **70,781 user sessions**, **4 strategic feature rollouts**, and **3.96M SAR in departmental operating budget** across H1 2026 (Jan – Jun 2026).

---

## 🎯 Project Objectives & Problem Statement

### **Problem Statement**
Traditional retail banking operations face increasing friction due to costly physical branch overhead, unknown digital user drop-offs, and lack of feature adoption visibility following digital product rollouts. Executive leadership requires a unified, data-driven performance dashboard to monitor cross-channel migration, evaluate initiative adoption uplifts, isolate session funnel bottlenecks, and ensure strict departmental fiscal control.

### **Core Objectives**
1. **Channel Migration Analytics**: Track financial transaction volume trends across Mobile App, Internet Banking, ATM, and Physical Branches to measure digital shift.
2. **Initiative Adoption Evaluation**: Measure pre vs. post-launch adoption rates across new digital features (AI Chatbot, Biometric Auth, Quick Pay, Mobile Onboarding).
3. **Session Behavioral Analytics**: Model a 5-stage user session funnel to pinpoint conversion drop-offs between login and transaction completion.
4. **Fiscal Governance & Budget Variance**: Monitor departmental spend across Marketing, Technology/Licensing, and Operations against budgeted thresholds.

---

## 📂 Dataset Overview & Model Architecture

The data model reflects an enterprise banking analytics stack, utilizing a **multi-fact schema** to handle transactional and behavioral web/app analytics at different granularities.

| Table | Granularity / Source | Records | Primary Metrics / Key Columns |
|---|---|---|---|
| **`Fact_Transactions`** | Transaction-level financial log | 26,270 rows | `TransactionID`, `Date`, `Channel`, `Amount_SAR`, `Status`, `CustomerID` |
| **`Fact_Sessions`** | Web/App session log (GA/Adobe engine) | 70,781 rows | `SessionID`, `Date`, `Channel`, `DeviceType`, `FunnelStageReached`, `SessionDuration_sec` |
| **`Initiatives`** | Feature launch performance tracker | 4 rows | `InitiativeName`, `LaunchDate`, `PreLaunchAdoptionPct`, `PostLaunchAdoptionPct` |
| **`Budget`** | Departmental monthly spend ledger | 24 rows | `Month`, `CostCategory`, `Budgeted_SAR`, `ActualSpend_SAR` |

### **Data Model Relationship Design**
* **Standalone Fact Tables**: `Fact_Transactions` and `Fact_Sessions` operate as independent fact tables (avoiding many-to-many relationship conflicts) with independent visual date hierarchies.
* **Filter Contexts**: Page-level and visual-level slicers dynamically segment metrics across channels, cost categories, and date ranges.

---

## 📊 Dashboard Architecture & Screenshots

### **Landing Page — Interactive Navigation Cover**
* Executive landing screen featuring a Deloitte-inspired digital banking theme, quick platform scope highlights, and three interactive **Page Navigation Buttons** linking directly to core reporting layers.

![Landing Page Cover](https://raw.githubusercontent.com/Shaadink/Digital-Banking-Performance-Analytics-Platform-DBI-/main/Screenshot%202026-08-12%20025608.png)

---

### **Page 1 — 6 Month DBI Performance Report (Executive Overview)**
* **KPI Strip**:
  * **Total Transactions**: `26,270`
  * **Total Transaction Value**: `27.09M SAR` (27,088,465.61 SAR)[cite: 1]
  * **Transaction Success Rate**: `94.0%` (Target: 95.0%)[cite: 1]
  * **June MoM Growth Rate**: `+11.1%`[cite: 1]
* **Visuals**:
  * *Multi-Line Chart*: 6-month transaction volume trend by channel (Mobile App, Internet Banking, ATM, Branch)[cite: 1].
  * *Donut Chart*: Total channel volume share distribution[cite: 1].
* **Key Executive Takeaway**: Mobile App transactions grew from 755 in Jan to 2,298 in June, officially surpassing physical branches in April 2026 to become the primary banking channel (33.4% overall share)[cite: 1].

![Page 1 Executive Overview](https://raw.githubusercontent.com/Shaadink/Digital-Banking-Performance-Analytics-Platform-DBI-/main/Screenshot%202026-08-12%20025627.png)

---

### **Page 2 — Initiative Performance & User Behavior**
* **KPI Strip & Controls**:
  * **Average Initiative Adoption Uplift**: `+11.3%`[cite: 1]
  * **Funnel Completion Rate**: `21.4%` (15,128 completed / 70,781 sessions)[cite: 1]
  * **Interactive Filter**: Global Channel Slicer (`Mobile App` / `Internet Banking`)[cite: 1].
* **Visuals**:
  * *Clustered Bar Chart*: Pre vs. Post Launch Adoption % across 4 product initiatives[cite: 1].
  * *Column/Funnel Chart*: 5-Stage Session Funnel (`Login` $\rightarrow$ `OTP Verification` $\rightarrow$ `Dashboard View` $\rightarrow$ `Transaction Initiated` $\rightarrow$ `Transaction Completed`)[cite: 1].
* **Key Behavioral Takeaway**: AI Chatbot Launch delivered the highest individual adoption uplift (+12.6%)[cite: 1]. Primary user session friction occurs between **Dashboard View (20.07K)** and **Transaction Initiated (16.56K)**—a **17.5% intention drop-off**[cite: 1].

![Page 2 Initiatives and User Behavior](https://raw.githubusercontent.com/Shaadink/Digital-Banking-Performance-Analytics-Platform-DBI-/main/Screenshot%202026-08-12%20025639.png)

---

### **Page 3 — Channel Trend & Budget Snapshot**
* **KPI Strip**:
  * **Total Actual Spend (YTD)**: `3,964,690 SAR`[cite: 1]
  * **Total Budgeted Spend (YTD)**: `3,959,233 SAR`[cite: 1]
  * **Budget Variance**: `+0.1%` (+5,457 SAR overrun / On Track)[cite: 1]
* **Visuals**:
  * *Stacked Area / Clustered Column Chart*: Monthly actual vs. budgeted spend progression[cite: 1].
  * *Financial Breakdown Matrix*: Cost category ledger detailing spend across Marketing, Technology/Licensing, and Operations[cite: 1].
* **Key Fiscal Takeaway**: Operational cost savings (-10.3% under budget) successfully neutralized software and cloud infrastructure overruns in Technology/Licensing (+10.6% over budget) caused by rapid mobile traffic scaling[cite: 1].

![Page 3 Channel Trend and Budget Snapshot](https://raw.githubusercontent.com/Shaadink/Digital-Banking-Performance-Analytics-Platform-DBI-/main/Screenshot%202026-08-12%20025651.png)

---

## 🧮 Core DAX Measures Reference

```dax
// 1. Total Transaction Count
Total Transactions = COUNTROWS(Fact_Transactions)

// 2. Total Transaction Value (SAR)
Total Transaction Value = SUM(Fact_Transactions[Amount_SAR])

// 3. Transaction Success Rate %
Success Rate % = 
DIVIDE(
    CALCULATE(COUNTROWS(Fact_Transactions), Fact_Transactions[Status] = "Success"),
    COUNTROWS(Fact_Transactions),
    0
)

// 4. Month-over-Month Transaction Growth %
MoM Transaction Growth % = 
VAR LatestDate = MAX(Fact_Transactions[Date])
VAR CurrentMonth = CALCULATE([Total Transactions], DATESINPERIOD(Fact_Transactions[Date], LatestDate, -1, MONTH))
VAR PrevMonth = CALCULATE([Total Transactions], DATEADD(DATESINPERIOD(Fact_Transactions[Date], LatestDate, -1, MONTH), -1, MONTH))
RETURN DIVIDE(CurrentMonth - PrevMonth, PrevMonth)

// 5. Session Funnel Completion Rate %
Completion Rate % = 
DIVIDE(
    CALCULATE(COUNTROWS(Fact_Sessions), Fact_Sessions[FunnelStageReached] = "Transaction Completed"),
    COUNTROWS(Fact_Sessions),
    0
)

// 6. Average Initiative Adoption Uplift %
Avg Adoption Uplift = 
AVERAGEX(Initiatives, Initiatives[PostLaunchAdoptionPct] - Initiatives[PreLaunchAdoptionPct])

// 7. Budget Variance %
Budget Variance % = 
DIVIDE(SUM(Budget[ActualSpend_SAR]) - SUM(Budget[Budgeted_SAR]), SUM(Budget[Budgeted_SAR]), 0)





## 💡 Key Business Insights & Strategic Recommendations

* **Accelerate Physical Branch Optimization**:
  * **Insight**: Branch transaction volume declined steadily from 856 (Jan) to 562 (Jun), while Mobile App volume expanded by 204%[cite: 1].
  * **Recommendation**: Reallocate physical branch Teller headcount toward digital onboarding assistance and high-value advisory services[cite: 1].

* **Address Dashboard-to-Initiation Conversion Friction**:
  * **Insight**: 17.5% of active user sessions terminate on the main dashboard without initiating a transaction[cite: 1].
  * **Recommendation**: Implement 1-click quick transfer prompts and biometric shortcut triggers on the main mobile landing dashboard to convert passive viewers into active transactions[cite: 1].

* **Permanent Operating Budget Reallocation**:
  * **Insight**: Technology/Licensing costs exceeded budget by +10.6% due to high server throughput during mobile surge months, while Operations stayed -10.3% under budget[cite: 1].
  * **Recommendation**: Permanently reallocate H2 operational cost savings into software infrastructure and cloud licensing budgets to support expanding mobile traffic[cite: 1].

* **SAMA Regulatory Compliance Integration**:
  * **Insight**: Regulatory reporting (SAMA) requires granular audit trails for transaction failure points[cite: 1].
  * **Recommendation**: Expand `Fact_Transactions` in production to link directly to automated compliance dimension tables logging security consent and regulatory data retention windows[cite: 1].





