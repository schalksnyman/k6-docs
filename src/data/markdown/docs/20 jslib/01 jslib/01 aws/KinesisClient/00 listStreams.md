---
title: 'KinesisClient.listStreams'
description: 'KinesisClient.listStreams returns a list of Kinesis streams with the specified parameters.'
excerpt: 'KinesisClient.listStreams returns a list of Kinesis streams with the specified parameters.'
---

`KinesisClient.listStreams` returns a list of Kinesis streams with the specified parameters.

### Parameters

| Name          | Type              | Description                                                                                                                                                                  |
| :------------ | :---------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `parameters`  | object (optional) | An optional object containing configuration options for listing the streams. Accepted properties are: `exclusiveStartStreamName` (optional string) to start listing from a specific stream, `limit` (optional number) to set the maximum number of streams to list, and `nextToken` (optional string) for paginating the list of streams. |

## Throws

| Type                    | Description                                           |
| :---------------------- | :---------------------------------------------------- |
| `InvalidSignatureError` | when using invalid credentials.                       |
| `KinesisServiceError`   | Throws an error if the request to list streams fails. |

### Returns

| Type         | Description                                                               |
| :----------- | :------------------------------------------------------------------------ |
| `Partial<ListStreamsResponse>` | Returns a partial instance of the ListStreamsResponse class. |

### Examples

<CodeGroup labels={[]}>

```javascript
import exec from 'k6/execution'

import { AWSConfig, KinesisClient } from 'https://jslib.k6.io/aws/0.8.0/kinesis.js'

const awsConfig = AWSConfig.fromEnvironment()
const kinesis = new KinesisClient(awsConfig)

export default function () {
    // List Kinesis streams with parameters
    const streams = kinesis.listStreams({
        exclusiveStartStreamName: 'test-stream',
        limit: 10,
        nextToken: 'next-token',
    });

    console.log(JSON.stringify(streams, null, 2));
}
```

</CodeGroup>
