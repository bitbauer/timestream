# Timeseries Data Lake for Climate prediction

## Introduction

Ingestion and processing of lots of climate data which is temporally relevant. 

A classic approach would be to set up a database like RDS which is cheap. Performance of classical RDS will be an issue, when you handle billions of records, that gets high frequent updates.

Alternatively, DynamoDB is an option which provides easy set up, is highly scalable and supports TTL. But querying and aggregating the data is difficult.

Based on our experience, Timestream is the better choice. It is a fully AWS managed time-series database, designed for huge amounts of data. (1)
Storage and query engine are decoupled which makes the dynamic scaling easy. Additionally, scaling is done completely under the hood, because it’s a SaaS solution.
There are other-time series solutions, which might have their special strengths, but because of our Big Data use case we choose AWS Timestream.
Timestream has connectors for Telegraf open source agent and for Prometheus time series database for system data
Don’t get it? So, let’s build it with DynamoDB. When you want to query huge datasets in DynamoDB, you need to create partitions. For strictly monotonically increasing timestamps, you have to generate hashed values for balancing data distribution in the partitions. This produces a lot of overhead in storing and querying your table, that isn’t necessary when using AWS Timestream.

## Solution architecture

<DIAGRAMM>

The solution architecture consists of several layers. Ingestion of events is handled by an API Gateway which performs authentication and additional steps such as schema validation. 
The data is then written to Timestream and stored durably. In our experience, a database/table per XXX is best suited for this task.


- Ingestion through API GW
- Timestream (Table / Database per XYZ)
- Analytics/Prediction: Sagemaker
- Visualization: Amazon QuickSight / Grafana ?

## Implementation
<GitHub Link> and <Documentation>

## Summary

### Links:
(1) [Amazon Timestream is now Generally Available](https://aws.amazon.com/about-aws/whats-new/2020/09/amazon-timestream-now-generally-available/)

----------------------------------------------------

### Open questions

* Table per measure vs multiple measure in one table
* Differences in query language to standard SQL
<<<<<<< HEAD
* Differences RDS vs ElasticSearch vs TimeScale vs TimeStream
* Pricing structure
* AWS Grafana + Timestream = Better together ? 
* Pricing structure (https://blog.timescale.com/blog/timescaledb-vs-amazon-timestream-6000x-higher-inserts-175x-faster-queries-220x-cheaper/)

#### Saved und Recent Queries sind nur für den eigenen Benutzer sichtbar
* Lustigerweise bleiben die Benutzerqueries bei Logout/Login erhalten. Wie werden sie in AWS gespeichert?
