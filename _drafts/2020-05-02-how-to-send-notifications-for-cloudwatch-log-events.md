---
layout: post
---
<!-- 
## Create SNS Requirements

### Create IAM Policy Document

Terraform Example
{% highlight terraform %}
data "aws_iam_policy_document" "my-topic-policy" {
  policy_id = "__default_policy_ID"

  statement {
    actions = [
      "SNS:Subscribe",
      "SNS:SetTopicAttributes",
      "SNS:RemovePermission",
      "SNS:Receive",
      "SNS:Publish",
      "SNS:ListSubscriptionsByTopic",
      "SNS:GetTopicAttributes",
      "SNS:DeleteTopic",
      "SNS:AddPermission",
    ]

    condition {
      test     = "StringEquals"
      variable = "AWS:SourceOwner"

      values = [
        "AWS_ACCOUNT",
      ]
    }

    effect = "Allow"

    principals {
      type = "AWS"
      identifiers = [
        "*",
      ]
    }

    resources = [
      aws_sns_topic.my_topic.arn,
    ]

    sid = "__default_statement_ID"
  }
}
{% endhighlight %}


### Create SNS Topic Policy

Terraform Example
{% highlight terraform %}
resource "aws_sns_topic_policy" "my_topic_policy" {
  arn = aws_sns_topic.my_topic.arn

  policy = data.aws_iam_policy_document.my-topic-policy.json
}
{% endhighlight %}
 -->

## Create SNS Topic

Terraform Example [^1]
{% highlight terraform %}
resource "aws_sns_topic" "my_topic" {
  name = "my_topic"
}
{% endhighlight %}

## Create CloudWatch Log Metric Filter

[Event Patterns in CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatchEventsandEventPatterns.html)

Terraform Example [^2]
{% highlight terraform %}
resource "aws_cloudwatch_log_metric_filter" "my_metric_filter" {
  name           = "metric_filter_name"
  pattern        = "[..., message=\"*What is the Matrix?*\"]" 
  log_group_name = "/log/group/name"

  metric_transformation {
    name          = "metric_name" # This will show up in the subject for SNS notifications via Email
    namespace     = "metric_namespace"
    value         = "1"
    default_value = 0
  }
}
{% endhighlight %}


## Create CloudWatch Metric Alarm

Terraform Example [^3]
{% highlight terraform %}
resource "aws_cloudwatch_metric_alarm" "my_metric_alarm" {
  alarm_name          = "metric_alarm_name"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = "1"
  metric_name         = "metric_name"
  namespace           = "metric_namespace"
  period              = "60"
  statistic           = "Maximum"
  threshold           = "0"
  datapoints_to_alarm = "1"
  # dimensions = {}
  alarm_actions       = ["arn:aws:sns:REGION:AWS_ACCOUNT:my_topic"] # Arn for SNS topic above
}
{% endhighlight %}

---

[^1]:[Terraform Resource aws_sns_topic](https://www.terraform.io/docs/providers/aws/r/sns_topic.html)
[^2]:[Terraform Resource aws_cloudwatch_log_metric_filter](https://www.terraform.io/docs/providers/aws/r/cloudwatch_log_metric_filter.html)
[^3]:[Terraform Resource aws_cloudwatch_metric_alarm](https://www.terraform.io/docs/providers/aws/r/cloudwatch_metric_alarm.html)
