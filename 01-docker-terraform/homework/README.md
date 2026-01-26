# Module 1 Homework: Docker & SQL
In this homework we'll prepare the environment and practice Docker and SQL

When submitting your homework, you will also need to include a link to your GitHub repository or other public code-hosting site.

This repository should contain the code for solving the homework.

When your solution has SQL or shell commands and not code (e.g. python files) file format, include them directly in the README file of your repository.

## Question 1. Understanding Docker images
Run docker with the python:3.13 image. Use an entrypoint bash to interact with the container.

What's the version of pip in the image?

(1) 25.3
(2) 24.3.1
(3) 24.2.1
(4) 23.3.1

--> My Answer: option number 1
```Bash
# confirm docker version:
docker --version

# create the docker image:
docker run -it --rm --entrypoint=bash python:3.13

# update packages:
apt update

# confirm python version:
python --version

# check pip version:
pip --version
```

## Question 2. Understanding Docker networking and docker-compose
Given the following docker-compose.yaml, what is the hostname and port that pgadmin should use to connect to the postgres database?

```yaml
services:
  db:
    container_name: postgres
    image: postgres:17-alpine
    environment:
      POSTGRES_USER: 'postgres'
      POSTGRES_PASSWORD: 'postgres'
      POSTGRES_DB: 'ny_taxi'
    ports:
      - '5433:5432'
    volumes:
      - vol-pgdata:/var/lib/postgresql/data

  pgadmin:
    container_name: pgadmin
    image: dpage/pgadmin4:latest
    environment:
      PGADMIN_DEFAULT_EMAIL: "pgadmin@pgadmin.com"
      PGADMIN_DEFAULT_PASSWORD: "pgadmin"
    ports:
      - "8080:80"
    volumes:
      - vol-pgadmin_data:/var/lib/pgadmin

volumes:
  vol-pgdata:
    name: vol-pgdata
  vol-pgadmin_data:
    name: vol-pgadmin_data
```
(1) postgres:5433
(2) localhost:5432
(3) db:5433
(4) postgres:5432
(5) db:5432
If there are more than one answers, select only one of them

--> My Answer: option number 4
<br>
| Service | URL | Credentials | Container Port |
|---------|-----|-------------|----------------|
| pgAdmin | http://localhost:8080 | pgadmin@pgadmin.com / pgadmin | 80 |
| PostgreSQL | localhost:5433 | postgres / postgres | 5432 |
| Jupyter Notebook | http://localhost:8888 | - | 8888 |
<br>

Prepare Postgres
Download the green taxi trips data for November 2025:
```bash
wget https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2025-11.parquet
```

You will also need the dataset with zones:

```bash
wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/misc/taxi_zone_lookup.csv
```
Download this data and put it into Database.

## Question 3. Counting short trips
For the trips in November 2025 (lpep_pickup_datetime between '2025-11-01' and '2025-12-01', exclusive of the upper bound), how many trips had a trip_distance of less than or equal to 1 mile?
Answers:

(1) 7,853
(2) 8,007
(3) 8,254
(4) 8,421

--> My Answers: option number 2
```sql
SELECT COUNT(*) as trip_count
FROM green_tripdata_2025
WHERE lpep_pickup_datetime >= '2025-11-01'
   AND lpep_pickup_datetime < '2025-12-01'
   AND trip_distance <= 1;
```

## Question 4. Longest trip for each day
Which was the pick up day with the longest trip distance? Only consider trips with trip_distance less than 100 miles (to exclude data errors).

Use the pick up time for your calculations.
(1) 2025-11-14
(2) 2025-11-20
(3) 2025-11-23
(4) 2025-11-25

--> My Answers: option number 1
```sql
SELECT trip_distance, lpep_pickup_datetime
FROM green_tripdata_2025
WHERE trip_distance <= 100
ORDER BY trip_distance DESC;
```

## Question 5. Biggest pickup zone
Which was the pickup zone with the largest total_amount (sum of all trips) on November 18th, 2025?
(1) East Harlem North
(2) East Harlem South
(3) Morningside Heights
(4) Forest Hills

--> My Answers: option number 1
<> Add External table

```sql
CREATE TABLE taxi_zone_lookup AS SELECT * FROM read_csv_auto('taxi_zone_lookup.csv');
```

<> Query result
```sql
SELECT 
    nt."PULocationID", 
    sum(gta.fare_amount) as total_amount, 
    tzl."LocationID", 
    tzl."Zone"
FROM green_tripdata_2025 gta
LEFT JOIN taxi_zone_lookup tzl
    ON gta."PULocationID" = tzl."LocationID"
WHERE lpep_pickup_datetime >= '2025-11-18'
    AND lpep_pickup_datetime <  '2025-11-19'
GROUP BY 
    gta."PULocationID", 
    tzl."LocationID",
    tzl."Zone" 
ORDER BY total_amount DESC;
```

## Question 6. Largest tip
For the passengers picked up in the zone named "East Harlem North" in November 2025, which was the drop off zone that had the largest tip?

Note: it's tip , not trip. We need the name of the zone, not the ID.
(1) JFK Airport
(2) Yorkville West
(3) East Harlem North
(4) LaGuardia Airport

--> My Answers: option number 2
```sql
SELECT 
    zns."Zone" AS dropoff_zone, 
    max(gta.tip_amount),
    zns."Zone"
FROM green_tripdata_2025 gta
JOIN zones zns 
ON gta."PULocationID" = zns."LocationID"
WHERE gta.lpep_pickup_datetime >= '2025-11-01'
AND gta.lpep_pickup_datetime <  '2025-12-01'
GROUP BY 
    dropoff_zone, 
    zns."Zone";
```


## Terraform
In this section homework we'll prepare the environment by creating resources in GCP with Terraform.

In your VM on GCP/Laptop/GitHub Codespace install Terraform. Copy the files from the course repo here to your VM/Laptop/GitHub Codespace.

Modify the files as necessary to create a GCP Bucket and Big Query Dataset.

## Question 7. Terraform Workflow
Which of the following sequences, respectively, describes the workflow for:

Downloading the provider plugins and setting up backend,
Generating proposed changes and auto-executing the plan
Remove all resources managed by terraform`

Answers:
(1) terraform import, terraform apply -y, terraform destroy
(2) teraform init, terraform plan -auto-apply, terraform rm
(3) terraform init, terraform run -auto-approve, terraform destroy
(4) terraform init, terraform apply -auto-approve, terraform destroy
(5) terraform import, terraform apply -y, terraform rm

## Submitting the solutions

Form for submitting: https://courses.datatalks.club/de-zoomcamp-2025/homework/hw1