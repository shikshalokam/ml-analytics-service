## Data Pipeline Verification [ml-analytic-service]
Follow this document for the first-time deployment of [ml-analytics-service](https://ed.sunbird.org/use/source-code/manage-learn/ml-analytics-service).

### Step 1: Verify Deployment 
1. Ensure the build and deployment job for `ml-analytic-service` is completed.
2. Log into the server of `ml-analytic-service`.
3. Check the logs. if no errors in the logs, then the scripts are running successfully.

Follow these steps to set up the Druid data source for the real-time data pipeline.

### Step 2: Set Up Druid
1. Navigate to the Jenkins job path:
   - `Dashboard/Deploy/{ENV-NAME}/DataPipeline/DruidIngestion`
2. Click on **Build with Parameters**.
3. Fill in the required details:
   - **Private_Branch**
   - **branch_or_tag**
4. Choose the ingestion task name from the list. If not available, add the following task names:
   ```
   raw_sl_survey, raw_sl_observation, raw_sl_observation_evidence, raw_sl_survey_evidence
   ```
5. Click **Build**.
6. Once the build is completed:
   - Go to the **Druid UI**.
   - Click on **Supervisors** and ensure all 4 supervisors are in **RUNNING** state with the following data sources:
     ```
     sl-survey, sl-observation, sl-observation-evidence, sl-survey-evidence
     ```
   - Check the **Tasks** section to confirm all data source tasks are in **RUNNING** state.

### Step 3: Validate Submission
1. Submit **survey** and **observation** via the portal.
2. Check the cron-tab.logs for **submission IDs**.

### Step 4: Verify Data Sources
1. Confirm that all the mentioned data sources are created.
2. Ensure data is present after submission.

**Note:**
Batch ingestion data sources are created after running batch scripts.

