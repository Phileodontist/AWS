# Processing Profile Data
***
This project serves as an exercise to read data from S3 into AWS Glue, to then be transformed and written back to S3.

**Note:** Data used within this project was derived from the Faker API, using generated data to produce profiles of individuals

## Requirements
For this particular task it assumes that theres a database `enterprise` in AWS Glue with the table `test_profile_data_csv`.
