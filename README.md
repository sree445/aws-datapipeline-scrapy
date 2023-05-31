# aws-datapipeline-scrapy

Introducing a Data Pipeline Project that integrates Airflow for Data Orchestration

### Technologies used

-   **Apache Airflow**: Apache Airflow is an open-source platform for authoring, scheduling and monitoring data and computing workflows. For this project it was used to manage data extraction and transformation process  
      
    
-   **Snowflake**: This is a cloud-based data warehousing platform for data storage and analytics purpose. For this project it was used to query datasets loaded automatically from S3  
      
    
-   **Snowpipe**: Snowpipe is a serverless data ingestion service offered by Snowflake, designed to simplify the process of loading data into Snowflake data warehouses. For this project it was used to automatically load data into snowflake table from stage environment.  
      
    
-   **AWS EC2**  : This is a cloud-based computing service that allows users to quickly launch virtual servers and manage cookies, security, and networking from an easy-to-use dashboard. It was used to host Airflow server for this project.  
      
    
-   **AWS S3**: This is a highly scalable object storage service that stores data as objects within buckets. It is commonly used to store and distribute large media files, data backups and static website files. For this project it is used to store data scraped from target website.  
      
    
-   **AWS SQS**: This is a message queuing service. It exchanges and stores messages between software components. The service adds the messages in a queue. Users or services pick up the messages from the queue. Once processed the messages gets deleted from the queue. In this project it was used to receive notifications from S3 to an SQS queue to be read by the Snowflake server.      
    

## Architecture

![flow](https://github.com/sree445/aws-datapipeline-scrapy/assets/32360977/fa8bb842-7c5a-42c3-bc3c-7748f9c23d2d)


### Languages used

-   Python  
      
    

### [](https://github.com/priye-1/airflow_data_pipeline/blob/master/readme.md#datasets)Datasets

Data was scraped from  [this website](https://rewardsforjustice.net/index/?jsf=jet-engine:rewards-grid&tax=crime-category:1070%2C1071%2C1073%2C1072%2C1074) using Scrapy. The website does not provide a public API, hence it was reverse engineered to get the endpoint. Postman was then used to analyze endpoint  
  

### [](https://github.com/priye-1/airflow_data_pipeline/blob/master/readme.md#getting-started)Getting Started

1.  Create EC2 Instance (AWS Ubuntu Server) with at least 4gb RAM as requested by airflow installation requirements - preferable t3.medium  
    [![](https://github.com/priye-1/airflow_data_pipeline/raw/master/readme_images/instance.png)](https://github.com/priye-1/airflow_data_pipeline/blob/master/readme_images/instance.png)  
      
    
2.  Install Airflow and other libraries on ec2 Instance. Use commands  [here](https://github.com/priye-1/airflow_data_pipeline/blob/master/ubuntu_commands.sh)  to install dependencies and start airflow server. Clone dags from this repository into your ubuntu directory using git. This will allow you to access the scraper and dag code on your server. Airflow UI can then be accessed using this url format -  `http://<public dns address>:8080`  
    [![](https://github.com/priye-1/airflow_data_pipeline/raw/master/readme_images/airflow.png)](https://github.com/priye-1/airflow_data_pipeline/blob/master/readme_images/airflow.png)  
    
3.  Create a snowflake account  [here](https://signup.snowflake.com/)  , also create database, staging environment for incoming files on snowflake, table and pipe to copy data from stage to snowflakes table. Data pipelines can leverage Snowpipe to continously load micro-batches of data into tables for automated tasks. Follow commands  [here](https://github.com/priye-1/airflow_data_pipeline/blob/master/snowflakes_queries.sql)  to create database, tables, etc.  
      
    
4.  Create S3 Bucket and IAM role to enable access to s3 from any instance, take note of the AWS key and secret key  
    [![](https://github.com/priye-1/airflow_data_pipeline/raw/master/readme_images/s3.png)](https://github.com/priye-1/airflow_data_pipeline/blob/master/readme_images/s3.png)  
      
    
5.  Set up AWS SQS for all bucket create operations  
    To enable the bucket to notify Snowpipe when new data arrives. This can be done by executing the query “show pipes;” in snowflakes worksheet, copying the notification_channel(ARN) value from the newly created pipe, and pasting it into the AWS SQS.  [![](https://github.com/priye-1/airflow_data_pipeline/raw/master/readme_images/event.png)](https://github.com/priye-1/airflow_data_pipeline/blob/master/readme_images/event.png)  
      
    
6.  Trigger dag manually from the UI and access Snowflake worksheet to preview data, with time the number of rows increases if airflow task is set to run on a schedule.  
    

[![](https://github.com/priye-1/airflow_data_pipeline/raw/master/readme_images/pipeline.png)](https://github.com/priye-1/airflow_data_pipeline/blob/master/readme_images/pipeline.png)

  
![Screenshot 2023-05-31 154420](https://github.com/sree445/aws-datapipeline-scrapy/assets/32360977/d5ba0513-18e0-4666-b0ca-9b4565542bd5)
 

#### [](https://github.com/priye-1/airflow_data_pipeline/blob/master/readme.md#pipeline-flow)Pipeline Flow

start Airflow server-> Trigger data crawler Dag -> Crawler starts -> loads data into s3 -> snowpipe loads data into table from s3  
  

#### [](https://github.com/priye-1/airflow_data_pipeline/blob/master/readme.md#necessary-files)Necessary Files

1.  Ubuntu commands for Airflow setup can be found  [here](https://github.com/priye-1/airflow_data_pipeline/blob/master/ubuntu_commands.sh)
2.  Airflow and scrapy code can be found  [here](https://github.com/priye-1/airflow_data_pipeline/tree/master/dags)  This should run in your environment if set up correctly
3.  Snowflake queries can be found  [here](https://github.com/priye-1/airflow_data_pipeline/blob/master/snowflakes_queries.sql)

### [](https://github.com/priye-1/airflow_data_pipeline/blob/master/readme.md#references)References

-   [Airflow S3_hook Source code](https://airflow.apache.org/docs/apache-airflow/1.10.4/_modules/airflow/hooks/S3_hook.html#S3Hook.check_for_key)
-   [Creating connection id in Airflow](https://towardsdatascience.com/apache-airflow-for-data-science-how-to-upload-files-to-amazon-s3-5bdf6fcb1cea)
-   [Setting up SnowSQL](https://docs.snowflake.com/en/user-guide/getting-started-tutorial-log-in)
-   [Automating Snowpipe for amazon S3](https://docs.snowflake.com/en/user-guide/data-load-snowpipe-auto-s3#step-2-create-the-iam-role-in-aws)
-   [How to automate SnowPipe To Load Data From AWS S3 To Snowflake](https://medium.com/snowflake/how-to-automate-snowpipe-to-load-data-from-aws-s3-to-snowflake-1df7fcfc7a85)
-   [Setting up a data pipeline using Snowflake’s Snowpipes in ‘10 Easy Steps](https://calogica.com/sql/snowflake/2019/04/04/snowpipes.html#8-create-the-snowpipe)
-   [Pass data between Airflow tasks](https://docs.astronomer.io/learn/airflow-passing-data-between-tasks?tab=taskflow#example-dag-using-xcoms)
-   [Scrapy To AWS S3](https://scrapeops.io/python-scrapy-playbook/scrapy-save-aws-s3/)
-   [Run Scrapy from a Script](https://docs.scrapy.org/en/latest/topics/practices.html#run-scrapy-from-a-script)
-   [Snowflakes Copy History](https://docs.snowflake.com/en/sql-reference/functions/copy_history)
