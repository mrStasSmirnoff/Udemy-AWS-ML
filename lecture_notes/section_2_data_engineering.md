## S3

### S3 Standart - General Purpose
 * 99.99% Availability
 * Used for frequently accessed data
 * Low latency and high throughput
 * Sustain 2 concurrent facility failures
 * Use Cases: Big Data analytics, mobile & gaming applications, content distribution...

### S3 Storage Classes – Infrequent Access
 * For data that is less frequently accessed, but requires rapid access when needed
 * Lower cost than S3 Standard

 * **Amazon S3 Standard-Infrequent Access (S3 Standard-IA)**
   * 99.9% Availability
   * Use cases: Disaster Recovery, backups
 
 * **Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA)**
   * High durability (99.999999999%) in a single AZ(availability zone); data lost when AZ is destroyed
   * 99.5% Availability
   * Use Cases: Storing secondary backup copies of on-premise data, or data you can recreate

### Amazon S3 Glacier Storage Classes
 * Low-cost object storage meant for archiving / backup
 * Pricing: price for storage + object retrieval cost
 * **Amazon S3 Glacier Instant Retrieval**
   * Millisecond retrieval, great for data accessed once a quarter
   * Minimum storage duration of 90 days
 * **Amazon S3 Glacier Flexible Retrieval**
   * Expedited (1 to 5 minutes), Standard (3 to 5 hours), Bulk (5 to 12 hours) – free
   * Minimum storage duration of 90 days
 * **Amazon S3 Glacier Deep Archive – for long term storage**
   * Standard (12 hours), Bulk (48 hours)
   * Minimum storage duration of 180 days

### S3 Intelligent-Tiering
 * Small monthly monitoring and auto-tiering fee
 * Moves objects automatically between Access Tiers based on usage
 * There are no retrieval charges in S3 Intelligent-Tiering

 * *Frequent Access* tier (automatic): default tier
 * *Infrequent Access* tier (automatic): objects not accessed for 30 days
 * *Archive Instant Access* tier (automatic): objects not accessed for 90 days
 * *Archive Access tier* (optional): configurable from 90 days to 700+ days
 * *Deep Archive Access* tier (optional): config. from 180 days to 700+ days


### S3 Encryption for Objects
 * SSE-S3: encrypts S3 objects using keys handled & managed by AWS
 * SSE-KMS: use AWS Key Management Service to manage encryption keys
    * Additional security (user must have access to KMS key) 
    * Audit trail for KMS key usage

### S3 Security
 * User based
   * **IAM policies** - which API calls should be allowed for a specific user
 *  Resource Based
   * **Bucket Policies** - bucket wide rules from the S3 console - allows cross account
   * Object Access Control List (ACL) – finer grain
   * Bucket Access Control List (ACL) – less common
 * Networking - **VPC Endpoint Gateway**
   * Allow traffic to stay within your VPC (instead of going through public web)
   * Make sure your private services (AWS SageMaker) can access S3
 * Logging and Audit:
   * S3 access logs can be stored in other S3 bucket 
   * API calls can be logged in AWS CloudTrail
 * Tagged Based (combined with IAM policies and bucket policies)

## AWS Kinesis Overview

 * **Kinesis** is a managed alternative to Apache Kafka
   * **Kinesis Data Streams**: low latency streaming ingest at scale
   * **Kinesis Analytics**: perform real-time analytics on streams using SQL
   * **Kinesis Firehose**: load streams into S3, Redshift, ElasticSearch & Splunk
   * **Kinesis Video Streams**: meant for streaming video in real-time

 * Streams are divided in ordered Shards / Partitions
 * Multiple applications can consume the same stream
 * Once data is inserted in Kinesis, it can’t be deleted (immutability)
 * Records can be up to 1MB in size

### Kinesis Data Streams – Capacity Modes
You can use Amazon Kinesis Data Streams to collect and process large streams of data records in real time. You can create data-processing applications, known as Kinesis Data Streams applications. A typical Kinesis Data Streams application reads data from a data stream as data records. These applications can use the Kinesis Client Library, and they can run on Amazon EC2 instances. You can send the processed records to dashboards, use them to generate alerts, dynamically change pricing and advertising strategies, or send data to a variety of other AWS services.

 * **Provisioned mode** (when capacity is planned)
 * **On-demand mode** (flexible capacity)
  
 * Kinesis Data Streams Limits to know
   * Producer
     * 1MB/s or 1000 messages/s at write PER SHARD
     * *ProvisionedThroughputException* otherwise
   * Consumer Classic
     * 2MB/s at read PER SHARD across all consumers
     * 5 API calls per second PER SHARD across all consumers
   * Data Retention
     * 24 hours data retention by default
     * Can be extended to 365 days
  
