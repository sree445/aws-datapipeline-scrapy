# aws-datapipeline-scrapy

### Technologies used

-   **Apache Airflow**: Apache Airflow is an open-source platform for authoring, scheduling and monitoring data and computing workflows. For this project it was used to manage data extraction and transformation process
-   
Introducing a Data Pipeline Project that integrates Airflow for Data Orchestration

Technologies used
Apache Airflow: Apache Airflow is an open-source platform for authoring, scheduling and monitoring data and computing workflows. For this project it was used to manage data extraction and transformation process

Snowflake: This is a cloud-based data warehousing platform for data storage and analytics purpose. For this project it was used to query datasets loaded automatically from S3

Snowpipe: Snowpipe is a serverless data ingestion service offered by Snowflake, designed to simplify the process of loading data into Snowflake data warehouses. For this project it was used to automatically load data into snowflake table from stage environment.

AWS EC2 : This is a cloud-based computing service that allows users to quickly launch virtual servers and manage cookies, security, and networking from an easy-to-use dashboard. It was used to host Airflow server for this project.

AWS S3: This is a highly scalable object storage service that stores data as objects within buckets. It is commonly used to store and distribute large media files, data backups and static website files. For this project it is used to store data scraped from target website.

AWS SQS: This is a message queuing service. It exchanges and stores messages between software components. The service adds the messages in a queue. Users or services pick up the messages from the queue. Once processed the messages gets deleted from the queue. In this project it was used to receive notifications from S3 to an SQS queue to be read by the Snowflake server.

Architecture

![flow](https://github.com/sree445/aws-datapipeline-scrapy/assets/32360977/fa8bb842-7c5a-42c3-bc3c-7748f9c23d2d)
