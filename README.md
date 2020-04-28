# Kinesis
This repository consists of different python based lambda implementations that are used to deaggregate records when produced by a producer that use kinesis aggregation/deaggregation module to put records to Kinesis Datastreams, Kinesis Firehose and Kinesis Data Analytics.

The AWS recommended aggregation/deaggregation module can be found here:
https://github.com/awslabs/kinesis-aggregation

# Kinesis Datastreams Lambda Processor
Use this lambda transformer when records are put into KDS from a producer that uses the kinesis aggregation/deaggregation module.
In case of KDS, a kinesis datastream event is configured with the lambda function that recieves the aggregated records. The aggregated records need to be deaggregated in the lambda handler. For that we create a deployment package, with the specified aggregation/deaggregation module in it.

Input data model in case of kinesis datastreams:

{
  "invocationId" : Lambda invocation ID (random GUID)
  "applicationArn" : Kinesis Analytics application ARN,
  "streamArn"    : Source stream ARN of the records,
  "records": [
    {
      "recordId" : random GUID,
      "kinesisStreamRecordMetadata" : {
        "sequenceNumber" : from the Kinesis Record,
        "partitionKey" : from the Kinesis Record,
        "shardId" : from the Kinesis Record
        "approximateArrivalTimestamp" : from the Kinesis Record
      },
      "data" : base64 encoded user payload
    }
  ]
}

*note: directly upload the provided deployment_package to s3

# Kinesis Firehose Lambda Preprocessor
Use this lambda transformer when records are put into KFH from a producer that uses the kinesis aggregation/deaggregation module. In case of KFH, the records are automatically deaggregated by the Firehose service so we do not have to manually deaggregate them in the lambda handler.


# Kinesis Data Analytics Lambda Preprocessor
Use this lambda transformer when records are put into KDA from a producer that uses the kinesis aggregation/deaggregation module. If records are ingested into KDA application then we have to deaggregated them in the lambda handler, if records are ingested using Firehose then we do not have to deaggregate them as Firehose does that itself.

*note: directly upload the provided deployment_package to s3


