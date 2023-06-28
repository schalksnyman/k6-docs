---
title: 'KinesisClient.putRecords'
description: 'KinesisClient.putRecords sends multiple records to a Kinesis stream in a single request.'
excerpt: 'KinesisClient.putRecords sends multiple records to a Kinesis stream in a single request.'
---

`KinesisClient.putRecords` sends multiple records to a Kinesis stream in a single request.

### Parameters

| Name          | Type              | Description                                                                                                                                                                  |
| :------------ | :---------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `records`     | [PutRecordsRequestEntry](#putrecordsrequestentry)[] | An array of records to put into the stream. |
| `parameters`  | object (optional) | An optional object containing additional options for the put records operation. Accepted properties are: `streamName` (optional string) specifying the name of the stream, and `streamARN` (optional string) specifying the Amazon Resource Name of the stream. |

#### PutRecordsRequestEntry

A `PutRecordsRequestEntry` describes a record to be created, and consists of an object with the following properties:

| Name          | Type              | Description                                                                                                                                                                  |
| :------------ | :---------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Data`        | string | The data blob to put into the record, which is base64-encoded when the blob is serialized. The maximum size of the data blob (the payload after base64-decoding) is 1 megabyte (MB). |
| `PartitionKey` | string | Determines which shard in the stream the data record is assigned to. Partition keys are Unicode strings with a maximum length limit of 256 characters for each key. |


### Returns

| Type                                      | Description                                                 |
| :---------------------------------------- | :---------------------------------------------------------- |
| [PutRecordsResponse](#putrecordsresponse) | Returns a partial instance of the PutRecordsResponse class. |

#### PutRecordsResponse

A `PutRecordsResponse` is an object with the following properties:

| Name          | Type              | Description                                                                                                                                                                  |
| :------------ | :---------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Records` | [PutRecordsResultEntry](#putrecordsresultentry)[] | An array of successfully and unsuccessfully processed record results, correlated with the request by natural ordering. |
| `FailedRecordCount` | number | The number of unsuccessfully processed records in a `PutRecords` request. |
| `EncryptionType` | string | The encryption type used on the records. This field can be one of the following values: `NONE`, `KMS`. |

## Throws

| Type                    | Description                                      |
| :---------------------- | :----------------------------------------------- |
| `InvalidSignatureError` | when using invalid credentials.                  |
| `KinesisServiceError`   | Throws an error if the put record request fails. |

### Examples

<CodeGroup labels={[]}>

```javascript
import exec from 'k6/execution'

import { AWSConfig, KinesisClient, PutRecordsRequestEntry } from 'https://jslib.k6.io/aws/0.8.0/kinesis.js'

const kinesis = new KinesisClient(AWSConfig.fromEnvironment())

export default function () {
    // Prepare records
    const records = [
        {
            Data: JSON.stringify({ message: 'Record 1' }),
            PartitionKey: 'partition-key-1',
        },
        {
            Data: JSON.stringify({ message: 'Record 2' }),
            PartitionKey: 'partition-key-2',
        },
    ];

    // Put records into a Kinesis stream
    const putRecordsResult = kinesis.putRecords(records, { streamName: 'test-stream'});

    console.log(putRecordsResult);
}
```

</CodeGroup>
