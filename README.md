# Credit Card Fraud: Sample Postgres Connection Procedure

Gather User ID/Password

Database: Credit_Card_Fraud

Tables:
model_upload_s
personal_info_s

Steps:

1.	Anaconda: pip install psycopg2
2.	Set up build:
python setup.py build
sudo python setup.py install


3.	Coding:

    conn = psycopg2.connect(
        host=”localhost”,
        database-“Credit_Card_Fraud”,
        user=”postgres”
        password=”ABCD1234”)    (Password set up for Postgres)

4.	Add database.ini to the .gitignore file

5.	Config() function read the database.ini file

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


PostgreSQL Python: Querying Data: Retrieving Data from Tables


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






















