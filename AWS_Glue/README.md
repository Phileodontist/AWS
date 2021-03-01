# AWS Glue Notes
***

## Main Concepts

### General
* Security Group with a self referencing inbound rule 
* When connecting to local S3 on your VPC, must have the following:
  * VPC Endpoint for s3 (gateway)
  * IAM Role with the `AmazonS3FullAccess` policy

### Connections
Specifies connection information for any given data resource.

### Crawlers
Using a `connection`, a crawler crawls through the specific data source, identifying generating metadata schema(s) through the use of classifies.
**Note:**  Custom customizers can be used to specify data formats: `JSON, XML, CSV`

**Prerequisites**
* Must have an IAM policy to have permissions to extract data from your data store and write to the Data Catalog. `AWSGlueServiceRole `


### Data Catalog
A repository that stores metadata about data sources, storing schemas used to retrieve data during the executino of a `job`.

### Job
Typically a scheduled event where data is retrieved from a data source, transformed and written to the specified destination.
