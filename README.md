# AdverseData
This project is to get the adverse events data related tothe pharm_class_epc ="nonsteroidal+anti-inflammatory+drug&quot,Thiazide Diuretic and TumorNecrosis Factor Blocker" from the provided API link, process it using pyspark and store it in effective way.

# Tools Used:
1) Databricks
2) Online free PowerPoint

# File Names and its short description:
1) adverse process flow.JPG: This diagram will show theProcess flow diagram designed end to end.
2) adverse_data_t1.ipynb: This code helps to hit the APIlink and get data related to "Tumor Necrosis Factor Blocker"
3) adverse_data_t2.ipynb: This code helps to hit the APIlink and get data related to "nonsteroidal+anti-inflammatory+drug"
4) adverse_data_t3.ipynb: This code helps to hit the APIlink and get data related to "Thiazide Diuretic"
5) Notepad readable code.py: This is just the same code as"adverse_data_t3.ipynb" just in notepad readable format for quick reference.
6) create-job_1.json: Json data to create job for adverse_data_t1.ipynb
7) create-job_2.json: Json data to create job for adverse_data_t2.ipynb
8) create-job_3.json: Json data to create job for adverse_data_t3.ipynb

# adverse_data_t1.ipynb -- Description:
Step1: Provided required URL details and using the requests’function checked whether the connection was established correctly or not. If successful,then extract the data related to "Tumor Necrosis Factor Blocker" andstore it in a data variable.
Step2: Store the raw data in bronze S3 location (raw layer),to maintain history and have the raw data for later analysis or Engineeringpurposes.
Step3: Applying various json functions to transform the jsondata to dataframe.
Step4: In the FDA portal its mentioned as schema is notstatic. So, choose the Delta table to store the data. Additionally, delta tables areSchema free, support acid transactions, highly performance oriented, inbuiltversion control and read older versions of data using time travel functions whichare very much suitable for this use case.
Step5: Created Delta table on the fly while writing the datafrom dataframe to Gold s3 location (Final layer).
Step6: In the FDA portal based on the interactive chart data, assumed few columns are mainly used for driving the adverse data in terms of analysis. Based on that choose qualification and receivedate columns as partition columns. Additionally, choose safetyreportid column as Primary column for Merge conditions.
Step7: In the FDA portal it's mentioned as, any history data will get updated anytime. so I used the MERGE operation for daily job.
Step8: As data grows drastically, we need to make sure the data is stored and accessed effectively. So added the Optimize command at the end of the code.

# Commands to run the create job in Databricks CLI:
databricks jobs create --json-file create-job_1.json
databricks jobs create --json-file create-job_2.json
databricks jobs create --json-file create-job_3.json

# Commands to run the jobs in Databricks CLI:
databricks jobs run-now --job-id 1
databricks jobs run-now --job-id 2
databricks jobs run-now --job-id 3

# Note:
The adverse_data_t2 and adverse_data_t3 descriptions are similar to adverse_data_t1.ipynb, the only difference is pharm_class_epc values.
