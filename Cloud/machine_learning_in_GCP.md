# GCP: Big data and ML

## Recommending Products using Cloud SQL and Apache Spark

In this lab, you populate rentals data in Cloud SQL for the rentals recommendation engine to use.

In the GCP console, click SQL (in the Storage section). Click Create instance and Choose MySQL.

Find the Connect to this instance box on the page and click on connect using Cloud Shell.

In the terminal, we can create databases/tables with SQL as such

'''
CREATE DATABASE IF NOT EXISTS recommendation_spark;

USE recommendation_spark;

DROP TABLE IF EXISTS Recommendation;
DROP TABLE IF EXISTS Rating;
DROP TABLE IF EXISTS Accommodation;

CREATE TABLE IF NOT EXISTS Accommodation
(
  id varchar(255),
  title varchar(255),
  location varchar(255),
  price int,
  rooms int,
  rating float,
  type varchar(255),
  PRIMARY KEY (ID)
);

CREATE TABLE  IF NOT EXISTS Rating
(
  userId varchar(255),
  accoId varchar(255),
  rating int,
  PRIMARY KEY(accoId, userId),
  FOREIGN KEY (accoId)
    REFERENCES Accommodation(id)
);

CREATE TABLE  IF NOT EXISTS Recommendation
(
  userId varchar(255),
  accoId varchar(255),
  prediction float,
  PRIMARY KEY(userId, accoId),
  FOREIGN KEY (accoId)
    REFERENCES Accommodation(id)
);

SHOW DATABASES;
'''

These tables can be imported into your Cloud SQL for analysis.

### Dataproc

You use Dataproc to train the recommendations machine learning model based on users' previous ratings. You then apply that model to create a list of recommendations for every user in the database

To launch Dataproc and configure it so that each of the machines in the cluster can access Cloud SQL. 


echo "Authorizing Cloud Dataproc to connect with Cloud SQL"
CLUSTER=rentals
CLOUDSQL=rentals
ZONE=us-central1-c
NWORKERS=2

machines="$CLUSTER-m"
for w in `seq 0 $(($NWORKERS - 1))`; do
   machines="$machines $CLUSTER-w-$w"
done

echo "Machines to authorize: $machines in $ZONE ... finding their IP addresses"
ips=""
for machine in $machines; do
    IP_ADDRESS=$(gcloud compute instances describe $machine --zone=$ZONE --format='value(networkInterfaces.accessConfigs[].natIP)' | sed "s/\['//g" | sed "s/'\]//g" )/32
    echo "IP address of $machine is $IP_ADDRESS"
    if [ -z  $ips ]; then
       ips=$IP_ADDRESS
    else
       ips="$ips,$IP_ADDRESS"
    fi
done

echo "Authorizing [$ips] to access cloudsql=$CLOUDSQL"
gcloud sql instances patch $CLOUDSQL --authorized-networks $ips

### Running the model

To create a trained model and apply it to all the users in the system:

Your data science team has created a recommendation model using Apache Spark and written in Python. Let's copy it over into our staging bucket.

'''
Copy over the model code by executing the below in Cloud Shell.
gsutil cp gs://cloud-training/bdml/v2.0/model/train_and_apply.py train_and_apply.py
cloudshell edit train_and_apply.py
'''

In the Dataproc console, click Jobs and then Submit Job. For Job type, select PySpark and for Main python file, specify the location of the Python file you uploaded to your bucket. Your <bucket-name> is likely your Project Id when you can find by clicking on the Project Id dropdown in the top navigation menu.

Note: It will take up to 5 minutes for the job to change from Running to Succeeded. You can continue to the next section on querying the results while the job runs.

If the job Failed, please troubleshoot using the logs and fix the errors. You may need to re-upload the changed Python file to Cloud Storage and clone the failed job to resubmit.

Once the job is completed successfully, you can explore this in the Cloud SQL shell.

'''
SELECT
    r.userid,
    r.accoid,
    r.prediction,
    a.title,
    a.location,
    a.price,
    a.rooms,
    a.rating,
    a.type
FROM Recommendation as r
JOIN Accommodation as a
ON r.accoid = a.id
WHERE r.userid = 10; 
'''

## Predicting Visitor Purchases with a Classification Model with BigQuery ML

BigQuery ML (BigQuery machine learning) is a new feature in BigQuery where data analysts can create, train, evaluate, and predict with machine learning models with minimal coding.

