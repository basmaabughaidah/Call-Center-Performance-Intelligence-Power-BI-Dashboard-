# Call Center Performance Intelligence (Power BI Dashboard)

Public documentation for a Power BI report that analyzes call center performance through **service, efficiency, and demand KPIs**.
This dashboard is designed to help operations and management **detect bottlenecks**, **meet SLAs**, and **improve customer experience** with data-driven decisions.

> This repository intentionally includes **documentation and screenshots only**.  
> The PBIX file and raw datasets are **not published** to protect privacy, customer information, and internal operational data.

---

## Why this project matters
Call centers are highly sensitive to delays, staffing imbalance, and peak-hour demand. This dashboard provides:
- **SLA visibility** (Service Level %) to ensure responsiveness targets are met
- **Efficiency tracking** (AHT) to control workload and agent performance
- **Customer impact monitoring** (Answered vs. Abandoned) to reduce frustration and churn risk
- **Root-cause analysis readiness** through drill-through and dimensional breakdowns (segment, time, region, call type)

---

## Business goals & targets
- **AHT (Average Handle Time):** target **≤ 230 seconds**
- **Service Level (SL%):** target **≥ 80%**
- Reduce **Abandoned Calls** and improve **Answered Calls**
- Understand **call volume patterns** to support staffing and scheduling decisions

---

## Core KPIs (What we measure)

### 1) Average Handle Time (AHT)
Measures the average time agents spend handling calls.
- Target: **230 seconds max**

### 2) Service Level Percentage (SL%)
Percent of calls answered within the defined threshold.
- Target: **80% minimum**

### 3) Answered Calls
Total calls successfully answered by agents.

### 4) Abandoned Calls
Calls dropped by customers before being answered.

### 5) Call Volumes
Total inbound demand:
- `Call Volumes = Answered + Abandoned`

---

## Analysis dimensions (How we slice the data)
- **Call Type:** inbound vs outbound
- **Customer Segment:** e.g., VIP / new customers / standard
- **Date & Time:** day/week/month trends + hour-of-day performance
- **Geography (optional):** region/area if available

---

## Data model (Recommended structure)

### Fact table (Call-level data)
Typical columns:
- `CallID`
- `CallType` (Inbound/Outbound)
- `HandleTime` (seconds)
- `CallOutcome` (Answered/Abandoned)
- `AnsweredWithinThreshold` (Yes/No)
- `CallDateTime`
- `CustomerSegment`
- `Region` (optional)

### Dimension tables
- **Date table** (mandatory for time intelligence)
- **Customer Segment**
- **Call Type**
- **Region** (optional)

---

## Report layout (Suggested pages & visuals)

### 1) Executive KPIs (Top section)
- **AHT card** with conditional formatting (red if > 230)
- **SL% gauge / KPI card** (red if < 80%)
- Cards: **Answered**, **Abandoned**, **Call Volumes**

### 2) Time trends
- **Line chart** for AHT, SL%, and Call Volumes across days/weeks/months
- Optional: hour-of-day view to detect peak and understaffed windows

### 3) Impact analysis (drivers)
- **Stacked bar** or **matrix** to compare performance by:
  - Call type
  - Customer segment
  - Time buckets (hour / shift)

### 4) Answered vs Abandoned distribution
- Pie/Donut chart for answered vs abandoned share
- Optional: trend of abandonment rate over time

### 5) Filters / slicers
- Date range
- Call type
- Customer segment
- Region (if available)


## Screenshots

![Screenshot 01](Screenshot%2001.png)
![Screenshot 02](Screenshot%2002.png)
![Screenshot 03](Screenshot%2003.png)
![Screenshot 04](Screenshot%2004.png)
![Screenshot 05](Screenshot%2005.png)
![Screenshot 06](Screenshot%2006.png)



---

## DAX measures (Core)

```DAX
AHT =
DIVIDE(
    SUM(CallData[HandleTime]),
    COUNT(CallData[CallID])
)

SL_Percentage =
DIVIDE(
    COUNTROWS(FILTER(CallData, CallData[AnsweredWithinThreshold] = "Yes")),
    COUNTROWS(CallData)
) * 100

CallVolumes =
COUNT(CallData[CallID])

AnsweredCalls =
COUNTROWS(FILTER(CallData, CallData[CallOutcome] = "Answered"))

AbandonedCalls =
COUNTROWS(FILTER(CallData, CallData[CallOutcome] = "Abandoned"))

