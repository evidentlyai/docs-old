
{% hint style="info" %}
**You are looking at the old Evidently documentation**: this API is available with versions 0.6.7 or lower. Check the newer version [here](https://docs.evidentlyai.com/introduction).
{% endhint %}

In this tutorial, you will use the Evidently open-source Python library to evaluate **data stability** and **data drift** on tabular data. You will run batch checks on a toy dataset and generate visual Reports and Test Suites in your Python environment.

We recommend going through this tutorial once to understand the basic functionality. Once you complete it, you will be ready to use all Evidently evaluations, including checks for ML model quality or text data.  

You can later run the Evidently Reports and Test Suites independently or use them as a logging layer for Evidently ML Monitoring. You can choose between self-hosting [ML monitoring dashboard](tutorial-monitoring.md) or sending the Reports and Test Suite to [Evidently Cloud platform](tutorial-cloud.md) to monitor metrics over time.  

To complete the tutorial, you need basic knowledge of Python. You should be able to complete it in **about 15 minutes**.

You can reproduce the steps in Jupyter notebooks or Colab or open and run a sample notebook from the links below.  

Jupyter notebook:
{% embed url="https://github.com/evidentlyai/evidently/blob/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/sample_notebooks/getting_started_tutorial.ipynb" %}

Video version:
{% embed url="https://youtu.be/2d5AgPKbthE" %}

You will go through the following steps:
* Install Evidently
* Prepare the input data
* Generate a pre-built data drift report
* Customize the report 
* Run and customize data stability tests 

If you're having problems or getting stuck, reach out on [Discord](https://discord.com/invite/xZjKRaNp8b).

## 1. Install Evidently

### MAC OS and Linux

To install Evidently using the pip package manager, run:

```bash
$ pip install evidently
```

### Hosted notebooks

If you are using Google Colab, Kaggle Kernel, Deepnote or Databricks notebooks, run the following command in the notebook cell:

```
!pip install evidently
```

### Windows

To install Evidently in Jupyter notebook on Windows, run:

```bash
$ pip install evidently
```

## 2. Import Evidently

After installing the tool, import `evidently` and the required components. In this tutorial, you will use several **test suites** and **reports**. Each corresponds to a specific type of analysis. 
 
You should also import `pandas`, `numpy`, and the toy `california_housing` dataset.

```python
import pandas as pd
import numpy as np

from sklearn.datasets import fetch_california_housing

from evidently import ColumnMapping

from evidently.report import Report
from evidently.metrics.base_metric import generate_column_metrics
from evidently.metric_preset import DataDriftPreset, TargetDriftPreset
from evidently.metrics import *

from evidently.test_suite import TestSuite
from evidently.tests.base_test import generate_column_tests
from evidently.test_preset import DataStabilityTestPreset, NoTargetPerformanceTestPreset
from evidently.tests import *
```

## 3. Prepare the data

In the example, you will work with a toy dataset. In practice, you should use the model prediction logs. They can include input data, model predictions, and true labels or actuals, if available. 

To prepare the data for analysis, create a `pandas.DataFrame`:

```python
data = fetch_california_housing(as_frame=True)
housing_data = data.frame
```

Rename one of the columns to “target” and create a “prediction” column. This way, the dataset will resemble the model application logs with known labels. 

```python
housing_data.rename(columns={'MedHouseVal': 'target'}, inplace=True)
housing_data['prediction'] = housing_data['target'].values + np.random.normal(0, 5, housing_data.shape[0])
```

Split the dataset by taking 5000 objects for **reference** and **current** datasets.   

```python
reference = housing_data.sample(n=5000, replace=False)
current = housing_data.sample(n=5000, replace=False)
```

The first **reference** dataset is the baseline. This is often the data used in model training or earlier production data. The second dataset is the **current** production data. Evidently will compare the current data to the reference. 
 
If you work with your own data, you can prepare two datasets with an identical schema. You can also take a single dataset and explicitly identify rows for reference and current data.

{% hint style="info" %}
**Column mapping.** In this example, we directly proceed to analysis. In other cases, you might need a `ColumnMapping` object to help Evidently process the input data correctly. For example, you can point to the encoded categorical features or specify the name of the target column. Consult the [Column Mapping section](../input-data/column-mapping.md) section for help.
{% endhint %}

## 4. Get the Data Drift report

Evidently **Reports** help explore and debug data and model quality. They calculate various metrics and generate a dashboard with rich visuals. 

To start, you can use **Metric Presets**. These are pre-built Reports that group relevant metrics to evaluate a specific aspect of the model performance. 

Let’s start with the **Data Drift**. This Preset compares the distributions of the model features and show which have drifted. When you do not have ground truth labels or actuals, evaluating input data drift can help understand if an ML model still operates in a familiar environment.

To get the Report, create a corresponding `Report` object, list the `preset` to include, and point to the reference and current datasets created in the previous step: 

```python
report = Report(metrics=[
    DataDriftPreset(), 
])

report.run(reference_data=reference, current_data=current)
report
```

It will display the HTML report directly in the notebook. 

First, you can see the Data Drift summary.

![Data Drift report overview](../.gitbook/assets/tutorial/get_started_3_data_drift_summary-min.png)

If you click on individual features, it will show additional plots to explore. 

![Data Drift report details](../.gitbook/assets/tutorial/get_started_4_data_drift_expand-min.png)

<details>
<summary>How does it work?</summary>
 
The data drift report compares the distributions of each feature in the two datasets. It [automatically picks](../reference/data-drift-algorithm.md) an appropriate statistical test or metric based on the feature type and volume. It then returns p-values or distances and visually plots the distributions. You can also [adjust the drift detection method or thresholds](../customization/options-for-statistical-tests.md), or pass your own.
</details>

{% hint style="info" %}
**Aggregated visuals in plots.** Starting from v 0.3.2, all visuals in the Evidently Reports are aggregated by default. This helps decrease the load time and report size for larger datasets. If you work with smaller datasets or samples, you can pass an [option to generate plots with raw data](../customization/report-data-aggregation.md). You can choose whether you want it on not based on the size of your dataset.
{% endhint %}

## 5. Customize the Report

Evidently Reports are very configurable. You can define which Metrics to include and how to calculate them. 

To create a custom Report, you need to list individual **Metrics**. Evidently has dozens of Metrics that evaluate anything from descriptive feature statistics to model quality. You can calculate Metrics on the column level (e.g., mean value of a specific column) or dataset-level (e.g., share of drifted features in the dataset).  

In this example, you can list several Metrics that evaluate individual statistics for the defined column. 

```python
report = Report(metrics=[
    ColumnSummaryMetric(column_name='AveRooms'),
    ColumnQuantileMetric(column_name='AveRooms', quantile=0.25),
    ColumnDriftMetric(column_name='AveRooms')
])

report.run(reference_data=reference, current_data=current)
report
```
You will see a combined report that includes multiple Metrics:

![Part of the custom report, ColumnSummaryMetric.](../.gitbook/assets/tutorial/get-started-column-summary_metric-min.png)

If you want to generate multiple column-level Metrics, there is a helper function. For example, in order to calculate the same quantile value for all the columns in the list, you can use the generator: 

```
report = Report(metrics=[
    generate_column_metrics(ColumnQuantileMetric, parameters={'quantile':0.25}, columns=['AveRooms', 'AveBedrms']),
])

report.run(reference_data=reference, current_data=current)
report
```

You can easily combine individual Metrics, Presets and metric generators in a single list:

```
report = Report(metrics=[
    ColumnSummaryMetric(column_name='AveRooms'),
    generate_column_metrics(ColumnQuantileMetric, parameters={'quantile':0.25}, columns='num'),
    DataDriftPreset()
])

report.run(reference_data=reference, current_data=current)
report
```

{% hint style="info" %}
**Available Metrics and Presets**. You can refer to the All Metrics [reference table](../reference/all-metrics.md) to browse available Metrics and Presets or use one of the example notebooks in the [Examples](../examples/examples.md) section.
{% endhint %}

## 6. Define the Report output format

You can render the visualizations in the notebook as shown above. There are also alternative options. 

If you only want to log the metrics and test results, you can get the output as a Python dictionary.

```python
report.as_dict()
```
You can also get the output as JSON. 

```python
report.json()
```

You can also save HTML or JSON externally and specify a path and file name: 

```python
report.save_html("file.html")
```

You can also save the output as an Evidently JSON `snapshot`. This will allow you to visualize the model or data quality over time using the Evidently ML monitoring dashboard.

```python
report.save("snapshot.json")
```

{% hint style="info" %}
**Building a live ML monitoring dashboard**. To better understand how the ML monitoring dashboard works, we recommend going through the [ML Monitoring Quickstart](tutorial-monitoring.md) after completing this tutorial.
{% endhint %}


## 7. Run data stability tests

Reports help visually explore the data or model quality or share results with the team. However, it is less convenient if you want to run your checks automatically and only react to meaningful issues.

To integrate Evidently checks in the prediction pipeline, you can use the **Test Suites** functionality. They are also better suited to handle large datasets.

Test Suites help compare the two datasets in a structured way. A **Test Suite** contains several individual tests. Each **Test** compares a specific value against a defined condition and returns an explicit pass/fail result. You can apply Tests to the whole dataset or individual columns. 

Just like with Reports, you can create a custom Test Suite or use one of the **Presets**. 

Let's create a custom one! Imagine you received a new batch of data. Before generating the predictions, you want to check if the quality is good enough to run your model. You can combine several Tests to check missing values, duplicate columns, and so on. 

You need to create a `TestSuite` object and specify the `preset`:

```python
tests = TestSuite(tests=[
    TestNumberOfColumnsWithMissingValues(),
    TestNumberOfRowsWithMissingValues(),
    TestNumberOfConstantColumns(),
    TestNumberOfDuplicatedRows(),
    TestNumberOfDuplicatedColumns(),
    TestColumnsType(),
    TestNumberOfDriftedColumns(),
])

tests.run(reference_data=reference, current_data=current)
tests
```

You will get a summary with the test results:

![Part of the custom Test Suite.](../.gitbook/assets/tutorial/get-started-test-output-min.png)

<details>
<summary>How does it work?</summary>
 
Evidently automatically generates the test conditions based on the provided reference dataset. They are based on heuristics. For example, the test for column types fails if the column types do not match the reference. The test for the number of columns with missing values fails if the number is higher than in reference. The test for the share of drifting features fails if over 50% are drifting. You can easily [pass custom conditions](../tests-and-reports/custom-test-suite.md) to set your own expectations.
 
</details>

You can also use **Test Presets**. For example, `NoTargetPerformance` preset combines multiple checks related to data stability, drift and data quality to help evaluate the model without ground truth labels. 

```python
suite = TestSuite(tests=[
    NoTargetPerformanceTestPreset(),
])

suite.run(reference_data=reference, current_data=current)
suite
```

You can group the outputs by test status, feature, test group, and type. By clicking on “details,” you will see related plots or tables.

![Details on Mean Value Stability test](../.gitbook/assets/tutorial/get_started_2_mean_value_stability-min.png)

If some of the Tests fail, you can use the supporting visuals for debugging:

![Failed tests](../.gitbook/assets/tutorial/test-notargetperformance-min.png)

Just like with Reports, you can combine individual Tests and Presets in a single Test Suite and use column test generator to generate multiple column-level tests:

```python
suite = TestSuite(tests=[
    TestColumnDrift('Population'),
    TestShareOfOutRangeValues('Population'),
    generate_column_tests(TestMeanInNSigmas, columns='num'),
    
])

suite.run(reference_data=reference, current_data=current)
suite
```
{% hint style="info" %}
**Available Tests and Presets**. You can refer to the All Tests [reference table](../reference/all-tests.md) to browse available Tests and Presets. To see interactive examples, refer to the notebooks in the [examples](../examples/examples.md) section.
{% endhint %}

You can also export the output in other formats.

To integrate Evidently checks in the prediction pipeline, you can get the output as JSON or a Python dictionary: 

```python
suite.as_dict()
```

You can extract necessary information from the JSON or Python dictionary output and design a conditional workflow around it. For example, if tests fail, you can trigger an alert, retrain the model or generate the Report. 

You can also save the output as an Evidently JSON `snapshot`. This will allow you to visualize the test results over time the Evidently ML monitoring dashboard.

```python
suite.save("snapshot.json")
```

## 8. What should I do next?

* **Explore available evaluations**

In this tutorial, you explored some of the data quality and data drift checks on tabular data. Evidently also support evaluations on text data and model quality checks. 

The easiest way to understand what else is there is to look at **Presets**. Both Tests and Reports have multiple Presets. Some, like Data Quality, require only input data. You can use them even without the reference dataset. When you have the true labels, you can run Presets like **Regression Performance**, **Ranking Performance** and **Classification Performance** to evaluate the model quality and errors. 

To understand the contents of each Preset, head to the [Preset overview](../presets/all-presets.md). If you want to see the pre-rendered examples of the reports, browse Colab notebooks in the [Examples](../examples/examples.md) section. You can also design custom Reports and Test Suites from individual Metrics and Tests. 

* **Learn how to get a Monitoring Dashboard**
 
If you want to track the results of different checks over time, you get an ML monitoring dashboard. Go through the [ML monitoring quickstart (Evidently Cloud) - Recommended](tutorial-cloud.md) or [ML monitoring quickstart (Self-hosting)](tutorial-monitoring.md) to see how to monitor metrics over time.  

* **Explore available integrations**

To explore how to integrate Evidently with other tools, refer to the [Integrations](../integrations). For example, if you run predictions in batches, you can use a tool like [Airflow](../integrations/evidently-and-airflow.md) to orchestrate the process.

* **Go through the steps in more detail**

To better understand working with Reports and Test Suites, refer to the **User Guide** section of the docs. A good next step is to explore how to pass custom test parameters to define your own [test conditions](../tests-and-reports/custom-test-suite.md).  

## Join our Community!

Evidently is in active development, so expect things to change and evolve. You can subscribe to the [user newsletter](https://www.evidentlyai.com/user-newsletter) or follow our [releases on GitHub](https://github.com/evidentlyai/evidently/releases) to stay updated about the latest functionality. 

We run a [Discord community](https://discord.gg/xZjKRaNp8b) to connect with our users and chat about ML in production topics. 

In case you have feedback or need help, just ask in Discord or open a GitHub issue. 

And if you want to support a project, give us a star on [GitHub](https://github.com/evidentlyai/evidently)!
