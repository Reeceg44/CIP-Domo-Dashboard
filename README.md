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

<img width="20" alt="image" src="https://github.com/user-attachments/assets/392f9180-0eb4-4d71-9cc7-e8cda5ad479e" /> CIP from 2018 to 2024 (Excel Upload)

- This dataset contains CIP account balance sheet data posted in journal entries from 2018 to 2024. It was originally exported from SAP into Excel, and then uploaded to DOMO.

<img width="20" alt="image" src="https://github.com/user-attachments/assets/b0a49e58-0ad9-4739-a164-fed8cdcd9724" /> CIP since 1/1/2025 (DOMO Dataset View)

- This dataset also contains CIP account balance sheet data posted in journal entries except it's a current and ongoing DOMO data view. It contains data since 1/1/2025 from SAP.
- This dataset doesn't include data from pre-2025 because the pre-2025 balance sheet data behind this dataset view isn't presented in a consistent way between DOMO and SAP. The pre and post 2025 data are appended later in the ETL to help paint the full picture.  

<img width="20" alt="image" src="https://github.com/user-attachments/assets/5444f2c5-c0a0-42dd-97ae-a5b63a4f4fc7" /> PRPS Table from SAP

- PRPS is a table native to SAP S/4HANA. It contains information pertaining to WBS elements such as project names, descriptions, company codes, etc.
- This PRPS table is considered by DOMO to be an ODBC (Open Database Connectivity). The dataset updates on a daily basis.

<img width="20" alt="image" src="https://github.com/user-attachments/assets/89fed7ab-5d62-48f4-a9d8-5dc82c025ad7" />  CIP Open Projects Google Sheet

- The function of this Google sheet is to list relevant information related to WBS elements with non-zero balances in CIP. It is connected to DOMO via DOMO's native Google Sheets connector tool. 
- Finance representatives from various business units have the ability to make changes to items such as in-service dates, project managers, and comments regarding the status of WBS elements.
- The information in this Google sheet is not readily available in SAP, so it is joined with other tables in the ETL.

<img width="20" alt="image" src="https://github.com/user-attachments/assets/5444f2c5-c0a0-42dd-97ae-a5b63a4f4fc7" /> RPSCO Table from SAP

- RPSCO is another native table to SAP connected to DOMO via an ODBC. 
- It containes budget information for object numbers within the SAP ecosystem.
- WBS elements from PRPS each have their own object number. Thus, RPSCO and PRPS are joined together in the ETL to show budget infrmation by WBS element.

## 2. Formula Adds, Joins, and Group By

- Formulas/additional columns were added to the "CIP from 2018 to 2024" Excel upload to possess the same amount of columns as the "CIP since 1/1/2025" in order for the later append between the two datasets to function properly.

<img width="1353" alt="image" src="https://github.com/user-attachments/assets/61f4f9eb-73f8-4f9e-9bc9-f9ed4e81b6e0" />

<br><br>
- The CIP SAP G/L data append is joined with the PRPS table from SAP with the following expression. The REGEXP_REPLACE function is used to link the datasets together even if the join column of "Assignment" from the "Append Rows" dataset and the "POSID" column from the "Select_PRPS" have inconsistent formatting. The pattern "[^a-zA-Z0-9]" looks for any character that is not a lowercase letter, not an uppercase letter, and not a number. It then replaces non-alphanumeric characters (spaces, dashes, slashes, periods) with nothing (an empty string).

<img width="1058" alt="image" src="https://github.com/user-attachments/assets/a7d57037-1828-421f-932e-5ca371c57666" />

<br><br>
- A group by tile is used for the RPSCO table from SAP. Column OBJNR (Object Number) specifies the grouping. The "Budget" and "Actuals" columns are created in the group by to show budget and actual values by project number.
- The "Budget" and "Actuals" columns are both aggregated with the SUM() function. Both use CASE statements to determine whether or not to include rows from RPSCO in their respective aggregations.
- The VORGA (Budget Type) column is used to establish the SUM() parameters in the case statement.
- "KBUD" indicates that VORGA is returning budget values for a given row. "COIN" indicates that VORGA is returning actual values for a given row; therefore, it should sum WLP01 through WLP16.
- The "WLPXX" fields in this table contain actual values for the periods in a given year. There are only 12 months in a year so WLP13 through WLP16 are used as special periods in SAP.

  <img width="1355" alt="image" src="https://github.com/user-attachments/assets/ae9718c2-f971-4da4-85d1-1fc69e59a25c" />

## 3. Data Anonymization and Obfuscation

- Most of the fields across this ETL contain sensitive information about the company; thus, I have employed multiple data anonymization techniques to protect the company's data privacy.
- For most of the qualitative fields, I concatenated the base column with a RAND() function which returns a pseudo-random value between 0 and 1. I then wrapped them in a SHA1() functions to generate 40-character hexadecimal strings based on the partially randomized values. 
- For the date columns, I used DATE_ADD and DATE_SUB functions to add or subtract an undisclosed number of days from a given column with the date data type.
- The numeric columns such as "Actuals" were obfuscated with RAND() functions as well. An example is IFNULL(`Actuals`* (1 + (RAND() * 0.05)),0).

 <img width="1386" alt="image" src="https://github.com/user-attachments/assets/b8df267d-dfd5-48d1-af6d-9f4613dd8ee8" />

<br><br>
- One of the visualizations used in the dashboard is a chart showing the running total balance of the CIP account over time. In order to obfuscate the trend line of the balance, I used a group by tile to group the rows by the anonymized posting date column and then aggregate the same column with a count function.

<img width="1269"  alt="image" src="https://github.com/user-attachments/assets/2e49fc9b-7d90-48e3-ad09-bb2fd646f329" />

  
