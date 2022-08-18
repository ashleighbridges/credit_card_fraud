# Credit Card Fraud: Dataset Descrption and Database Summary Process

Steps involved with the derivation of our dataset and database creation involved the following: 

1.	Data Used: Our dataset from Kaggle.com  is a simulated credit card transaction dataset covering the credit cards of 1000 customers from a pool of 800 merchants. The data set covers transactions over a 2 year period covering legitimate as well as fraudulent transactions.

        Link to Dataset: https://www.kaggle.com/datasets/kartik2112/fraud-detection

2.	Data Review: The Kaggle data was reviewed and analyzed by our Machine Learning team and placed into 2 separate tables: 

        a.	Model_Upload table: Contains those attributes which can be used in machine learning models
        b.	Personal Info table: Contains those attributes which are more personal in nature
           and is commonly referred to as “P.I”

3.	Creation of Database: A database was created in Postgres (named Credit_Card_Fraud) with the 2 tables created with this database. 
 
4.	Database Schema: A database schema was created which displayed the tables, attributes and appropriate data types and characteristics assigned

5.	Create Tables: Tables were then created by means of queries within the Credit_Card_Fraud database within Postgres. 
6.	CSV Files/Upload: Data suiting the appropriate columns were created as CSV files and then uploaded within Postgres to the appropriate 2 tables
7.	Data Testing: The data was then tested and reviewed by means of creating  joins within the tables and related queries such as performing counts on specific types      of data
8.	Amazon Web Services: After the review steps outlined above, the 2 tables were then placed in the S3 area within AWS to be used in our machine learning analysis
9. Tableau: Copies of the same 2 files were additionally used for our visualization charts within Tableau



             Screenshots of the above steps can be viewed at: 

                        XXXXXXXXXXXXXXXXX

















