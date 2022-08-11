# credit_card_fraud

Observations thus far
While there are only 23 columns the following are all listed as objects which would need to be converted to numerical values for machine learning.

Trans_date_trans_time, merchant, category, fist, last, gender, street, city, state, job, dob, trans_num.

I dropped the following columns based on these observations

Merchant – in order to get this object field to less than 10 unique values it would have grouped the bulk of these values into the other category.  The results of the bucketing will skew the results as the transactions are so varied.

First is the first name of the customer and based on experience in fraud the name of the customer has no impact on susceptibility to fraud.

Last is the last name of the customer and based on experience in fraud the name of the customer has no impact on susceptibility to fraud.

Street – This is tied to the sample volume and bucketing will skew the results to be overfitted.

City – This object is too broad for machine learning and bucketing down will result in massive overfitting for the other category.

Job – if this is bucketed to instances with less than 4500 results it still results in over 86 categories and would overfit

Trans_num – this is a unique identifier to the specific transaction for tracking purposes only and would not be influential to the identification of potential fraud.

I updated the trans_date_trans_time field to better get what it was by creating an hour, day, and month category then dropped the aforementioned category

I updated the DOB column to an age column to convert this to an integer column

