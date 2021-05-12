# AWS 

## S3

### Include/Exclude 

```sh
aws s3 sync s3://bucket1/path/ s3://bucket2/path/ --include '*' --exclude '*.xml'
aws s3 rm s3://bucket1/path --exclude '*' --include '*.jpg' --recursive
```

### High-throughput copy configuration

```sh
aws configure set default.s3.max_concurrent_requests 1000
aws configure set default.s3.max_queue_size 10000
aws configure set default.s3.multipart_threshold 64MB
aws configure set default.s3.multipart_chunksize 16MB
```

### Accessing Requester Pays Buckets

By default, the S3 storage, request, and data transfer costs are all paid for by the owner of the objects.  However, an owner may choose to enable the Requester Pays feature of S3 to have the request and data transfer costs changed to the person accessing the objects.  This requires a special flag to be added to requests so that the caller knows that they're going to be charged for the request.  

```
$ aws s3 cp s3://bucket/path/file.tif . 
fatal error: An error occurred (403) when calling the HeadObject operation: Forbidden
$ aws s3 cp s3://bucket/path/file.tif . --request-payer
download: s3://bucket/path/file.tif  to ./file.tif
```

Note that if you get a message saying that the `--request-payer` option is invalid, you need to upgrade to a newer version of the AWS CLI.

http://docs.aws.amazon.com/AmazonS3/latest/dev/ObjectsinRequesterPaysBuckets.html

Note that all of the examples assume that an AWS credential provider is configured.  There are several options for doing this, most typically as a set of exported environment variables AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY or as a file ~/.aws/credentials. 

Examples using Requestor Pays

AWS CLI:
```
aws s3api get-object --request-payer requester s3://bucket/path/file.tif file.tif
aws s3 cp --request-payer s3://bucket/path/file.tif .
```

GDAL
gdalinfo --config AWS_REQUEST_PAYER requester /vsis3/bucket/path/file.tif 

Boto
add parameter RequestPayer='requester' to the request (e.g., get_object)

Java API v1
	GetObjectRequest#setRequesterPays(boolean isRequesterPays)

Java API v2
	GetObjectRequest.Builder#requestPayer(RequestPayer.REQUESTER)


### Details on S3 Costs

There are three aspects of S3 usage that incur cost: Storage, Request, and Data Transfer.  

* Storage - cost for storing the data in S3, billed in prorated GB/month (about $0.022 per GB/month)
* Request - cost for making a request to list a set of objects or retrieve an object, billed per 1,000 requests ($0.005 per 1,000 requests)
* Data Transfer - cost for transferring bytes from S3 out to the Internet or to an S3 Region different from the one in which the data is stored. Data retrieved from S3 in the same S3 Region as the data is stored incurs no cost. The cost varies depending on how much total data is retrieved each month, from free for the first GB to $0.05 per GB when great than 150 TB/Month are retrieved.

Storage costs are always paid by the owner of the data.  

### Static Website

[Hosting a static website using Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)

## Athena

Create w/ ORC:
```
CREATE EXTERNAL TABLE {namespace}.{table_name} (
  bucket string,
  key string,
  size bigint,
  last_modified_date timestamp,
  storage_class string
  )
  PARTITIONED BY (dt string)
  ROW FORMAT SERDE 'org.apache.hadoop.hive.ql.io.orc.OrcSerde'
  STORED AS INPUTFORMAT 'org.apache.hadoop.hive.ql.io.SymlinkTextInputFormat'
  OUTPUTFORMAT  'org.apache.hadoop.hive.ql.io.IgnoreKeyTextOutputFormat'
  LOCATION 's3://bucket1-inventory/bucket1/path1/hive/'
```

Repair:
```
MSCK REPAIR TABLE {namespace}.{table_name}
```
