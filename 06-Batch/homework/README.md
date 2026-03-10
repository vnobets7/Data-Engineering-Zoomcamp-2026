# Module 6 Homework

In this homework we'll put what we learned about Spark in practice.

For this homework we will be using the Yellow 2025-11 data from the official website:

```bash
wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2025-11.parquet
```


## Question 1: Install Spark and PySpark
- Install Spark
- Run PySpark
- Create a local spark session
- Execute spark.version.

What's the output?

> [!NOTE]
> To install PySpark follow this [guide](https://github.com/DataTalksClub/data-engineering-zoomcamp/blob/main/06-batch/setup/)
<br>
--> My Answer: spark version: 4.1.1

## Question 2: Yellow November 2025
Read the November 2025 Yellow into a Spark Dataframe.

Repartition the Dataframe to 4 partitions and save it to parquet.

What is the average size of the Parquet (ending with .parquet extension) Files that were created (in MB)? Select the answer which most closely matches.<br>
(a) 6MB<br>
(b) 25MB<br>
(c) 75MB<br>
(d) 100MB<br>

--> My Answer: option b

## Question 3: Count records
How many taxi trips were there on the 15th of November?
Consider only trips that started on the 15th of November.<br>
(a) 62,610<br>
(b) 102,340<br>
(c) 162,604<br>
(d) 225,768<br>

--> My Answer: option c

## Question 4: Longest trip
What is the length of the longest trip in the dataset in hours<br>
(a) 22.7<br>
(b) 58.2<br>
(c) 90.6<br>
(d) 134.5<br>

--> My Answer: option c

## Question 5: User Interface
Spark's User Interface which shows the application's dashboard runs on which local port?<br>
(a) 80<br>
(b) 443<br>
(c) 4040<br>
(d) 8080<br>

--> My Answer: option c

## Question 6: Least frequent pickup location zone
Load the zone lookup data into a temp view in Spark:

```bash
wget https://d37ci6vzurychx.cloudfront.net/misc/taxi_zone_lookup.csv
```

Using the zone lookup data and the Yellow November 2025 data, what is the name of the LEAST frequent pickup location Zone?<br>
(a) Governor's Island/Ellis Island/Liberty Island<br>
(b) Arden Heights<br>
(c) Rikers Island<br>
(d) Jamaica Bay<br>

--> My Answer: option a<br>

If multiple answers are correct, select any