# credit-card-customer-analysis-report
Credit card -customer Analysis Dashboard



Credit Card Financial Dashboard
1.Project Overview:
There are two dashboards on weekly basis-
1.Transaction based credit card Report
2.Customer based analysis Report
Project Objective
	To develop a comprehensive credit card weekly dashboard that provides real-time insights into key performance metrics and trends, enabling stakeholders to monitor and analyze credit card operations effectively.
	How to refresh data with added new data in dashboards.
Datasets Overview: used two datasets
	Customer details dataset
	Credit Card details dataset

Credit card details dataset 
This table has 20 columns and ---rows.
Columns are-1.Client_Num (data type-Whole Number)
                        2.Card_category(data type- Text)
                        3.Annual_Fees(data type-Whole Number)
                        4. Acivation_30 days (data type-Whole Number)
                        5.Customer_Acquisition_cost ( data type-Whole Number)
                        6.Week_started (data type-date)
                        7.week_number(data type-Text)
                        8.Quater (data type-Text)
                        9.Current_year (data type-Whole Number)
                       10.Credit_limit (data type- Decimal Number)
                       11.Total_revolving_balance (data type-Whole Number)
                       12.Total_trans_Amount (data type-Whole Number)
                   13.Total_trans_vol (data type-Whole Number)
                   14.Avg_utilization_ratio (data type-Decimal Number)
                   15. Use_ chip (data type-Text)
                   16. Exp_type (data type-Text)
                   17.Interest_earned (data type-Decimal Number)
                   18.Delinquent_Acc (data type-Whole Number)
                   19. Revenue (data type-Decimal Number)
                   20. week_num (data type-Whole Number)
Customer Details dataset
This table has 15 columns and ---rows.
Columns are-    1.Client_Num ( data type-Whole number)
                            2.Customer_Age (data type-Whole number)
                            3. Gender (data type-Text)
                            4.Dependent_count(data type-Whole number)
                            5. Educational Level- (data type-Text)
                            6. Marital_status-(data type-Text)
                            7.State_cd – (data type-Text) 
                            8.Zipcode- (data type-Whole number)
                            9. Car_owner- (data type-Text)
                           10.House_owner-(data type-Text)
                           11.Personal_Loan-(data type-Text) 
                           12.contact- (data type-Text)
                           13.customer_job- (data type-Text)
                           14.Income-( data type-Whole number) 
                           15. Cust_satisfaction_score- (data type-Whole number)

Steps : followed in this project-
1.	Imported data to SQL database(Data Extraction)
	Prepare csv file
	Create tables in SQL
	Imported csv file into SQL
	created table in SQL.
	Connected and imported table to Power BI 
SQL queries used to create database ans table–
CREATE database cc_db;
USE cc_db;
CREATE table cc_details (Client_Num INT,
    Card_Category VARCHAR(20),
    Annual_Fees INT,
    Activation_30_Days INT,
    Customer_Acq_Cost INT,
    Week_Start_Date DATE,
    Week_Num VARCHAR(20),
    Qtr VARCHAR(10),
    current_year INT,
    Credit_Limit DECIMAL(10,2),
    Total_Revolving_Bal INT,
    Total_Trans_Amt INT,
    Total_Trans_Ct INT,
    Avg_Utilization_Ratio DECIMAL(10,3),
    Use_Chip VARCHAR(10),
    Exp_Type VARCHAR(50),
    Interest_Earned DECIMAL(10,3),
    Delinquent_Acc VARCHAR(5)
);
SELECT* from cc_details;
CREATE TABLE cust_detail (
    Client_Num INT,
    Customer_Age INT,
    Gender VARCHAR(5),
    Dependent_Count INT,
    Education_Level VARCHAR(50),
    Marital_Status VARCHAR(20),
    State_cd VARCHAR(50),
    Zipcode VARCHAR(20),
    Car_Owner VARCHAR(5),
    House_Owner VARCHAR(5),
    Personal_Loan VARCHAR(5),
    Contact VARCHAR(50),
    Customer_Job VARCHAR(50),
    Income INT,
    Cust_Satisfaction_Score INT
);
SELECT * from cust_detail;
Imported csv file into SQL—



2.	Data Processing And DAX Queries
	Checked null values in columns
	Added new column(AgeGroup) in customer detail table using DAX query
AgeGroup = SWITCH(
TRUE(),
 'customer'[Customer_Age] <30,"20-30",
'customer'[Customer_Age]>=30 && 'customer'[Customer_Age] <40,"30-40",
'customer'[Customer_Age] >=40 &&'customer'[Customer_Age]<50,"40-50",
'customer'[Customer_Age] >=50 &&'customer'[Customer_Age]<60,"50-60",    
'customer'[Customer_Age] >=60,"60+",
 "unknown" )     

•	Created Income groups column in customer detail table using following query
        INCOME group = SWITCH( 
         TRUE(),
                 'customer'[Income]<35000,"LOW",
                 'customer'[Income]>=35000 &&'customer'[Income]<70000,"MEDIUM",
                 'customer'[Income]>=70000,"HIGH",
                  "UNKNOWN" )


•	Created new Revenue column in credit card detail table using following query
       Revenue =     (credit_card[Annual_Fees]+credit_card[Interest_Earned]+credit_card[Total_Trans_Amt])
•	Created new week number column in credit card detail table using following query, so that we can sort values according to week number
                            week_num2 = WEEKNUM(credit_card[Week_Start_Date])


	Added 2 new measures in credit card details table --
	current revenue measure using DAX query          
   current_week_revenue = CALCULATE(
    SUM(credit_card[Revenue]),
                    FILTER(
ALL('credit_card'),'credit_card'[week_num2] =MAX(credit_card[week_num2])))

	Previous week revenue measure using DAX query
previous_week_revenue = CALCULATE(
SUM(credit_card[Revenue]),
FILTER(
           ALL('credit_card'),'credit_card'[week_num2] =MAX(credit_card[week_num2])-1))


	To calculate Week over week revenue added new measure in credit card detail table
wow_revenue = DIVIDE(('credit_card'[current_week_revenue]-'credit_card'[previous_week_revenue]),'credit_card'[previous_week_revenue])
	changed values in Percentage, removed decimal values

3.	Designed 2 dashboards for insights
1.Transaction based credit card Report
      2.Customer based analysis Report
4. Updation of Dashboards with new week data

5.	Project Insights- week-53 (31st dec)
Week on week change:
•	Revenue increased by 28.8%
•	Total Transaction Amount & count increased by 
•	Customer count increased by
Overview YTD:
•	Overall revenue is 57 M.
•	Total interest is 8M.
•	Total transaction amount is 46M.
•	Male customers are contributing more in revenue i.e.31M ,female 26M.
•	Blue &Silver credit card  are contributing to 93% of overall transactions.
•	TX,NY,& CA are top contributing regions.
•	Overall activation rate is 59.49%.
•	Overall Delinquent rate is 5.67%

-------------------------------------------------------end of project------------------------------------------------------------
