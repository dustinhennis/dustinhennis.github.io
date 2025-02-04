---
layout: post
tags: aws cloudwatch
---

The following provides an overview of how to manually inject messages into AWS CloudWatch log streams.  This can be useful for testing any CloudWatch metric filters looking for specific log messages.

### Create events file (ie. events.json)

{% gist fd7d81a6c6cca3af2f06c810a24e32f7 %}

### Put log events

{% highlight shell %}
aws logs put-log-events \
--log-group-name log_group_name \
--log-stream-name stream_name \
--log-events file://events.json \
--profile profile_name \
--region us-east-1
{% endhighlight %}

Returns the following response (example)

{% highlight json %}
{
    "nextSequenceToken": "49601496708150489434817690129464993209400204711539685554"
}
{% endhighlight %}

### Put log events using the sequence token

{% highlight shell %}
aws logs put-log-events \
--log-group-name log_group_name \
--log-stream-name stream_name \
--log-events file://events.json \
--sequence-token 49601496708150489434817690129464993209400204711539685554 \
--profile profile_name \
--region us-east-1
{% endhighlight %}

---
[AWS CLI User Guide: Configuration and Credential File Settings](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)

[AWS CLI Command Reference: logs/put-log-events](https://docs.aws.amazon.com/cli/latest/reference/logs/put-log-events.html)

---
### Tags

{%- if page.tags -%}
    {% for tag in page.tags %}
[#{{ tag }}](http://dustinhennis.github.io/tags#{{tag | slugize}})
    {% endfor %}
{%- endif -%}