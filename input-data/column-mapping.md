---
description: How to use column mapping in Evidently.
---

{% hint style="info" %}
**You are looking at the old Evidently documentation**: this API is available with versions 0.6.7 or lower. Check the newer version [here](https://docs.evidentlyai.com/introduction).
{% endhint %}


Column mapping helps map your input data schema or specify column types. For example, to run evaluation on text data, you must specify which columns in your dataset contain texts. This allows Evidently to process the input data correctly. 

You can create a `ColumnMapping` object in Python prior to generating a Report or Test Suite or map the columns visually when working in the Evidently platform.

You only need to map columns that will be used in your evaluations. 

# Default mapping strategy

If the `column_mapping` is not specified or set as `None`, Evidently will use the default mapping strategy, trying to match the columns automatically.

**Column types**:
* All columns with numeric types (np.number) will be treated as Numerical. 
* All columns with DateTime format (np.datetime64) will be treated as DateTime. 
* All other columns will be treated as Categorical.

**Dataset structure**:
* The column named **"id"**  will be treated as an ID column. 
* The column named **"datetime"** will be treated as a DateTime column. 
* The column named **"target"** will be treated as a target column with a true label or value.
* The column named **"prediction"** will be treated as a model prediction.

# Which columns do you need?

To run certain types of evaluations, you must include specific columns or provide a reference dataset. For example, to run text evaluations, you must have at least one column labeled as text. To run data drift checks, you always need a reference dataset.

Here are example requirements:

| Evaluation | Feature columns  | Prediction column | Target column  | ID column | Datetime column | Reference dataset
|---|---|---|---|---|---|---|
| **Text Evals** | Required (Text) | Optional | Optional | Optional | Optional | Optional |
| **Data Quality** | Required (Any) | Optional | Optional | Optional | Optional | Optional |
| **Data Drift** | Required (Any) | Optional | Optional | Optional | Optional | Required |
| **Target Drift** | Optional | Target and/or prediction required | Target and/or prediction required | Optional | Optional | Required |
| **Classification** | Optional | Required | Required | Optional | Optional | Optional |
| **Regression** | Optional | Required | Required | Optional | Optional | Optional |

{% hint style="info" %} 
**It's best always to use column mapping**. Without it, Evidently will apply its own heuristics to map the input data automatically. To avoid errors, it's safer to set the column mapping manually.
{% endhint %}

# Code example

Notebook example on specifying column mapping:

{% embed url="https://github.com/evidentlyai/evidently/blob/main/examples/how_to_questions/how_to_use_column_mapping.ipynb" %}

**Imports**. Imports to use column mapping:

```python
from evidently import ColumnMapping
```

**Basic API**. Once you create a `ColumnMapping` object, you pass it along with the data when computing the [Report or Test Suite](../tests-and-reports/introduction.md). For example:

```python
column_mapping = ColumnMapping()

column_mapping.target = 'target'
column_mapping.prediction = 'prediction'
column_mapping.numerical_features = numerical_features
column_mapping.categorical_features = categorical_features

report = Report(metrics=[
    RegressionPreset(),
])

report.run(reference_data=ref,
           current_data=cur,
           column_mapping=column_mapping)

report
```

# Column mapping

## DateTime and ID

To map columns containing DateTime and ID:

```python
column_mapping.datetime = 'date' #'date' is the name of the column with datetime
column_mapping.id = None #there is no ID column in the dataset
```

{% hint style="info" %} 
**Why map them:** A "DateTime" column serves as the index for certain plots, giving you richer visualizations in your Reports. If you have a timestamp column, it's always a good idea to map it. Mapping the "Datetime" and "ID" columns also excludes them from analyses like data drift detection, where it wouldn't add any value.
{% endhint %}

## Target and Prediction

To map columns containing Target and Prediction:

```python
column_mapping.target = 'y' 
column_mapping.prediction = 'pred' 
```

This matches regression or simple classification tasks. For more complex cases, check detailed instructions on how to map inputs for [classification](classification_data.md) and [ranking and recommendations](recsys_data.md).

{% hint style="info" %} 
**Why map them:** If you have a dataset with ML inferences, it's necessary to map where Predictions and Ground Truth are to compute the quality metrics. You also need to map the Target and/or Prediction to generate the Target Drift Preset.
{% endhint %}

## Categorical and numerical columns

To split the columns into numerical and categorical types, pass them as lists: 

```python
column_mapping.numerical_features = ['temp', 'atemp', 'humidity'] 
column_mapping.categorical_features = ['season', 'holiday'] 
```

{% hint style="info" %} 
**Why map them:** Column types impact evaluations. For example, the data [drift algorithm](../reference/data-drift-algorithm.md) selects statistical tests based on column type, or `ColumnSummaryMetric` visualizations change with feature type. Manually mapping columns avoids errors, like numerical columns with few unique values being mistaken for categorical.
{% endhint %}

## Text data 

To specify columns that contain text data: 

```python
column_mapping.text_features = ['email_subject', 'email_body']
```

{% hint style="info" %} 
**Why map them:** Always map text columns to enable text-specific evaluations. This also ensures text columns are excluded from Tests and Metrics where they don’t apply. If you don’t map text columns, they’ll be treated as categorical, potentially leading to irrelevant evaluations like raw text distribution histograms.
{% endhint %}

## Embeddings features

To specify which columns in your dataset contain embeddings, pass a dictionary where keys are embedding names and values are lists of columns. 

Here is an example of how you point to the defined list of columns that contain embeddings:

```python
column_mapping = ColumnMapping()
column_mapping.embeddings = {'small_subset': embeddings_data.columns[:10]}
```

{% hint style="info" %} 
**Why map them:** To apply embeddings-specific data drift detection methods. 
{% endhint %}

## DateTime features 

You might have temporal features in your dataset. For example, “date of the last contact.” 
 
To map them, pass them as a list: 

```python
column_mapping.datetime_features = ['last_call_date', 'join_date'] 
```

{% hint style="info" %} 
**What is the difference between DateTime features and DateTime?** DateTime is a single timestamp column. It often represents the time when a data row was recorded. Use it if you want to see it as index on the plots. A DateTime feature is any time-related column in your dataset, such as input features in a ML model.
{% endhint %}
 
{% hint style="info" %} 
**Why map them:** DateTime feature will be ignored in data drift calculation. Evidently will also calculate appropriate stats and different visualizations for DateTime features in the `ColumnSummaryMetric`.
{% endhint %}

## Task parameter for target function

It’s often important to specify whether your Target column is continuous or discrete. This impacts Data Quality, Data Drift, and Target Drift evaluations for the Target column.
 
To define it explicitly, specify the task parameter:

```python
column_mapping.target = 'y'
column_mapping.task = 'regression'
```

It accepts the following values: 
* `regression`
* `classification`
* `recsys` (for ranking and recommenders)

**Default**: If you don't specify the task, Evidently will use a simple strategy: if the target has a numeric type and the number of unique values > 5: task == ‘regression.’ In all other cases, the task == ‘classification’.

{% hint style="info" %} 
**Why map it:** Classes encoded as numbers may sometimes look like continuous targets. Explicitly specifying the target type ensures the right visualizations and statistical tests for the target (prediction) are selected. 
{% endhint %} 
