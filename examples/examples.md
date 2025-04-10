---
description: Code examples and tutorials.
---

{% hint style="info" %}
**You are looking at the old Evidently documentation**: this API is available with versions 0.6.7 or lower. Check the newer version [here](https://docs.evidentlyai.com/introduction).
{% endhint %}

# Quick Start

Check the short Quickstart examples [here](/get-started/README.MD).

# Get Started Tutorials

Introductory tutorials that walk you through the basic functionality step by step.

Title| Guide | Code
-- | -- | --
LLM Evaluation | [Tutorial](tutorial-llm.md) | [Jupyter notebook](https://github.com/evidentlyai/evidently/blob/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/sample_notebooks/llm_evaluation_tutorial.ipynb)
Data & ML Monitoring | [Tutorial](tutorial-cloud.md) | [Jupyter notebook](https://github.com/evidentlyai/evidently/blob/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/sample_notebooks/data_and_ml_monitoring_tutorial.ipynb)
LLM Tracing| [Tutorial](tutorial_tracing.md) | [Jupyter notebook](https://github.com/evidentlyai/evidently/blob/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/sample_notebooks/llm_tracing_tutorial.ipynb) 
Intro to Reports & Test Suites (OSS) | [Tutorial](tutorial_reports_tests.md) | [Jupyter notebook](https://github.com/evidentlyai/evidently/blob/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/sample_notebooks/getting_started_tutorial.ipynb)
Self-host ML monitoring Dashboard (OSS)| [Tutorial](tutorial-monitoring.md) | [Jupyter notebook](https://github.com/evidentlyai/evidently/blob/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/sample_notebooks/get_started_monitoring.py)


# Example Reports and Tests 

Simple examples show different local evaluations (Metrics, Tests and Presets) for tabular data and ML. 

Title| Code example | Contents
-- | -- | --
Evidently Test Presets| [Jupyter notebook](https://github.com/evidentlyai/evidently/blob/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/sample_notebooks/evidently_test_presets.ipynb)| Pre-built Test Suites on tabular data: <ul><li>Data Drift</li><li>Data Stability</li><li> Data Quality</li><li>NoTargetPerformance</li><li>Regression</li><li>Classification (Multi-class, binary, binary top-K)</li></ul>    
Evidently Tests| [Jupyter notebook](https://github.com/evidentlyai/evidently/blob/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/sample_notebooks/evidently_tests.ipynb) <br>  | <ul><li>All individual Tests (50+) that one can use to create a custom Test Suite. Tabular data examples.</li><li>How to set test conditions and parameters.</li></ul>
Evidently Metric Presets| [Jupyter notebook](https://github.com/evidentlyai/evidently/blob/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/sample_notebooks/evidently_metric_presets.ipynb)  | All pre-built Reports: <ul><li>Data Drift</li><li>Target Drift</li><li>Data Quality</li><li>Regression</li><li>Classification</li></ul>     
Evidently Metrics| [Jupyter notebook](https://github.com/evidentlyai/evidently/blob/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/sample_notebooks/evidently_metrics.ipynb) | <ul><li>All individual metrics (30+) that one can use to create a custom Report.</li><li>How to set simple metric parameters.</li></ul>
Evidently LLM Metrics| [Jupyter notebook](https://github.com/evidentlyai/evidently/blob/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/how_to_questions/how_to_evaluate_llm_with_text_descriptors.ipynb) | <ul><li>Evaluations for Text Data and LLMs</li></ul>

For LLM and text metrics, check the [LLM evaluation tutorial](tutorial-llm.md). 

# Tutorials - LLM

Title | Tutorial
--- | --- 
How to create LLM judge evaluator | [Tutorial](cookbook_llm_judge.md)
How to run regression testing for LLM products| [Tutorial](cookbook_llm_regression_testing.md)

# Tutorials - ML

To better understand the Evidently use cases, refer to the **detailed tutorials** accompanied by the blog posts.

Title | Code example | Blog post 
--- | --- | --- 
Understand ML model decay in production (regression example) | [Jupyter notebook](https://github.com/evidentlyai/community-examples/blob/main/tutorials/bicycle_demand_monitoring.ipynb) | [How to break a model in 20 days. A tutorial on production model analytics.](https://evidentlyai.com/blog/tutorial-1-model-analytics-in-production) 
Compare two ML models before deployment (classification example) | [Jupyter notebook](https://github.com/evidentlyai/community-examples/blob/main/tutorials/ibm_hr_attrition_model_validation.ipynb) | [What Is Your Model Hiding? A Tutorial on Evaluating ML Models.](https://evidentlyai.com/blog/tutorial-2-model-evaluation-hr-attrition) 
Evaluate and visualize historical data drift | [Jupyter notebook](https://github.com/evidentlyai/evidently/blob/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/integrations/mlflow_logging/historical_drift_visualization.ipynb) | [How to detect, evaluate and visualize historical drifts in the data.](https://evidentlyai.com/blog/tutorial-3-historical-data-drift) 
Monitor NLP models in production | [Colab](https://colab.research.google.com/drive/15ON-Ub_1QUYkDbdLpyt-XyEx34MD28E1) | [Monitoring NLP models in production: a tutorial on detecting drift in text data](https://www.evidentlyai.com/blog/tutorial-detecting-drift-in-text-data) 
Create ML model cards |[Jupyter notebook](https://github.com/evidentlyai/community-examples/tree/main/tutorials/How_to_create_an_ML_model_card.ipynb) | [A simple way to create ML Model Cards in Python](https://www.evidentlyai.com/blog/ml-model-card-tutorial) 
Use descriptors to monitor text data | [Jupyter notebook](https://github.com/evidentlyai/community-examples/tree/main/tutorials/How_to_add_a_custom_text_descriptor.ipynb) | [Monitoring unstructured data for LLM and NLP with text descriptors](https://www.evidentlyai.com/blog/unstructured-data-monitoring)|  

You can find more examples in the [Community Examples](https://github.com/evidentlyai/community-examples) repository. 

## How to examples

For code examples on specific functionality, check the How-To examples:

{% content-ref url="https://github.com/evidentlyai/evidently/tree/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/how_to_questions" %}
[How to guides]([../how-to-guides/README.md](https://github.com/evidentlyai/evidently/tree/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/integrations))
{% endcontent-ref %}

## Integrations

To see how to integrate Evidently in your prediction pipelines and use it with other tools, refer to the **integrations**.

{% content-ref url="../integrations/evidently-integrations.md" %}
[integrations/evidently-integrations.md](../integrations/evidently-integrations.md)
{% endcontent-ref %}
