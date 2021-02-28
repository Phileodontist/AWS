import sys
import re
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from awsglue.dynamicframe import DynamicFrame
from awsglue.context import GlueContext
from awsglue.job import Job

from pyspark.context import SparkContext
from pyspark.sql.functions import *
from pyspark.sql.types import *

## @params: [JOB_NAME]
args = getResolvedOptions(sys.argv, ['JOB_NAME'])

sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
job = Job(glueContext)
job.init(args['JOB_NAME'], args)


def retrieveFirstName(name):
    fullName = re.sub("(Mr\.|Mrs\.|Dr\.|Miss)", "", name).split(" ")
    return fullName[0]
    
def retrieveLastName(name):
    fullName = re.sub("(Mr\.|Mrs\.|Dr\.|Miss)", "", name).split(" ")
    return fullName[1]
    
def retrieveBirthDate(birthDate):
    ## Remove Minutes/Seconds/etc from birthdate
    return birthDate.split(" ")[0]     
    
def replaceNewLine(field):
    ## Remove Minutes/Seconds/etc from birthdate
    return re.sub("\n", " ", field)   

## @type: DataSource
## @args: [database = "enterprise", table_name = "test_profile_data_csv", transformation_ctx = "datasource0"]
## @return: datasource0
## @inputs: []
########### Creating glue dynamic frame from the catalog ###########
ds = glueContext.create_dynamic_frame.from_catalog(database = "enterprise", table_name = "test_profile_data_csv", transformation_ctx = "datasource0")

########### Creating spark data frame from the glue context ###########
ds_df = ds.toDF()

fnUDF = udf(retrieveFirstName, StringType()) 
lnUDF = udf(retrieveLastName, StringType()) 
bdUDF = udf(retrieveBirthDate, StringType()) 
rnlUDF = udf (replaceNewLine, StringType()) 

ds_df_transformed = ds_df.withColumn("firstName", fnUDF(col("name")))       \
                        .withColumn("lastName", lnUDF(col("name")))         \
                        .withColumn("birthDate", bdUDF(col("birthdate")))   \
                        .withColumn("residence", rnlUDF(col("residence")))  \
                        .withColumn("address", rnlUDF(col("address")))

datasource0 = DynamicFrame.fromDF(ds_df_transformed, glueContext, "datasource0")

## @type: DataSink
## @args: [connection_type = "s3", connection_options = {"path": "s3://testingglueconnection"}, format = "csv", transformation_ctx = "datasink2"]
## @return: datasink2
## @inputs: [frame = applymapping1]
datasink2 = glueContext.write_dynamic_frame.from_options(frame = datasource0, connection_type = "s3", connection_options = {"path": "s3://testingglueconnection/profiles_transformed"}, format = "csv", transformation_ctx = "datasink2")
job.commit()
