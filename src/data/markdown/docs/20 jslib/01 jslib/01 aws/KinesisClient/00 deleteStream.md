---
title: 'KinesisClient.deleteStream'
description: 'KinesisClient.deleteStream deletes a Kinesis stream with the specified parameters.'
excerpt: 'KinesisClient.deleteStream deletes a Kinesis stream with the specified parameters.'
---

`KinesisClient.deleteStream` deletes a Kinesis stream with the specified parameters.

### Parameters

| Name          | Type              | Description                                                                                                                                                                  |
| :------------ | :---------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `streamName`  | string            | The name of the stream to delete. |
| `parameters`  | object (optional) | An optional object containing configuration options for the stream deletion. Accepted properties are: `streamARN` (optional string) for the Amazon Resource Name of the stream, and `enforceConsumerDeletion` (optional boolean) to enforce the deletion of all registered consumers before deleting the stream.|

### Throws

| Type                    | Description                                           |
| :---------------------- | :---------------------------------------------------- |
| `InvalidSignatureError` | when using invalid credentials.                       |
| `KinesisServiceError`   | Throws an error if the stream deletion request fails. |

### Examples

<CodeGroup labels={[]}>

```javascript
import exec from 'k6/execution'

import { AWSConfig, KinesisClient } from 'https://jslib.k6.io/aws/0.8.0/kinesis.js'

const awsConfig = AWSConfig.fromEnvironment()
const kinesis = new KinesisClient(awsConfig)
const testStream = 'test-stream'

export default function () {
    // Create test stream
    kinesis.createStream(testStream, { shardCount: 10, streamModeDetails: 'PROVISIONED' });

    // Delete test stream
    kinesis.deleteStream(testStream, {
        streamARN: 'arn:aws:kinesis:us-west-2:123456789012:stream/test-stream',
        enforceConsumerDeletion: true
    });
}
```

</CodeGroup>