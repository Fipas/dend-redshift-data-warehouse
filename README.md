# Project 3 - Data Warehouse

This project was made as part of the Udacity Data Engineering Nanodegree. \
Its goal is to apply concepts of data warehouses with AWS and build an ETL pipeline for a database hosted on Redshift.

To complete it, it was necessary to load data from S3 to staging tables on Redshift and execute SQL statements that create the analytics tables from these staging tables.

# Introduction

A music streaming startup, Sparkify, has grown their user base and song database and want to move their processes and data onto the cloud. Their data resides in S3, in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

They'd like a data engineer to build an ETL pipeline that extracts their data from S3, stages them in Redshift, and transforms data into a set of dimensional tables for their analytics team to continue finding insights in what songs their users are listening to

# Files in repository

## Dataset

Two datasets were used for this, one for songs and one for log data.

### Song Dataset

The first dataset is a subset of real data from the [Million Song Dataset](https://labrosa.ee.columbia.edu/millionsong/). Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. For example, here are filepaths to two files in this dataset.

```
song_data/A/B/C/TRABCEI128F424C983.json
song_data/A/A/B/TRAABJL12903CDCF1A.json
```

And below is an example of what a single song file, `TRAABJL12903CDCF1A.json`, looks like.

```
{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}
```

### Log Dataset

The second dataset consists of log files in JSON format generated by this [event simulator](https://github.com/Interana/eventsim) based on the songs in the dataset above. These simulate activity logs from a music streaming app based on specified configurations.

The log files in the dataset are partitioned by year and month. For example, here are filepaths to two files in this dataset.

```
log_data/2018/11/2018-11-12-events.json
log_data/2018/11/2018-11-13-events.json

```

Below is an example of what the data in a log file looks like

![logfile_data_example](https://video.udacity-data.com/topher/2019/February/5c6c3ce5_log-data/log-data.png)


## Database management files and ETL process

The database schema is contained within the file `create_tables.py`, and the the queries used during the ETL process are in `sql_queries.py`. The ER diagram below illustrates the database schema.

![database schema](https://udacity-reviews-uploads.s3.us-west-2.amazonaws.com/_attachments/339318/1586016120/Song_ERD.png)


The ETL process is contained within `etl.py`. It extracts data from the song dataset and log dataset contained within S3 buckets into staging tables in Redshift, and use them to fill `songs`, `artists`, `users`, `time` and `songplays` tables. 

# How to run the scripts

To be able to run the ETL pipeline, Python 3 is required. \
For more information about how to install Python, please visit https://www.python.org/about/gettingstarted/ \
It is also needed to correctly create a Redshift cluters and IAM_ROLE with read access to S3 and put their information at `dwh.cfg`\
psycopg2 is also used. It can be installed using `pip`.

## Usage

First, create your Redshift cluster and IAM_ROLE with read access to S3 and put their information at `dwh.cfg`\
Don't forget to allow inbound TCP traffic at port 5439 for your cluster. 

To create the tables at the database, from the root directory execute
```
python create_tables.py
```

This will create two staging tables and the dimension and facts tables.

Once the database is created, the ETL pipeline can be run. To do so, execute from the root directory

 ```
python etl.py
 ```
This will copy data from two S3 buckets into staging tables and fill dimension and fact tables appropriately. 


# License

This project is licensed under the MIT License. For more information about it, please visit https://opensource.org/licenses/MIT