---
description: How to add a widget with custom text.
---

{% hint style="info" %}
**You are looking at the old Evidently documentation**: this API is available with versions 0.6.7 or lower. Check the newer docs version [here](https://docs.evidentlyai.com/introduction).
{% endhint %}

**Pre-requisites**:
* You know how to generate custom Reports using individual Metrics.

# Code example

How-to notebook:
{% embed url="https://github.com/evidentlyai/evidently/blob/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/how_to_questions/how_to_add_a_text_comment_to_the_report.ipynb" %}

# What you can do

You can add a widget that contains any custom text to the Evidently Report. Here is how this can look:

![Text Comment()](../.gitbook/assets/reports/metric_comment-min.png)

 You can include multiple text widgets in a single Report. 

# Using “Comment” Metric

To add a text widget, you must first define the contents of the comment. You can use markdown to format the text.

An example of adding "model_description" comment:

```python
model_description = """
 # Model Description
 This is a demand forecasting model.


 ## Intended use
 * Weekly sales planning
 * Weekly capacity planning
"""
```

When creating the Report, include the `Comment` Metric and reference the earlier defined text.

Example:

```python
report = Report(metrics=[
   Comment(model_description),
   ColumnDistributionMetric('TOTAL_ORDERS')
])


report.run(current_data=raw_data, reference_data=None)
report
```


