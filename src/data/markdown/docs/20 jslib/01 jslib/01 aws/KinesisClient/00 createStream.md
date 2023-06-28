---
title: 'KinesisClient.createStream'
description: 'KinesisClient.createStream creates a new Kinesis stream with the specified name and options.'
excerpt: 'KinesisClient.createStream creates a new Kinesis stream with the specified name and options.'
---

`KinesisClient.createStream` creates a new Kinesis stream with the specified name and options.

### Parameters

| Name          | Type              | Description                                                                                                                                                                  |
| :------------ | :---------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `streamName`  | string            | The name of the stream to create. |
| `options`     | object (optional) | An optional object containing configuration options for the stream. Accepted properties are: `shardCount` (optional number) setting the number of shards for the stream, and `streamModeDetails` (optional object) setting the stream mode details.|

### Throws

| Type                    | Description                                           |
| :---------------------- | :---------------------------------------------------- |
| `InvalidSignatureError` | when using invalid credentials.                       |
| `KinesisServiceError`   | Throws an error if the stream creation request fails. |

### Returns

| Type         | Description                                                               |
| :----------- | :------------------------------------------------------------------------ |
| `void`       | Returns nothing as this method sends a create stream request without expecting a response. |

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
    kinesis.deleteStream(testStream);
}
```

</CodeGroup>