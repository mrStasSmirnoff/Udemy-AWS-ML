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
   * **Kinesis Streams**: low latency streaming ingest at scale
   * **Kinesis Analytics**: perform real-time analytics on streams using SQL
   * **Kinesis Firehose**: load streams into S3, Redshift, ElasticSearch & Splunk
   * **Kinesis Video Streams**: meant for streaming video in real-time

 * Streams are divided in ordered Shards / Partitions
 * Multiple applications can consume the same stream
 * Once data is inserted in Kinesis, it can’t be deleted (immutability)
 * Records can be up to 1MB in size

### Kinesis Data Streams – Capacity Modes

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
  
 * Kinesis Data Firehose
   * Fully Managed Service, no administration
   * Near Real Time (60 seconds latency minimum for non full batches)
   * Data Ingestion into Redshift / Amazon S3 / ElasticSearch / Splunk
   * Automatic scaling
   * Supports many data formats
   * Data Conversions from CSV / JSON to Parquet / ORC (only for S3)
   * Data Transformation through AWS Lambda (ex: CSV => JSON)
   * Supports compression when target is Amazon S3 (GZIP, ZIP, and SNAPPY)
   * Pay for the amount of data going through Firehose

 * Kinesis Data Analytics
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