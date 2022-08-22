

# Credit Card Fraud Trends and Predictions

## Project Information:
- Reason we selected this topic:
  - We selected this topic due to our team's background in the financial services sector, including two teammates with credit card experience
  - Additionally, the increased risk of credit card fraud can create significant financial and reputational losses for businesses so it is critical to detect fraudulent activity in a fast and efficient manner.
  - The model we have built can be utilized by businesses, such as credit card companies and banks, to create a more accurate and expedited fraud detection process. 
- Questions we hope to answer with the data:
  - Are there any trends in the data?
  - Can any of these trends be used to predict and prevent future fraudulent transactions?

## Presentation

*Link to Google Slides:* https://docs.google.com/presentation/d/1clOUcDR-ilnj36s_9WvjgXK9cDWncTfy4-4wAS_239I/edit?usp=sharing

## Dashboard

*Link to Dashboard:* https://public.tableau.com/app/profile/ashleigh.bridges/viz/Credit_Card_Fraud_v7/Observations?publish=yes

## Dataset Description:
Our dataset is a simulated credit card transaction dataset, covering the cards of 1000 customers with a pool of 800 merchants. The dataset covers transactions over a  full two year period. It contains both legitimate and fraudulent transactions
- *Link to dataset: https://www.kaggle.com/datasets/kartik2112/fraud-detection*

Our team reviewed the data from Kaggle and created an Entity Relationship Diagram to visualize how best to split up the data to best suit our needs. We decided to create two separate tables within PostgreSQL:
- Model_Upload table: Contains those attributes which can be used in machine learning models
- Personal_Info table: Contains those attributes which are more personal in nature and is commonly referred to as "PII"

![ERD](https://user-images.githubusercontent.com/100883212/185511296-cf307011-007d-4764-a5cf-3319a40c654f.png)

*Entity Relationship Diagram*

After verifying all of the necessary data was imported correctly, we placed the tables in an S3 area within AWS to then be linked to the machine learning model.

The following steps were used to connect the PostgreSQL database to the machine learning model:

```
1. Create Database in Postgres Credit_Card_Fraud

2. Create tables in Postgres
   	model_upload_s
	personal_info_s

3. Set up within Anaconda: pip install psycopg2

4. Set up Build:
	python setup.py build
	sudo python setup.py install

   5. Python Coding: Connect to the Postgres Database by use of keyword arguments: Checking username and password
        conn = psycopg2.connect(host=”localhost”,
        database-“Credit_Card_Fraud”,
        user=”postgres”
        password=”ABCD1234”) (Password set up for Postgres)

        6. Add database.ini to the .gitignore file

        7. The .gitignore file will be shown as this: database.ini

           8. The config() function is placed in the config.py file: The following config()    
              function reads the database.ini file and returns connection parameters.

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

9. Query #a The following queries data from a Postgres database using the fetchone() method. 
            This data is used to populate tables used in Pandas for machine learning analysis
		 
def get_model_upload_s ():
""" query data from the model_upload_s  table """
    conn = None

try:
    params = config()
    conn = psycopg2.connect(**params)
    cur = conn.cursor()
    cur.execute("SELECT trans_num, merchant, category, amt, gender, city, state, zip,          
                 lat, long, city_pop, job, unix_time, merch_lat, merch_long, 
                 is_fraud” 
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

  Query #b: JOIN To select those fraud amounts greater than 1000
  def get_model_upload_s ():
     """ query data from the model_upload_s  table """
     conn = None

try:

    params = config()
    conn = psycopg2.connect(**params)    
    cur = conn.cursor()    
    cur.execute("Select pi.first, pi.last, pi.street, mu.city, mu.amt, mu.merchant, 
                 mu.job
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

10. Query #c: Counts: To select count of fraud cases
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
```

## Machine Learning Process:
As the purpose of the machine learning is to predict if a transaction is fraud (binary outcome), we immediately leaned towards a logistic regression testing model. The next decision was between supervised or unsupervised. We chose supervised as we already knew the question we wanted answered (is this fraud or not?). 

Due to the HUGE amount of data we had at our disposal and the clear imbalance in good vs. fraud transaction, we knew that the accuracy of the model would be highly skewed. As such, we focused on the Precision and Recall/Sensitivity rates of our models.

As part of the exercise, evaluating the data and identifying what could be most impactful in reaching our goals. This preprocessing resulted in several features being deemed irrelevant.

The model we built pulls the data from an S3 bucket, which acts as a repository for this data. We treated this as if this is were a location in which the file would have been posted. The model then creates a few new columns of data based on the time of the transaction and the amount of time passed between the last transaction on the same account. It also performs bucketing based on region and adds this data to a column. Finally, we begin filtering out unnecessary features (‘state’, ’merch_lat’, ‘merch_long’, ‘lat’, ‘long’, ‘unix_time’, ‘cc_num’, ‘unnamed: 0’, ‘trans_date_trans_time’, ‘merchant’, ‘dob’, ‘first’, ‘last’, ‘street’, ‘trans_num’, ‘city’, ‘job’). The data is then split, scaled, and trained.  

Models tested were SMOTE, Random Under sampling, Random Oversampling, & Random Forest. These were tested first with the original data only filtered, then with the new features and filting, and lastly a mix of the two for best results.

The final results were as follows:

- Accuracy was 100% (this is due to the skewed amount of valid transactions)
- Precision was near 100% for both valid and fraud transactions
  - This is significant as it shows that the model is correctly detecting fraudulent transactions 98% of the time
  - This also sets the lowest threshold for requiring an employee to call a customer to confirm a transaction was fraudulent
- Recall/Sensitivity was 100% for valid transactions
  - This was only 76% for fraud and is where the most opportunity for the model remained
  - This indicates that for every 3 transactions identified as fraud, there was still 1 fraudulent transaction not being identified
- While the recall rate of fraud transactions was better in other iterations, they resulted in lower recall rates for valid transactions and the amount of work and costs needed to clear that volume would likely outweigh the potential losses of the missed fraudulent transactions

## Model Improvement:
If we had more time, we would work to quantify distance of the transaction from the home market (if this was not a moto transaction). We would attempt to identify the type of transaction (chip, swipe, moto, token) and we would quantify if the spend was out of the norm for the customer. One thing we identified was that much of the fraud in this set were large dollar transactions, while our experience working in fraud has been that most fraudulent transactions and the total fraud on accounts are under $200 per transaction. As such, our model may inaccurately favor detecting larger transactions as fraud, which would not be evident until we were able to factor in more low-dollar fraud transactions.