There is a newly available ecommerce dataset that has millions of Google Analytics records for the Google Merchandise Store loaded into BigQuery. In this lab, you will use this data to run some typical queries that businesses would want to know about their customers' purchasing habits.

Now that you have your initial features selected, you are now ready to create your first ML model in BigQuery. There are the two model types to choose from: Forecasting (linear_reg) and Classification (logistic_reg). There are many additional model types used in Machine Learning (like Neural Networks and decision trees) and available using libraries like TensorFlow. At time of writing, BigQuery ML supports the two listed above.

You can train a basic model in Big Query (i.e. SQL) like this:
'''
CREATE OR REPLACE MODEL `ecommerce.classification_model`
OPTIONS
(
model_type='logistic_reg',
labels = ['will_buy_on_return_visit']
)
AS

#standardSQL
SELECT
  * EXCEPT(fullVisitorId)
FROM

  # features
  (SELECT
    fullVisitorId,
    IFNULL(totals.bounces, 0) AS bounces,
    IFNULL(totals.timeOnSite, 0) AS time_on_site
  FROM
    `data-to-insights.ecommerce.web_analytics`
  WHERE
    totals.newVisits = 1
    AND date BETWEEN '20160801' AND '20170430') # train on first 9 months
  JOIN
  (SELECT
    fullvisitorid,
    IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit
  FROM
      `data-to-insights.ecommerce.web_analytics`
  GROUP BY fullvisitorid)
  USING (fullVisitorId)
;
'''

Now that training is complete, you can evaluate how well the model performs with this query using ML.EVALUATE.

'''
SELECT
  roc_auc,
  CASE
    WHEN roc_auc > .9 THEN 'good'
    WHEN roc_auc > .8 THEN 'fair'
    WHEN roc_auc > .7 THEN 'not great'
  ELSE 'poor' END AS model_quality
FROM
  ML.EVALUATE(MODEL ecommerce.classification_model,  (

SELECT
  * EXCEPT(fullVisitorId)
FROM

  # features
  (SELECT
    fullVisitorId,
    IFNULL(totals.bounces, 0) AS bounces,
    IFNULL(totals.timeOnSite, 0) AS time_on_site
  FROM
    `data-to-insights.ecommerce.web_analytics`
  WHERE
    totals.newVisits = 1
    AND date BETWEEN '20170501' AND '20170630') # eval on 2 months
  JOIN
  (SELECT
    fullvisitorid,
    IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit
  FROM
      `data-to-insights.ecommerce.web_analytics`
  GROUP BY fullvisitorid)
  USING (fullVisitorId)

));
'''

#Creating a Streaming Data Pipeline for a Real-Time Dashboard with Cloud Dataflow

## Create a Cloud Pub/Sub Topic

Cloud Pub/Sub is an asynchronous global messaging service. By decoupling senders and receivers, it allows for secure and highly available communication between independently written applications. Cloud Pub/Sub delivers low-latency, durable messaging.

In Cloud Pub/Sub, publisher applications and subscriber applications connect with one another through the use of a shared string called a topic. A publisher application creates and sends messages to a topic. Subscriber applications create a subscription to a topic to receive messages from it.

Google maintains a few public Pub/Sub streaming data topics for labs like this one. We'll be using the NYC Taxi & Limousine Commission’s open dataset

Cloud Dataflow is a serverless way to carry out data analysis. In this lab, you will set up a streaming data pipeline to read sensor data from Pub/Sub, compute the maximum temperature within a time window, and write this out to BigQuery. In Dataflow there are multiple options to CREATE JOB FROM TEMPLATE. Once the feed is set up, you can analyse it in BigQuery.

## Create a Real-Time Dashboard

Explore Data > Explore with Data Studio in BigQuery page. This will allow you to create live dashboards.

# Classifying Images of Clouds in the Cloud with AutoML Vision

upload images to Cloud Storage and use them to train a custom model to recognize different types of clouds (cumulus, cumulonimbus, etc.).

AutoML Vision provides an interface for all the steps in training an image classification model and generating predictions on it. Start by enabling the Cloud AutoML API.

In order to train a model to classify images of clouds, you need to provide labeled training data so the model can develop an understanding of the image features associated with different types of clouds. In this example your model will learn to classify three different types of clouds: cirrus, cumulus, and cumulonimbus. To use AutoML Vision you need to put your training images in Google Cloud Storage.

AutoML Vision contains the following options:
- Singe-Label Classification
- Multi-Label Classification
- Object detection
