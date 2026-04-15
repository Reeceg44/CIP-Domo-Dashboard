# CIP Domo Dashboard Overview
I created a dashboard dedicated to tracking the balance in Briggs & Stratton's CIP (Construction in Process) balance sheet account. The project uses anonymized data to protect company data integrity. 

Tools Used: SAP S/4HANA, DOMO, Excel, Google Sheets

Dataset: CIP Dataset created from joining multiple tables within DOMO's native ETL tool.

## 🧠 Project Goal and Background

- The goal of this project was to automate the process of pulling multiple tables together manually on a monthly basis related to CIP. This automated solution assists the business in properly tracking current capital projects in CIP. 

- If a project with its assets is considered to be in-service, it is the responsibility of the project's business segment to capitalize the project and its assets. This dashboard helps to facilitate this process. 

## 1. Pulling the data sources together

Data Sources:

<img width="20" height="20" alt="image" src="https://github.com/user-attachments/assets/392f9180-0eb4-4d71-9cc7-e8cda5ad479e" /> CIP from 2018 to 2024 (Excel Upload)

- This dataset contains CIP account balance sheet data posted in journal entries from 2018 to 2024. It was originally exported from SAP into Excel, and then uploaded to DOMO.

<img width="20" height="20" alt="image" src="https://github.com/user-attachments/assets/b0a49e58-0ad9-4739-a164-fed8cdcd9724" /> CIP since 1/1/2025 (DOMO Dataset View)

- This dataset also contains CIP account balance sheet data posted in journal entries except it's a current and ongoing DOMO data view. It contains data since 1/1/2025 from SAP.
- This dataset doesn't include data from pre-2025 because the pre-2025 balance sheet data behind this dataset view isn't presented in a consistent way between DOMO and SAP. The pre and post 2025 data are appended later in the ETL to help paint the full picture.  

    
