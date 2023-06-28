---
title: 'KinesisClient'
head_title: 'KinesisClient'
description: 'KinesisClient enables interaction with the AWS Kinesis Service'
excerpt: 'KinesisClient allows interacting with the AWS Kinesis Service'
---

<BlockingAwsBlockquote />

`KinesisClient` interacts with the AWS Kinesis Service. With it, the user can create and delete streams, list available streams in the current region, send and retrieve data records, list shards in a stream, and retrieve a shard iterator. `KinesisClient` operations are blocking.

Both the dedicated `kinesis.js` jslib bundle and the all-encompassing `aws.js` bundle include the `KinesisClient`.

### Methods

| Function                                                         | Description                                          |
| :--------------------------------------------------------------- | :--------------------------------------------------- |
| [`createStream`](/javascript-api/jslib/aws/kinesisclient/kinesisclient-createstream) | Creates a new Kinesis stream. |
| [`deleteStream`](/javascript-api/jslib/aws/kinesisclient/kinesisclient-deletestream) | Deletes a Kinesis stream. |
| [`listStreams`](/javascript-api/jslib/aws/kinesisclient/kinesisclient-liststreams) | Returns a list of your streams. |
| [`putRecords`](/javascript-api/jslib/aws/kinesisclient/kinesisclient-putrecords) | Sends multiple records to a Kinesis stream in a single request. |
| [`getRecords`](/javascript-api/jslib/aws/kinesisclient/kinesisclient-getrecords) | Retrieves records from a Kinesis stream. |
| [`listShards`](/javascript-api/jslib/aws/kinesisclient/kinesisclient-listshards) | Lists the shards in a Kinesis stream. |
| [`getShardIterator`](/javascript-api/jslib/aws/kinesisclient/kinesisclient-getsharditerator) | Retrieves a shard iterator for the specified shard in a Kinesis stream. |


### Throws

`KinesisClient` methods throw errors in case of failure.

| Error                   | Condition                                                 |
| :---------------------- | :-------------------------------------------------------- |
| `InvalidSignatureError` | when using invalid credentials                            |
| `KinesisServiceError`   | when AWS replied to the requested operation with an error |

### Example

<CodeGroup labels={[]}>

```javascript
import exec from 'k6/execution'

import { AWSConfig, KinesisClient } from 'https://jslib.k6.io/aws/0.8.0/kinesis.js'

const awsConfig = new AWSConfig({
    region: __ENV.AWS_REGION,
    accessKeyId: __ENV.AWS_ACCESS_KEY_ID,
    secretAccessKey: __ENV.AWS_SECRET_ACCESS_KEY,
    sessionToken: __ENV.AWS_SESSION_TOKEN,
})

const kinesis = new KinesisClient(awsConfig)
const testStream = 'test-stream'

export default function () {
    // Create test stream
    kinesis.createStream(testStream, { shardCount: 10, streamModeDetails: 'PROVISIONED' });

    // If our test stream does not exist, abort the execution.
    const streamsResponse = kinesis.listStreams()
    if (streamsResponse.streamNames.filter((s) => s === testStream).length == 0) {
        exec.test.abort()
    }

    // Send record to test stream
    kinesis.putRecords([{ Data: JSON.stringify({value: '123'}), PartitionKey: 'test' }], { streamName: testStream });

    // Delete test stream
    kinesis.deleteStream(testStream);
}
```

</CodeGroup>