### Kinesis Data Firehose
Amazon Kinesis Data Firehose is a fully managed service for delivering real-time streaming data to destinations such as Amazon Simple Storage Service (Amazon S3), Amazon Redshift, Amazon OpenSearch Service, Splunk, and any custom HTTP endpoint or HTTP endpoints owned by supported third-party service providers, including Datadog, Dynatrace, LogicMonitor, MongoDB, New Relic, and Sumo Logic. Kinesis Data Firehose is part of the Kinesis streaming data platform, along with Kinesis Data Streams, Kinesis Video Streams, and Amazon Kinesis Data Analytics. With Kinesis Data Firehose, you don't need to write applications or manage resources. You configure your data producers to send data to Kinesis Data Firehose, and it automatically delivers the data to the destination that you specified. You can also configure Kinesis Data Firehose to transform your data before delivering it.

   * Fully Managed Service, no administration
   * Near Real Time (60 seconds latency minimum for non full batches)
   * Data Ingestion into Redshift / Amazon S3 / ElasticSearch / Splunk
   * Automatic scaling
   * Supports many data formats
   * Data Conversions from CSV / JSON to Parquet / ORC (only for S3)
   * Data Transformation through AWS Lambda (ex: CSV => JSON)
   * Supports compression when target is Amazon S3 (GZIP, ZIP, and SNAPPY)
   * Pay for the amount of data going through Firehose

### Kinesis Data Analytics
With Amazon Kinesis Data Analytics for SQL Applications, you can process and analyze streaming data using standard SQL. The service enables you to quickly author and run powerful SQL code against streaming sources to perform time series analytics, feed real-time dashboards, and create real-time metrics.

   * Use cases
     * Streaming ETL: select columns, make simple transformations, on streaming data
     * Continuous metric generation: live leaderboard for a mobile game
     * Responsive analytics: look for certain criteria and build alerting (filtering)
   * Features
     * Pay only for resources consumed (but it’s not cheap)
     * Serverless; scales automatically
     * Use IAM permissions to access streaming source and destination(s) • SQL or Flink to write the computation
     * Schema discovery
     * Lambda can be used for pre-processing


## AWS Glue
AWS Glue is a fully managed ETL (extract, transform, and load) service that makes it simple and cost-effective to categorize your data, clean it, enrich it, and move it reliably between various data stores and data streams. AWS Glue consists of a central metadata repository known as the AWS Glue Data Catalog, an ETL engine that automatically generates Python or Scala code, and a flexible scheduler that handles dependency resolution, job monitoring, and retries. AWS Glue is serverless, so there’s no infrastructure to set up or manage.

AWS Glue is designed to work with semi-structured data. It introduces a component called a dynamic frame, which you can use in your ETL scripts. A dynamic frame is similar to an Apache Spark dataframe, which is a data abstraction used to organize data into rows and columns, except that each record is self-describing so no schema is required initially. With dynamic frames, you get schema flexibility and a set of advanced transformations specifically designed for dynamic frames. You can convert between dynamic frames and Spark dataframes, so that you can take advantage of both AWS Glue and Spark transformations to do the kinds of analysis that you want.

You can use the AWS Glue console to discover data, transform it, and make it available for search and querying. The console calls the underlying services to orchestrate the work required to transform your data. You can also use the AWS Glue API operations to interface with AWS Glue services. Edit, debug, and test your Python or Scala Apache Spark ETL code using a familiar development environment.

### Glue Data Catalog
  * Metadata repository for all your tables
    * Automated Schema Inference
    * Schemas are versioned
  * Integrates with Athena or Redshift Spectrum (schema & data discovery)
  * Glue Crawlers can help build the Glue Data Catalog

### Glue Data Catalog - Crawlers
  * Crawlers go through your data to infer schemas and partitions
  * Works JSON, Parquet, CSV, relational store 
  * Crawlers work for: S3, Amazon Redshift, Amazon RDS
  * Run the Crawler on a Schedule or On Demand  

  * Need an IAM role / credentials to access the data stores

