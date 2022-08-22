Credit Card Fraud Trends and Predictions

Communication Protocols:

We will meet during class time on Tuesdays/Thursdays, once weekly on either Sunday or Monday evening, and as needed at other times
Slack will be our main point of communication outside of meetings
Project Information:

Reason we selected this topic:
We selected this topic due to our team's background in the financial services sector, including two teammates with credit card experience
Additionally, the increased risk of credit card fraud can create significant financial and reputational losses for businesses so it is critical to detect fraudulent activity in a fast and efficient manner.
The model we have built can be utilized by businesses, such as credit card companies and banks, to create a more accurate and expedited fraud detection process.
Questions we hope to answer with the data:
Are there any trends in the data?
Can any of these trends be used to predict and prevent future fraudulent transactions?
Google Slides Link

https://docs.google.com/presentation/d/1clOUcDR-ilnj36s_9WvjgXK9cDWncTfy4-4wAS_239I/edit?usp=sharing

Dataset Description:

Our dataset is a simulated credit card transaction dataset, covering the cards of 1000 customers with a pool of 800 merchants. The dataset covers transactions over a full two year period. It contains both legitimate and fraudulent transactions

Link to dataset: https://www.kaggle.com/datasets/kartik2112/fraud-detection
Our team reviewed the data from Kaggle and created an Entity Relationship Diagram to visualize how best to split up the data to best suit our needs. We decided to create two separate tables within PostgreSQL:

Model_Upload table: Contains those attributes which can be used in machine learning models
Personal_Info table: Contains those attributes which are more personal in nature and is commonly referred to as "PII"
ERD

Entity Relationship Diagram

After verifying all of the necessary data was imported correctly, we placed the tables in an S3 area within AWS to then be linked to the machine learning model.

Machine Learning Process:

As the purpose of the machine learning is to predict if a transaction is fraud (binary outcome), we immediately leaned towards a logistic regression testing model. The next decision was between supervised or unsupervised. We chose supervised as we already knew the question we wanted answered (is this fraud or not?).

Due to the HUGE amount of data we had at our disposal and the clear imbalance in good vs. fraud transaction, we knew that the accuracy of the model would be highly skewed. As such, we focused on the Precision and Recall/Sensitivity rates of our models.

As part of the exercise, evaluating the data and identifying what could be most impactful in reaching our goals. This preprocessing resulted in several features being deemed irrelevant.

The model we built pulls the data from an S3 bucket, which acts as a repository for this data. We treated this as if this is were a location in which the file would have been posted. The model then creates a few new columns of data based on the time of the transaction and the amount of time passed between the last transaction on the same account. It also performs bucketing based on region and adds this data to a column. Finally, we begin filtering out unnecessary features (‘state’, ’merch_lat’, ‘merch_long’, ‘lat’, ‘long’, ‘unix_time’, ‘cc_num’, ‘unnamed: 0’, ‘trans_date_trans_time’, ‘merchant’, ‘dob’, ‘first’, ‘last’, ‘street’, ‘trans_num’, ‘city’, ‘job’). The data is then split, scaled, and trained.

Models tested were SMOTE, Random Under sampling, Random Oversampling, & Random Forest. These were tested first with the original data only filtered, then with the new features and filting, and lastly a mix of the two for best results.

The final results were as follows:

Accuracy was 100% (this is due to the skewed amount of valid transactions)
Precision was near 100% for both valid and fraud transactions
This is significant as it shows that the model is correctly detecting fraudulent transactions 98% of the time
This also sets the lowest threshold for requiring an employee to call a customer to confirm a transaction was fraudulent
Recall/Sensitivity was 100% for valid transactions
This was only 76% for fraud and is where the most opportunity for the model remained
This indicates that for every 3 transactions identified as fraud, there was still 1 fraudulent transaction not being identified
While the recall rate of fraud transactions was better in other iterations, they resulted in lower recall rates for valid transactions and the amount of work and costs needed to clear that volume would likely outweigh the potential losses of the missed fraudulent transactions
Model Improvement:

If we had more time, we would work to quantify distance of the transaction from the home market (if this was not a moto transaction). We would attempt to identify the type of transaction (chip, swipe, moto, token) and we would quantify if the spend was out of the norm for the customer. One thing we identified was that much of the fraud in this set were large dollar transactions, while our experience working in fraud has been that most fraudulent transactions and the total fraud on accounts are under $200 per transaction. As such, our model may inaccurately favor detecting larger transactions as fraud, which would not be evident until we were able to factor in more low-dollar fraud transactions.

## Tableau Dashboard Visualization

<img width="204" alt="Total_transaction_amt" src="https://user-images.githubusercontent.com/101374716/185832983-88e10af7-2395-4904-b2c6-242550716ff6.png">

Total amount of fraudulent transactions


Please see description of each visualization below. By clicking on the Tableau Dashboard link you will be able to deep dive into each worksheet as they are interactive.

<img width="270" alt="Cat_Trans_percentages" src="https://user-images.githubusercontent.com/101374716/185827674-b51f75dc-afaf-4405-ba96-b6d411869ba7.png">

Category Transaction Percentages visualization shows the percentage total count of merchant categories. You will be able to click on each on of the dots to see the percentage of each category. You can also filter categories and show fraudulent and non-fraudulent charges. 


<img width="300" alt="Transactions_By_Gender" src="https://user-images.githubusercontent.com/101374716/185827743-f267cd21-0529-4d5c-a880-02232462274c.png">

Transactions by Gender visualization shows the count of transaction via Female or Male. You are able to click each bar to get the count.


<img width="239" alt="Fraud_Gender" src="https://user-images.githubusercontent.com/101374716/185827988-87b430b3-392a-478f-b4c6-1abbf81f3ae9.png">

Fraud by Gender shows fradulent and non-fradulent charges via gender. You can also filter from fradulent to non-fraudlent and female or male. 

<img width="304" alt="Trans_Percent_by_Gender" src="https://user-images.githubusercontent.com/101374716/185828250-b0d9109e-f3f8-4cc1-9e56-b4b91ad058e8.png">

Transaction percent by Gender will visualize the percentage of fraud and non-fraudulent transactions by gender. 


<img width="1088" alt="Fraud_city" src="https://user-images.githubusercontent.com/101374716/185828116-2356272f-43cf-4e0f-83b3-1492b1a8822d.png">

Fraudlent Charges by City visualization shows an interactive map of cities where fraudulent charges happened. Once you hover over each circle it will show City, Zip code, and count of transactions. 


<img width="793" alt="Fraud_Amt_city" src="https://user-images.githubusercontent.com/101374716/185828128-c7587893-6421-4f0b-82a1-076f123bc81d.png">

Fraud amount by city is another interactive map to see the amount of fraudulent charges by city. Hover over the circle to see City, Zipcode, and Total Fradulent amount. 


<img width="1234" alt="Fraud_by_week" src="https://user-images.githubusercontent.com/101374716/185828177-3b419d37-4279-4d55-b632-a967a70bf586.png">

This interactive graph helps you view total amount of fraud vs non-fraud per week. You can click into the map week over week to get a summary or amount of fradulent or non-fraudlent charges for that week. 


<img width="1062" alt="Fraud_by_age" src="https://user-images.githubusercontent.com/101374716/185828421-94466646-443c-408c-9792-31900285f532.png">

Fraud by age visualization shows the age vs the amount of the transaction. You are able to see the age via fraudulent or non-fraudlent charges. You can click on each graph line over a particular age to see the total amount by that age. 









