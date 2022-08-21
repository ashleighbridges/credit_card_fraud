# Credit Card Fraud: Dataset Description and Database Summary Process

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
7.	Data Testing: The data was then tested and reviewed by means of creating  joins within the tables and related queries such as performing counts on specific             types of data
8.	Amazon Web Services: After the review steps outlined above, the 2 tables were then placed in the S3 area within AWS to be used in our machine learning analysis
9.      Tableau: Copies of the same 2 files were additionally used for our visualization charts within Tableau



             Screenshots of the above steps can be viewed at: 

             https://github.com/ashleighbridges/credit_card_fraud/tree/main



Database: Python Connection

	Postgres Connection Procedures	
Steps: 		
1	Create Database in Postgres	Credit_Card_Fraud
		
2	Create tables in Postgres	
		model_upload_s
		personal_info_s
		
3	Set up within Anaconda: 	pip install psycopg2
		
4	Set up Build: 	
		python setup.py build
		sudo python setup.py install
		
4	Python Coding: 	Connect to the Postgres Database by use of keyword arguments: Checking username and password
		conn = psycopg2.connect(
        host=”localhost”,
	
        database-“Credit_Card_Fraud”,
	
        user=”postgres”
        password=”ABCD1234”) (Password set up for Postgres)

		
5	 Add database.ini to the .gitignore file
	
6	The .gitignore file will be shown as this: database.ini

7	The config() function is placed in the config.py file:	The following config() function reads the database.ini file and returns connection parameters.

        The config fig() function is placed in the config.py file as shown
		#!/usr/bin/python

        from configparser import ConfigParser

        def config(filename='database.ini', section='postgresql'):

        # create a parser
    
        parser = ConfigParser()
    
        # read config file
    
        parser.read(filename)
    
        # get section, default to postgresql
    
        db = {}
    
    if parser.has_section(section):
    
        params = parser.items(section)
        for param in params:
              db[param[0]] = param[1]
       
    else:
    
       raise Exception('Section {0} not found in the {1} file'.format(section, filename))
          

     return db


8	Query #1	The following queries data from a Postgres database using the fetchone() method. This data is used to populate tables used in Pandas for 

                         machine learning analysis
			 
	def get_model_upload_s ():
    """ query data from the model_upload_s  table """
        conn = None
   
    try:
    
        params = config()
        
        conn = psycopg2.connect(**params)
        
        cur = conn.cursor()
        
        cur.execute("SELECT trans_num, merchant, category, amt, gender, city, state, zip, lat, long, 
                     city_pop, job, unix_time, merch_lat, merch_long, is_fraud”

                     FROM  model_upload_s  
                     ORDER BY trans_num")

        print("Credit records from model_upload_s file: ", cur.rowcount)
        
        row = cur.fetchone()
        
        while row is not None:
        
            print(row)
            
            row = cur.fetchone()
            
        cur.close()
        
            except (Exception, psycopg2.DatabaseError) as error:
    
            print(error)
        
    finally:
    
            if conn is not None:
        
            conn.close()




9	Query #2: JOIN	To select those fraud amounts greater than 1000

	def get_model_upload_s ():
    """ query data from the model_upload_s  table """
        conn = None
   
    try:
    
        params = config()
        
        conn = psycopg2.connect(**params)
        
        cur = conn.cursor()
        
        cur.execute("Select pi.first, pi.last, pi.street, mu.city, mu.amt, mu.merchant, mu.job
                                 from model_upload_s mu
                                 JOIN personal_info_s pi
                                 ON mu.trans_num = pi.trans_num
                                 where mu.amt> 1000”


        print("Credit records from model_upload_s file: ", cur.rowcount)
        
        row = cur.fetchone()
        
        while row is not None:
        
            print(row)
            
            row = cur.fetchone()
            
         cur.close()
        
    except (Exception, psycopg2.DatabaseError) as error:
    
        print(error)
        
    finally:
    
        if conn is not None:
        
            conn.close()


10	Query #3: Counts: To select count of fraud cases
		def get_model_upload_s ():
    """ query data from the model_upload_s  table """
        conn = None
   
    try:
    
        params = config()
        
        conn = psycopg2.connect(**params)
        
        cur = conn.cursor()

        
        cur.execute("Select count (*)
                                 From model_upload_s mu
                                 JOIN personal_info_s  pi
                                 ON mu.trans_num = pi.trans_num
                                 where mu.amt > 1000”

        print("Credit records from model_upload_s file: ", cur.rowcount)
        
        row = cur.fetchone()
        
        while row is not None:
        
            print(row)
            
            row = cur.fetchone()
            
        cur.close()
        
    except (Exception, psycopg2.DatabaseError) as error:
    
        print(error)
        
    finally:
    
        if conn is not None:
        
            conn.close()


		














