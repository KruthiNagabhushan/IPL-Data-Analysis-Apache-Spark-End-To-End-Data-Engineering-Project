# IPL Data Analysis | Apache Spark End-To-End Data Engineering Project 


An end-to-end data engineering pipeline to analyze Indian Premier League (IPL) data using PySpark on Databricks. This project ingests raw data from AWS S3, performs transformations/aggregations, and derives actionable insights about matches, players, and team performances. Link to Databricks notebook: https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/3550376818114390/1845185041180616/2624604177551557/latest.html

---

## Project Overview

### Analyze IPL data to uncover patterns such as:
- Top-performing batsmen/bowlers by season
- Team performance trends
- Toss impact on match outcomes
- Player economy rates in powerplay overs
- Stadium performance metrics

### Tech Stack

- Data Processing: Apache Spark (PySpark)
- Cloud Storage: AWS S3
- Notebook Environment: Databricks
- Visualization: Matplotlib, Seaborn

---

## Dataset

Source: [data.world](https://data.world/)  
Storage: AWS S3 bucket (`s3://ipl-data-analysis-project/`)  
Files:
- `Ball_By_Ball.csv`: Ball-level delivery data (runs, extras, wickets, etc.)
- `Match.csv`: Match metadata (venue, teams, toss, winner, etc.)
- `Player.csv`: Player profiles (batting hand, bowling skill, country)
- `Player_match.csv`: Player-match participation details
- `Team.csv`: Team information

---

## Pipeline Architecture

### 1. Data Ingestion
   - Read CSV files from S3 into Spark DataFrames with custom schemas.
   - Example schema for `Ball_By_Ball.csv`:
     ```python
     ball_by_ball_schema = StructType([...])
     ball_by_ball_df = spark.read.schema(ball_by_ball_schema).csv("s3://path")
     ```

### 2. Data Transformations
   - Filter invalid deliveries (wides/no-balls excluded).
   - Calculate metrics like:
     - Total/average runs per innings
     - Running total of runs per over
     - High-impact balls (wickets or 6+ runs)
   - Join datasets (e.g., match details with player stats).

### 3. SQL Analysis 
   Use Spark SQL to answer business questions:
   ```sql
   -- Top 5 batsmen by season
   SELECT player_name, season_year, SUM(runs_scored) AS total_runs
   FROM ball_by_ball
   JOIN match ON ball_by_ball.match_id = match.match_id
   JOIN player ON player.player_id = ball_by_ball.striker
   GROUP BY player_name, season_year
   ORDER BY total_runs DESC
   ```

### 4. Visualizations
Sample insights visualized using Matplotlib/Seaborn:

- Toss vs. match outcome correlation
- Economy rates of bowlers during powerplay overs


