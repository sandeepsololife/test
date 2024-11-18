# Setup Instructions for Raisin Interview DAG

This guide will help you set up the required environment to execute the raisin_interview_task DAG.

# Prerequisites

## Software Requirements
Docker Desktop: Ensure Docker Desktop is installed and running on your system.
Docker: Ensure Docker is installed and running on your system.
Docker Compose: Required for setting up Airflow and supporting services.
Python: Python 3.7 or above.
Airflow: Installed and configured via docker-compose.yml file.
PostgreSQL: Installed and configured via docker-compose.yml file.
MinIO: A local or remote MinIO server for S3-compatible storage. Installed and configured via docker-compose.yml file.
Python Packages: Installed and configured via docker-compose.yml file on airflow-init.

## Environment Details
* PostgreSQL:
    Host: postgres
    Port: 5432
    Database: demo
    User: airflow
    Password: airflow

Use PSQL or tools like DBeaver for setting up and querying data.

### MinIO:
    Endpoint: http://minio-server:9000
    Access Key: minioadmin
    Secret Key: minioadmin
    Bucket: interview-demo

A local or remote MinIO server for S3-compatible storage.
On local, Please migrate to `http://minio-server:9000` and login with
credentials provided above `minioadmin`.

### Airflow-Webserver:
    Endpoint: http://localhost:8080/login/
    Username: airflow
    Password: airflow
    Raisin Interview Dag: raisin_interview_task

The above two services along with Airflow services starts after the 
docker-compose created images and containers successfully. 

### Additional Files

Ensure the following .sql files are available in the /opt/airflow/dags/ directory:

* distinct_country_names.sql
* highest_total_balance.sql
* monthly_country_balances.sql

These files are mounted to dags folder in Airflow along with `.csv`resource files provided for data. This has been done for simplicity purposes.



## Steps to Setup the Environment

* Open your terminal and move into project directory.

```cd sandeep-kanumuri-data-challenge```

* Build airflow-init with below code:

```docker-compose up airflow-init```

This sets up Airflow backend, Postgres and MINIO along with setting up tables in Postgres and dumps provided `.csv` data in to them. Also creates other empty tables to dump data from Airflow later when the dags are run based on the queries defined in files `highest_total_balance.sql` and `monthly_country_balances.sql` for answering business questions asked in the challenge.

* Once the above command is successful then run the below command:

```docker-compose up -d```

This will setup all the Airflow services.
Now, you should be able to see all the services running as containers in your docker desktop. (Note: After this step don't mind if airflow-init shows with status Exited.)


### Now you have finished all the setup. To run the Dag, go to the Airflow-Webserver endpoint `http://localhost:8080/login/` as mentioned in the Environment Details section and login with `Username` and `Password` provided there. Then you should see a list of DAG's in the Airflow UI. 

#### Then search in the list for the Dag: 
```raisin_interview_task```

* Open it and turn-on the Dag and Run it Or Just Trigger it manually if want to run quickly.

* Once all the tasks are done executing, they show success with Green color.

* Now login to MinIO to check for the `.csv` files uploaded with data in the created bucket. There should be 5 files. One for each country with account balances calculated with currency conversions from `GBP` to `EUR` (based on dates the transactions happened) on Month-End basis based on pay-in and pay-out in the provided files data.

* Now login to Postgres with a tool of your comfort and You can also check all the raw data provided tables, forex table and 5 files with month-end account balances one for each country and Highest account balance having country table showing the single row with highest account balance and country calculated in EUR after forex conversions.

## If any support needed or facing any issues with your setup then Don't hesitate to reach me out.



