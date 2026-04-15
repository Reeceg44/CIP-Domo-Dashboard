# CIP Domo Dashboard Overview
I created a dashboard dedicated to tracking the balance in Briggs & Stratton's CIP (Construction in Process) balance sheet account. The project uses anonymized data to protect company data integrity. 

Tools Used: SAP S/4HANA, DOMO, Excel, Google Sheets

Dataset: CIP Dataset created from joining multiple tables within DOMO's native ETL tool.

## 🧠 Project Goal and Background

- The goal of this project was to automate the process of pulling multiple tables together manually on a monthly basis related to CIP. This automated solution assists the business in properly tracking current capital projects in CIP. 

- If a project with its assets is considered to be in-service, it is the responsibility of the project's business segment to capitalize the project and its assets. This dashboard helps to facilitate this process.

## ETL Preview:

<img width="1889" height="707" alt="image" src="https://github.com/user-attachments/assets/6d8a5348-6a4a-4649-82d7-ca7c0b643bdc" />


## 1. Data Sources

<img width="20" height="20" alt="image" src="https://github.com/user-attachments/assets/392f9180-0eb4-4d71-9cc7-e8cda5ad479e" /> CIP from 2018 to 2024 (Excel Upload)

- This dataset contains CIP account balance sheet data posted in journal entries from 2018 to 2024. It was originally exported from SAP into Excel, and then uploaded to DOMO.

<img width="20" height="20" alt="image" src="https://github.com/user-attachments/assets/b0a49e58-0ad9-4739-a164-fed8cdcd9724" /> CIP since 1/1/2025 (DOMO Dataset View)

- This dataset also contains CIP account balance sheet data posted in journal entries except it's a current and ongoing DOMO data view. It contains data since 1/1/2025 from SAP.
- This dataset doesn't include data from pre-2025 because the pre-2025 balance sheet data behind this dataset view isn't presented in a consistent way between DOMO and SAP. The pre and post 2025 data are appended later in the ETL to help paint the full picture.  

<img width="20" height="20" alt="image" src="https://github.com/user-attachments/assets/5444f2c5-c0a0-42dd-97ae-a5b63a4f4fc7" /> PRPS Table from SAP

- PRPS is a table native to SAP S/4HANA. It contains information pertaining to WBS elements such as project names, descriptions, company codes, etc.
- This PRPS table is considered by DOMO to be an ODBC (Open Database Connectivity). The dataset updates on a daily basis.

<img width="20" height="20" alt="image" src="https://github.com/user-attachments/assets/89fed7ab-5d62-48f4-a9d8-5dc82c025ad7" /> CIP Open Projects Google Sheet

- The function of this Google sheet is to list relevant information related to WBS elements with non-zero balances in CIP. It is connected to DOMO via DOMO's native Google Sheets connector tool. 
- Finance representatives from various business units have the ability to make changes to items such as in-service dates, project managers, and comments regarding the status of WBS elements.
- The information in this Google sheet is not readily available in SAP, so it is joined with other tables in the ETL.

<img width="20" height="20" alt="image" src="https://github.com/user-attachments/assets/5444f2c5-c0a0-42dd-97ae-a5b63a4f4fc7" /> RPSCO Table from SAP

- RPSCO is another native table to SAP connected to DOMO via an ODBC. 
- It containes budget information for object numbers within the SAP ecosystem.
- WBS elements from PRPS each have their own object number. Thus, RPSCO and PRPS are joined together in the ETL to show budget infrmation by WBS element. 

