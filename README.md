# Twitter Sentiment Analysis Using Hadoop and Hive

## Project Overview

This project demonstrates the use of Hadoop and Hive to perform sentiment analysis on a Twitter dataset. The process involves setting up a Hadoop cluster, managing metadata using Hive, uploading a CSV dataset to the Hadoop Distributed File System (HDFS), and querying the data for insights. This project showcases the integration of big data technologies for processing and analyzing large datasets.


## What is Hadoop?

Hadoop is an open-source framework designed for distributed storage and processing of large datasets across clusters of computers using simple programming models. It provides the following components:

- **HDFS (Hadoop Distributed File System):** A distributed file storage system that splits large files into smaller blocks and stores them across a cluster of machines.
- **MapReduce:** A programming model for processing large datasets in parallel.
- **YARN (Yet Another Resource Negotiator):** Manages resources and scheduling tasks across the Hadoop cluster.
- **Common Utilities:** Provide the libraries and utilities required by other Hadoop modules.


## What is Hive?

Hive is a data warehousing tool built on top of Hadoop that enables querying and managing large datasets using a SQL-like language called HiveQL. It simplifies the process of querying structured and semi-structured data stored in HDFS. Hive is highly suitable for data analysis tasks, as it translates SQL queries into MapReduce jobs, ensuring scalability and performance.

### Key Features of Hive:
- Supports querying with SQL-like syntax (HiveQL).
- Manages metadata using a relational database like Derby, MySQL, or PostgreSQL.
- Enables integration with HDFS for storing and processing large-scale datasets.


## Steps to Perform Sentiment Analysis

### 1. Dataset Preparation
Ensure the dataset is a CSV file containing tweets and their associated metadata. The file should be preprocessed for any unnecessary data or inconsistencies.


### 2. Start Hadoop Services
Start all Hadoop services using the following command:
```bash
Start-all.cmd
```
### 3. Start Derby Server/Network
Start the Derby server to manage Hive's metadata in `C:\derby\bin`:
```bash
StartNetworkServer -h 0.0.0.0
```
### 4. Initialize Hive Schema
Navigate to the Hive installation directory (e.g.,
`C:\hive\bin` ) and initialize the Hive schema:
```bash
hive --service schematool -dbType derby -initSchema
```
### 5. Creating a Directory in HDFS
Create the directory in HDFS to store the CSV file:
```bash
hdfs dfs -mkdir -p /user/hive/warehouse/twitter
```

### 6. Load Dataset into HDFS
Use the following command to upload the dataset into HDFS:
```bash
hdfs dfs -put C:/Users/Saisamarth/Desktop/twitter_data.csv /user/hive/warehouse/twitter/
```
### 7. Start Hive
Using this command in `C:\hive\bin`:
```bash
hive
```

### 8. Create and Load Hive Table
Create a Hive table to store the data:
```bash
CREATE TABLE twitter_data (
    tweet_id STRING,
    user_id STRING,
    tweet_text STRING,
    sentiment STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE;
```
Load data from HDFS into the Hive table:
```bash
LOAD DATA INPATH '/user/hive/warehouse/twitter/twitter_data.csv' INTO TABLE twitter_data;
```
### 9. Sentiment Analysis Queries
Count Sentiments
```bash
SELECT sentiment, COUNT(*) AS sentiment_count
FROM twitter_data
GROUP BY sentiment;
```
Extract Positive Tweets
```bash
SELECT tweet_id, tweet_text
FROM twitter_data
WHERE sentiment = 'positive';
```

Extract Negative Tweets
```bash
SELECT tweet_id, tweet_text
FROM twitter_data
WHERE sentiment = 'negative';
```

Identify Users with the Most Tweets
```bash
SELECT user_id, COUNT(*) AS tweet_count
FROM twitter_data
GROUP BY user_id
ORDER BY tweet_count DESC
LIMIT 10;
```



















