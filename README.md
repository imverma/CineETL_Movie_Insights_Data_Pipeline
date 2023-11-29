# CineETL: Movie Insights Data Pipeline

## Overview:
CineETL is a robust data pipeline designed to extract, transform, and load movie-related data, providing comprehensive insights into the film industry's dynamics. This solution is tailored for stakeholders seeking data-driven decisions in the cinematic domain.

## Objective:
- Analyze and identify top-rated movies based on historical data.
- Categorize and quantify audience preferences by genre.
- Examine seasonal trends affecting box office revenues.
- Provide inflation-adjusted financial insights for genre-based earnings.

## Data Sources:
- **Primary Dataset**: [Movielens dataset](https://www.kaggle.com/rounakbanik/the-movies-dataset) from Kaggle, encompassing 26 million user ratings across 45,000 films.
- **Supplementary Data**: [Consumer Price Index](https://fred.stlouisfed.org/series/CUSR0000SS62031) from Fred St. Louis to adjust financial figures for inflation.

## Technical Architecture:
- **Extraction**: Data is sourced using the Kaggle API and direct fetch requests for the St Louis Fred's CPI dataset.
- **Transformation**: Apache Spark is employed for its distributed data processing capabilities, ensuring efficient handling of large datasets.
- **Loading**: Data is ingested into AWS Redshift, a petabyte-scale data warehouse solution.
- **Orchestration**: Apache Airflow manages the ETL workflow, ensuring data integrity and timely execution.

## Rationale for Technology Selection:
- **Apache Spark**: Chosen for its distributed processing capabilities, ensuring scalability and performance.
- **Apache Airflow**: Offers dynamic ETL management, dependency resolution, and error handling.
- **AWS Redshift**: Provides a scalable cloud Data Warehouse solution with seamless integration capabilities.
- **Docker**: Ensures environment consistency, reducing discrepancies between development and production setups.

## Data Modeling:
Data normalization is prioritized to optimize storage, ensure data integrity, and facilitate efficient CRUD operations.

## Assumptions and Considerations:
Based on the current design, it's been assumed that the data volume won't see a significant surge and the pipeline is designed to execute just once. However, let's explore some potential scenarios:

### Scenario: Data volume surges by 100x
- **Solution**: We can bolster the Spark cluster by adding more worker nodes, enhancing computational performance. Additionally, Airflow's scheduling capabilities can be leveraged to fetch segmented data, thereby managing the data volume more efficiently.

### Scenario: Pipeline execution required by 7am daily
- **Solution**: The EC2 instance can be activated to initiate the pipeline before the 7am mark. While the current Airflow pipeline is configured for a one-time run, it can be reconfigured for daily data ingestion. It would be prudent to integrate an additional node in our Airflow DAG for data retrieval via API or direct requests, followed by data transfer to S3. For managing intensive tasks during backfilling, the CeleryExecutor can be employed to distribute processes, ensuring resilience. Moreover, Airflow's SLA capabilities can be harnessed to trigger alerts if the pipeline hasn't completed by a specified time, such as 6:00am.

### Scenario: Database accessed by over 100 users
- **Solution**: While Redshift is adept at managing multiple users, it's essential to be proactive with scaling strategies, either by adding more nodes or scaling existing ones. To expedite query responses, understanding frequent user queries can help refine the data model. Pre-aggregated data tables can be introduced to cut down on query durations. Furthermore, assigning sort keys based on user query patterns can further optimize data retrieval.


## Future Considerations:
- **Scalability**: Infrastructure is designed to accommodate data volume increases, with Spark and Redshift's scalability features at the forefront.
- **Automation**: The Airflow scheduler can be adjusted for more frequent data updates, ensuring real-time insights.
- **Concurrency**: Redshift's architecture supports multiple concurrent user queries, ensuring system responsiveness during peak loads.

## Deployment and Configuration:
A comprehensive guide is provided for stakeholders, detailing the deployment process from Docker containerization to Airflow UI configuration, ensuring a seamless setup and execution experience.