### Glue and S3 Partitions
  * Glue crawler will extract partitions based on how your S3 data is organized
  * Think up front about how you will be querying your data lake in S3
  * Example: devices send sensor data every hour
  * Do you query primarily by time ranges?
    * If so, organize your buckets as s3://my-bucket/dataset/yyyy/mm/dd/device
  * Do you query primarily by device?
    * If so, organize your buckets as s3://my-bucket/dataset/device/yyyy/mm/dd

### Glue ETL
  * Transform data, Clean Data, Enrich Data (before doing analysis)
    * Generate ETL code in Python or Scala, you can modify the code
    * Can provide your own Spark or PySpark scripts
    * Target can be S3, JDBC (RDS, Redshift), or in Glue Data Catalog
  * Fully managed, cost effective, pay only for the resources consumed
  * Jobs are run on a serverless Spark platform 
  
  * Glue Scheduler to schedule the jobs
  * Glue Triggers to automate job runs based on “events”

### Glue ETL - Transformations
  * Bundled Transformations:
    * DropFields, DropNullFields – remove (null) fields
    * Filter – specify a function to filter records
    * Join – to enrich data
    * Map - add fields, delete fields, perform external lookups 
  * Machine Learning Transformations: 
    * FindMatches ML: identify duplicate or matching records in your dataset, even when the 
      records do not have a common unique identifier and no fields match exactly. 
    * Format conversions: CSV, JSON, Avro, Parquet, ORC, XML
    * Apache Spark transformations (example: K-Means)

## AWS Data Stores for Machine Learning
  * Redshift:
    * Data Warehousing, SQL analytics (OLAP - Online analytical processing)
    * Load data from S3 to Redshift
    * Use Redshift Spectrum to query data directly in S3 (no loading)

  * RDS, Aurora:
    * Relational Store, SQL (OLTP - Online Transaction Processing)
    * Must provision servers in advance

  * DynamoDB:
    * NoSQL data store, serverless, provision read/write capacity
    * Useful to store a machine learning model served by your application

  * S3:
    * Object storage
    * Serverless, infinite storage
    * Integration with most AWS Services

  * OpenSearch (previously ElasticSearch):
    * Indexing of data
    * Search amongst data points
    * Clickstream Analytics

  * ElastiCache:
    * Caching mechanism
    * Not really used for Machine Learning

## AWS Data Pipeline
AWS Data Pipeline is a web service that you can use to automate the movement and transformation of data. With AWS Data Pipeline, you can define data-driven workflows, so that tasks can be dependent on the successful completion of previous tasks. You define the parameters of your data transformations and AWS Data Pipeline enforces the logic that you've set up.

The following components of AWS Data Pipeline work together to manage your data:

  * A pipeline definition specifies the business logic of your data management. For more information
  see Pipeline Definition File Syntax.
  
  * A pipeline schedules and runs tasks by creating Amazon EC2 instances to perform the defined work activities. You upload your pipeline definition to the pipeline, and then activate the pipeline. You can edit the pipeline definition for a running pipeline and activate the pipeline again for it to take effect. You can deactivate the pipeline, modify a data source, and then activate the pipeline again. When you are finished with your pipeline, you can delete it.
  
  * Task Runner polls for tasks and then performs those tasks. For example, Task Runner could copy log files to Amazon S3 and launch Amazon EMR clusters. Task Runner is installed and runs automatically on resources created by your pipeline definitions. You can write a custom task runner application, or you can use the Task Runner application that is provided by AWS Data Pipeline
### AWS Data Pipeline Features
  * Destinations include S3, RDS, DynamoDB, Redshift and EMR
  * Manages task dependencies
  * Retries and notifies on failures
  + Data sources may be on-premises
  * Highly available

### AWS Data Pipeline Example




### AWS Data Pipeline vs Glue
AWS Glue provides support for Amazon S3, Amazon RDS, Redshift, SQL, and DynamoDB and also provides built-in transformations. On the other hand, AWS Data Pipeline allows you to create data transformations through APIs and also through JSON, while only providing support for DynamoDB, SQL, and Redshift. 
Additionally, AWS Glue provides support for the Apache Spark framework (Scala and Python) while AWS Data Pipeline supports all the platforms supported by EMR in addition to Shell. 
Data transformation functionality is a critical factor while evaluating AWS Data Pipeline vs AWS Glue as this will impact your particular use case significantly.

  * Glue:
    * Glue ETL - Run Apache Spark code, Scala or Python based, focus on the ETL
    * Glue ETL - Do not worry about configuring or managing the resources
    * Data Catalog to make the data available to Athena or Redshift Spectrum
  * Data Pipeline:
    * Orchestration service
    * More control over the environment, compute resources that run code, & code
    * Allows access to EC2 or EMR instances (creates resources in your own account)

