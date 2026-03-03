# Module 5 Homework: Data Platforms with Bruin

In this homework, we'll use Bruin to build a complete data pipeline, from ingestion to reporting.

## Setup

1. Install Bruin CLI: `curl -LsSf https://getbruin.com/install/cli | sh`
2. Initialize the zoomcamp template: `bruin init zoomcamp my-pipeline`
3. Configure your `.bruin.yml` with a DuckDB connection
4. Follow the tutorial in the [main module README](../../../05-data-platforms/)

After completing the setup, you should have a working NYC taxi data pipeline.

### Question 1. Bruin Pipeline Structure
In a Bruin project, what are the required files/directories?<br>
(a) `bruin.yml` and `assets/`<br>
(b) `.bruin.yml` and `pipeline.yml` (assets can be anywhere)<br>
(c) `.bruin.yml` and `pipeline/` with `pipeline.yml` and `assets/`<br>
(d) `pipeline.yml` and `assets/` only<br>

--> My Answer: option (c)

### Question 2. Materialization Strategies
You're building a pipeline that processes NYC taxi data organized by month based on `pickup_datetime`. Which incremental strategy is best for processing a specific interval period by deleting and inserting data for that time period?<br>
(a) `append` - always add new rows<br>
(b) `replace` - truncate and rebuild entirely<br>
(c) `time_interval` - incremental based on a time column<br>
(d) `view` - create a virtual table only<br>

--> My Answer: option (c)

### Question 3. Pipeline Variables
You have the following variable defined in `pipeline.yml`:

```yaml
variables:
  taxi_types:
    type: array
    items:
      type: string
    default: ["yellow", "green"]
```

How do you override this when running the pipeline to only process yellow taxis?<br>
(a) `bruin run --taxi-types yellow`<br>
(b) `bruin run --var taxi_types=yellow`<br>
(c) `bruin run --var 'taxi_types=["yellow"]'`<br>
(d) `bruin run --set taxi_types=["yellow"]`<br>

--> My Answer: option (c)

### Question 4. Running with Dependencies
You've modified the `ingestion/trips.py` asset and want to run it plus all downstream assets. Which command should you use?<br>
(a) `bruin run ingestion.trips --all`<br>
(b) `bruin run ingestion/trips.py --downstream`<br>
(c) `bruin run pipeline/trips.py --recursive`<br>
(d) `bruin run --select ingestion.trips+`<br>

--> My Answer: option (d)

### Question 5. Quality Checks
You want to ensure the `pickup_datetime` column in your trips table never has NULL values. Which quality check should you add to your asset definition?<br>
(a) `name: unique`<br>
(b) `name: not_null`<br>
(c) `name: positive`<br>
(d) `name: accepted_values, value: [not_null]`<br>

--> My Answer: option (b)

### Question 6. Lineage and Dependencies
After building your pipeline, you want to visualize the dependency graph between assets. Which Bruin command should you use?<br>
(a) `bruin graph`<br>
(b) `bruin dependencies`<br>
(c) `bruin lineage`<br>
(d) `bruin show`<br>

--> My Answer: option (c)

### Question 7. First-Time Run
You're running a Bruin pipeline for the first time on a new DuckDB database. What flag should you use to ensure tables are created from scratch?<br>
(a) `--create`<br>
(b) `--init`<br>
(c) `--full-refresh`<br>
(d) `--truncate`<br>

--> My Answer: option (c)