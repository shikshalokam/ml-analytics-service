# Debugging Guide for Admin and Program Dashboard Issues 

This document serves as a comprehensive **debugging guide** for troubleshooting issues related to the **Admin and Program Dashboards**. These dashboards rely on **batch data ingestion pipelines**, and any inconsistencies in data availability can impact their performance.  

### **Purpose of this Guide**  
The guide provides a **structured approach** to identify and resolve issues by:  
1. **Verifying Data Pipelines** – Ensuring batch ingestion processes are running as expected.  
2. **Validating Druid Data Sources** – Checking data availability and querying necessary sources.  
3. **Confirming Report Generation** – Investigating missing or outdated reports and resolving discrepancies.  

### **When to Use this Guide**  
- If the Admin or Program dashboard is **not displaying updated data**.  
- If **specific reports** are missing or showing incorrect data.  
- If there are **errors in batch ingestion logs** or **Druid data sources**.  

## **Step 1: Verify Data Pipelines**  
To debug issues with the Admin and Program Dashboards, follow these steps:  

### **1. Check Data Pipeline Execution**  
Ensure that all data pipelines are running properly every day. The Admin and Program Dashboards rely on batch ingestion. Below is a list of data sources and their corresponding batch scripts:  

| No. | Script Name | Data Source |  
|----|------------------------------|------------------------------------|  
| 1  | `pyspark_project_batch.py` | `ml-project-status` |  
| 2  | `pyspark_observation_status_batch.py` | `ml-obs-status`, `ml-obs-domain`, `ml-obs-domain-criteria` |  
| 3  | `pyspark_sur_distinct_count_status.py` | `ml-surveydistinctCount-status` |  

### **2. Check Logs**  
To analyze logs on the `ml-analytics-service` server:  
- **Pipeline Logs:** `/opt/ml-analytics-service/logs`  
- **Cron Job Logs:** `/opt/ml-analytics-service/cron-tab.logs`  

**Note** : If you find any discrepancies in the data pipeline logs, please follow this document to resolve the issue [Data Pipeline Verification](/debug-documentation/data-pipeline.md) .
### **3. Reference Documentation**  
Use the **[Admin and Program Dashboard Report Doc](https://docs.google.com/spreadsheets/d/1Eoa8ggJAW3fz7AM1NhZsX-QrMtP8_DxtJ__LGlWyzj0/edit?gid=68465466#gid=68465466)** to identify which reports depend on which Druid data sources.  

---

## **Step 2: Validate Druid Data Sources**  

1. Navigate to the **Druid Data Sources** section.  
2. Click on each data source to check its availability.  
3. Ensure **data source availability is 100%**.  
4. Click on the **Query** tab in the top navigation bar.  
5. Run queries for all data sources listed in the table above.  

**Note:**  
- The `__time` field should reflect data from **one day before**.  
  - Example: If today’s date is **05/02/2025**, then `__time` should be **04/02/2025**.  
- All reports fetch data from the previous day.  

---

## **Step 3: Validate Reports**  

1. If a report is not updating on the dashboard:  
   - Open the **Network** tab in the browser.  
   - Get the **report_id** from the request.  
   ![Dashboard Screenshot](/debug-documentation/images/report_id.png)

   - Identify the corresponding **data source** for that report_id and validate it.  

2. If the **data source is 100% available** and contains the required data from the previous day, check **blob storage** at:  
   - `reports/hawk-eye/{report_id}`  

3. If the CSV for today's date exists, it indicates that CSVs were generated successfully.  

4. If the CSV file is **missing**, run the **run-report job** for the corresponding report_id.  

---