## AWS Batch
More details here: https://docs.aws.amazon.com/batch/latest/userguide/what-is-batch.html

  * Run batch jobs as Docker images
  * Dynamic provisioning of the instances (EC2 & Spot Instances)
  * Optimal quantity and type based on volume and requirements
  * No need to manage clusters, fully serverless
  * You just pay for the underlying EC2 instances
  * Schedule Batch Jobs using CloudWatch Events
  * Orchestrate Batch Jobs using AWS Step Functions

### AWS Batch vs Glue

  * Glue:
    * Glue ETL - Run Apache Spark code, Scala or Python based, focus on the ETL
    * Glue ETL - Do not worry about configuring or managing the resources
    * Data Catalog to make the data available to Athena or Redshift Spectrum

  * Batch:
    * For any computing job regardless of the job (must provide Docker image)
    * Resources are created in your account, managed by Batch
    * For any non-ETL related work, Batch is probably better

## DMS – Database Migration Service
AWS Database Migration Service (AWS DMS) is a cloud service that makes it easy to migrate relational databases, data warehouses, NoSQL databases, and other types of data stores. You can use AWS DMS to migrate your data into the AWS Cloud or between combinations of cloud and on-premises setups.

With AWS DMS, you can perform one-time migrations, and you can replicate ongoing changes to keep sources and targets in sync. If you want to migrate to a different database engine, you can use the AWS Schema Conversion Tool (AWS SCT) to translate your database schema to the new platform. You then use AWS DMS to migrate the data. Because AWS DMS is a part of the AWS Cloud, you get the cost efficiency, speed to market, security, and flexibility that AWS services offer.

At a basic level, AWS DMS is a server in the AWS Cloud that runs replication software. You create a source and target connection to tell AWS DMS where to extract from and load to. Then you schedule a task that runs on this server to move your data. AWS DMS creates the tables and associated primary keys if they don't exist on the target. You can precreate the target tables yourself if you prefer. Or you can use AWS Schema Conversion Tool (AWS SCT) to create some or all of the target tables, indexes, views, triggers, and so on.

  * Quickly and securely migrate databases to AWS, resilient, self healing
  * The source database remains available during the migration
  * Supports:
    * Homogeneous migrations: ex Oracle to Oracle
    * Heterogeneous migrations: ex Microsoft SQL Server to Aurora
  * Continuous Data Replication using CDC
  * You must create an EC2 instance to perform the replication tasks

### AWS DMS vs Glue
  * Glue:
    * Glue ETL - Run Apache Spark code, Scala or Python based, focus on the ETL
    * Glue ETL - Do not worry about configuring or managing the resources
    * Data Catalog to make the data available to Athena or Redshift Spectrum
  * AWS DMS:
    * Continuous Data Replication
    * No data transformation
    * Once the data is in AWS, you can use Glue to transform it


## AWS Step Functions
Step Functions is a serverless orchestration service that lets you combine AWS Lambda functions and other AWS services to build business-critical applications. Through Step Functions' graphical console, you see your application’s workflow as a series of event-driven steps.

Step Functions is based on state machines and tasks. A state machine is a workflow. A task is a state in a workflow that represents a single unit of work that another AWS service performs. Each step in a workflow is a state.

With Step Functions' built-in controls, you examine the state of each step in your workflow to make sure that your application runs in order and as expected. Depending on your use case, you can have Step Functions call AWS services, such as Lambda, to perform tasks. You can create workflows that process and publish machine learning models. You can have Step Functions control AWS services, such as AWS Glue, to create extract, transform, and load (ETL) workflows. You also can create long-running, automated workflows for applications that require human interaction.

  * Use to design workflows
  * Easy visualizations
  * Advanced Error Handling and Retry mechanism outside the code
  * Audit of the history of workflows
  * Ability to “Wait” for an arbitrary amount of time
  * Max execution time of a State Machine is 1 year

### Step Functions – Examples
  * Train a Machine Learning Model
  * Tune a Machine Learning Model
  * Manage a Batch Job
