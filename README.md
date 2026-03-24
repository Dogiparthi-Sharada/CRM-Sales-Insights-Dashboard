# CRM Sales Insights & Process Optimization Dashboard

## Project Overview
This project analyzes a CRM sales pipeline to identify workflow 
inefficiencies and data governance gaps. Using Python, SQL, and 
Power BI, I built an end-to-end analytics solution that quantifies 
the impact of process improvements on sales team performance.

## Problem Statement
Sales representatives were experiencing delayed follow-up times 
due to poor data quality at the CRM entry point. Approximately 
40% of incoming leads had missing or inaccurate contact 
information, forcing reps to manually search for data before 
they could begin outreach — creating significant bottlenecks.

## Tools & Technologies
- **Python (pandas, numpy)** — Synthetic dataset generation 
  mimicking enterprise CRM environments
- **SQL (MySQL)** — Data extraction, cleaning, and 
  before/after performance benchmarking
- **Power BI & DAX** — Interactive dashboard with 
  Before/After optimization slicer
- **Lucidchart** — As-Is vs To-Be process flow documentation
- **Excel** — Initial data structuring and validation

## Dashboard Preview
![CRM Dashboard](CRM_Sales_Optimization_Dashboard_Preview.png)

## Methodology
1. **Requirements Gathering** — Identified where the workflow 
   was stalling by analyzing lead follow-up timestamps
2. **Data Generation** — Built a realistic 5,000-record dataset 
   with engineered Before/After optimization periods using Python
3. **SQL Analysis** — Quantified error rates and follow-up times 
   across both periods to prove improvement metrics
4. **Process Mapping** — Documented As-Is vs To-Be workflows 
   in Lucidchart to communicate bottlenecks to stakeholders
5. **Dashboard Build** — Developed interactive Power BI dashboard 
   with Phase slicer enabling Before/After comparison

## Key Results

| Metric | Before Optimization | After Optimization | Improvement |
|---|---|---|---|
| Avg. Follow-Up Days | 4.5 days | 3.8 days | **15% faster** |
| Data Error Rate | 40% | 10% | **30% improvement** |
| Total Leads Analyzed | 5,000 | 5,000 | — |

## SQL Queries Used

### Proving 15% Follow-Up Efficiency Gain
```sql
SELECT 
    CASE 
        WHEN CAST(Date_Assigned AS DATETIME) < 
          DATE_SUB(CURDATE(), INTERVAL 180 DAY) 
        THEN 'Before Optimization'
        ELSE 'After Optimization'
    END AS Phase,
    ROUND(AVG(Days_To_Follow_Up), 2) AS Avg_Days_To_Follow_Up
FROM CRM_Sales_Pipeline_Data
GROUP BY Phase
ORDER BY Phase;
```

### Proving 30% Data Accuracy Improvement
```sql
SELECT 
    CASE 
        WHEN CAST(Date_Assigned AS DATETIME) < 
          DATE_SUB(CURDATE(), INTERVAL 180 DAY) 
        THEN 'Before Optimization'
        ELSE 'After Optimization'
    END AS Phase,
    ROUND((SUM(CASE WHEN Data_Quality_Flag = 'Missing/Inaccurate' 
      THEN 1 ELSE 0 END) / COUNT(Lead_ID)) * 100, 2) AS Error_Rate_Pct
FROM CRM_Sales_Pipeline_Data
GROUP BY Phase
ORDER BY Phase;
```

### Identifying Bottlenecks by Lead Source
```sql
SELECT 
    Lead_Source,
    COUNT(Lead_ID) AS Total_Leads,
    ROUND(AVG(Days_To_Follow_Up), 2) AS Avg_Follow_Up_Days
FROM CRM_Sales_Pipeline_Data
WHERE Status IN ('New', 'In Progress')
GROUP BY Lead_Source
ORDER BY Avg_Follow_Up_Days DESC;
```

## DAX Measures (Power BI)
```
Error Rate % = 
DIVIDE(
    CALCULATE(
        COUNTROWS('CRM_Sales_Pipeline_Data'),
        'CRM_Sales_Pipeline_Data'[Data_Quality_Flag] = "Missing/Inaccurate"
    ),
    COUNTROWS('CRM_Sales_Pipeline_Data')
) * 100
```

## Process Flow (As-Is vs To-Be)

### Before Optimization
```
New Lead → CRM System → Rep Reviews Lead
→ Data Missing? (40% YES) → Manual Search → Delayed Follow-Up (4.5 days)
```

### After Optimization
```
New Lead → System Validates Data → Clean Lead Routed to Rep
→ Rep Sends Email Immediately → Follow-Up Complete (3.8 days)
```

## Repository Structure
```
crm-sales-insights-dashboard/
│
├── CRM_Sales_Pipeline_Data.csv   # Generated dataset (5,000 records)
├── generate_crm_data.py          # Python data generation script
├── dashboard_screenshot.png      # Power BI dashboard screenshot
└── README.md                     # Project documentation
```

## Skills Demonstrated
- Data governance and quality management
- Workflow analysis and process optimization
- Stakeholder communication through data visualization
- Change management measurement (before/after analysis)
- End-to-end analytics pipeline (Python → SQL → Power BI)
