---
layout: post
---
## How To: Put events into CloudWatch log streams

The following provides an overview of how to manually inject messages into AWS CloudWatch log streams.  This can be usefule for testing any CloudWatch metric filters looking for specific log messages.

1. Create events file (ie. events.json)

{% gist fd7d81a6c6cca3af2f06c810a24e32f7 %}

2. Put log events
```
aws logs put-log-events \
--log-group-name log_group_name \
--log-stream-name stream_name \
--log-events file://events.json \
--profile profile_name \
--region us-east-1
```

Returns the following response
```
{
    "nextSequenceToken": "49601496708150489434817690129464993209400204711539685554"
}
```

3. Put log events using the sequence token
```
aws logs put-log-events \
--log-group-name log_group_name \
--log-stream-name stream_name \
--log-events file://aws-cloudwatch-log-events.json \
--sequence-token 49601496708150489434817690129464993209400204711539685554 \
--profile profile_name \
--region us-east-1
```

---
[^1]: [AWS CLI User Guide: Configuration and Credential File Settings](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)

[^2]: [AWS CLI Command Reference: logs/put-log-events](https://docs.aws.amazon.com/cli/latest/reference/logs/put-log-events.html)