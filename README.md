# Credit Card Fraud Trends and Predictions

## Communication Protocols:
- We will meet during class time on Tuesdays/Thursdays, once weekly on either Sunday or Monday evening, and as needed at other times
- Slack will be our main point of communication outside of meetings

## Project Information:
- Selected topic
  - Credit Card Fraud Trends and Predictions
- Reason they selected the topic
  - We selected this topic due to our team's background in the financial services sector, including two teammates with credit card experience
- Description of the source data
  - Our dataset is a simulated credit card transaction dataset, covering the cards of 1000 customers with a pool of 800 merchants. The dataset covers transactions over a   full two-year period. It contains both legitimate and fraudulent transactions
- Questions they hope to answer with the data
  - Are there any trends in the data?
  - Can any of these trends be used to predict and prevent future fraudulent transactions?

Link to dataset: https://www.kaggle.com/datasets/kartik2112/fraud-detection


ML updates
As the purposed of the machine learning is to predict if a transaction is fraud (Binary outcome), we immediately leaned towards logistic regression testing.  The next choice was to go with supervised or unsupervised.  We chose supervised as we already knew the question we wanted answered (is this fraud or not). 

Due to the HUGE amount of data we had at our disposal and the clear imbalance in good vs. fraud transaction we knew that the accuracy of the model would be highly skewed.  As such there was going to be more focus on the Precision and Recall/Sensitivity rates of our models.

As part of the exercise there was a lot of reviewing the columns and identifying what may be impactful in our goals.  As such there was a bit of preprocessing that resulted in several features being deemed irrelevant.

The model built pulls the data from an S3 bucket which is acting as a repository for this data.  We are treating this as if this is a location in which the file would have been posted.  The model then creates a few new columns of data based on the time of the transaction and how long ago was the prior transaction on the same account.  It also performs bucketing based on region and adds this data to a column.  Finally we begin filtering out unnecessary features (‘state’, ’merch_lat’, ‘merch_long’, ‘lat’, ‘long’, ‘unix_time’, ‘cc_num’, ‘unnamed: 0’, ‘trans_date_trans_time’, ‘merchant’, ‘dob’, ‘first’, ‘last’, ‘street’, ‘trans_num’, ‘city’, ‘job’).  The data is then split, scaled, and trained.  

Models tested  were SMOTE, Random Under sampling, Random Oversampling, & Random Forest.

These were tested with the original data only filtered, with the new features and filting, then a mix of the two for best results.

The final result was as follows:

•	Accuracy was 100% (this is due to the skewed amount of valid transactions.

•	Precision was near 100% for both valid and fraud transactions.  

o	This is significant as it shows that when it keys in on fraud its truly fraud 98% of the time.

o	This would also have been the lowest instance of an employee needing to call to confirm fraud incorrectly.

•	Recall/Sensitivity was again 100% for valid transactions

o	This was only 76% for fraud and is where the most opportunity for the model remained.  

o	This indicates that for every 3 transactions identified as fraud there was still 1 getting through.  

•	While the recall rate of fraud transactions was better in other iterations it resulted in lower recall rates for valid transactions and the amount of work and costs needed to clear that volume would likely outweigh the potential losses of the missed fraud transactions.

If we had more time I would work to quantify distance of the transaction from the home market if this was not a moto transaction. I would go back and attempt to identify the type of transaction (chip, swipe, moto, token) and I would quantify if the spend was out of norms for the customer.  One thing identified was that much of the fraud in this set was large dollar while my experience has been that most fraud transactions and total fraud on accounts are sub $200 per instance.  As such our model may inaccurately favor larger transactions as fraud which would not be evident until we were able to factor in more low dollar fraud transactions.

