# Purpose
Create a Postgres database with a star schema that is optimized for queries on song play analysis.

## Introduction
A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. Currently, they don't have an easy way to query their data, which resides in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

I have modeled and defined fact and dimension tables in a Postgres database and built ETL pipeline using SQL and pandas dataframes in python. I have tested my database and ETL pipeline by running queries given to me by the analytics team from Sparkify to compare my results with their expected results. 

## Files
1. [`test.ipynb`](test.ipynb) checks that the database/tables are created properly with basic queries.
2. [`create_tables.py`](create_tables.py) drops and creates the tables. This should be run each time before you run your ETL scripts in order to reset the tables.
3. [`etl.ipynb`](etl.ipynb) reads and processes a single file from song_data and log_data and loads the data into your tables. It contains a detailed instruction to create the logic for the etl.py script.
4. [`etl.py`](etl.py) scans the log_data and song_data directories for all the json files, processes the data, and loads it into the appropriate tables.
5. [`sql_queries.py`](sql_queries.py) contains all the SQL queries, and is imported into the last three files above.
6. [`Dashboard.ipynb`](Dashboard.ipynb) a few sample analysis queries that can be run to form a sample 'dashboard'


## Schema for Song Play Analysis
I have created a star schema to represent the song and log data and optimize queries on song play analysis.

### Fact Table
1. `songplays` - records from the log_data with page == 'NextSong', which represent song plays
 - songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

### Dimension Tables
1. `users` - users in the app
 - user_id, first_name, last_name, gender, level
2. `songs` - songs in the music database
 - song_id, title, artist_id, year, duration
3. `artists` - artists in the music database
 - artist_id, name, location, latitude, longitude
4. `time` - timestamps of records in songplays broken down into the specific unit
 - `start_time`, hour, day, week, month, year, weekday
 
## Data Quality Checks
Duplicate data will be excluded from tables based on primary key, which can be seen in [`sql_queries.py`](sql_queries.py). If a duplicate user record is encountered, on conflict, their 'level' will be updated to that of the excluded record. Will be profiling data to identify data quality rules and then adding queries that use regular expression matching to create views with cleaned data.

## Dashboard
Contains a few sample queries but since the log files only have one matching song record within the song data, its difficult to write a query to see who is listening to what.

## How to Use:
Run [`create_tables.py`](create_tables.py) to create the database/tables. Then run, [`test.ipynb`](test.ipynb) to make sure the database/tables are created properly. Make sure the data folder is in the same directory as the script file, then, run [`etl.py`](etl.py) to process the all of the json files in the log_data and song_data directories. Use [`test.ipynb`](test.ipynb) to query the resulting data and write queries to analyze. [`etl.ipynb`](etl.ipynb) will give you a better understanding of the etl logic.

## NOTE:
You will not be able to run [`test.ipynb`](test.ipynb), [`etl.ipynb`](etl.ipynb), or [`etl.py`](etl.py) until you have run [`create_tables.py`](create_tables.py) at least once to create the sparkifydb database, which these other files connect to.