---
description: Overview of the available Evidently integrations.
---

{% hint style="info" %}
**You are looking at the old Evidently documentation**: this API is available with versions 0.6.7 or lower. Check the newer version [here](https://docs.evidentlyai.com/introduction).
{% endhint %}

Evidently is a Python library, and can be easily integrated with other tools to fit into the existing workflows.

Below are a few specific examples of how to integrate Evidently with other tools in the ML lifecycle. You can adapt them for other workflow management, visualization, tracking and other tools.

| Tool | Description | Guide or example |
|---|---|---|
| Notebook environments (Jupyter, Colab, etc.) | Render visual Evidently Reports and Test Suites. | [Docs](notebook-environments.md)<br>[Code examples](../examples/examples.md) |
| Streamlit | Create a web app with Evidently Reports.  | [Tutorial](https://www.evidentlyai.com/blog/ml-model-monitoring-dashboard-tutorial)<br> [Code example](https://github.com/evidentlyai/evidently/tree/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/integrations/streamlit_dashboard)|
| MLflow | Log metrics calculated by Evidently to MLflow. | [Docs](evidently-and-mlflow.md)<br>[Code example](https://github.com/evidentlyai/evidently/blob/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/integrations/mlflow_logging/mlflow_integration.ipynb) |
| DVCLive | Log metrics calculated by Evidently to DVC. | [Docs](evidently-and-dvclive.md)<br>[Code example](https://github.com/evidentlyai/evidently/blob/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/integrations/dvclive_logging/dvclive_integration.ipynb) |
| Airflow | Run data and ML model checks as part of an Airflow DAG. | [Docs](evidently-and-airflow.md)<br>[Code example](https://github.com/evidentlyai/evidently/tree/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/integrations/airflow_drift_detection) |
| Metaflow | Run data and ML model checks as part of a Metaflow Flow. | [Docs](evidently-and-metaflow.md) |
| FastAPI + PostgreSQL| Generate on-demand Reports for models deployed with FastAPI.  | [Tutorial](https://www.evidentlyai.com/blog/fastapi-tutorial)<br>[Code example](https://github.com/evidentlyai/evidently/tree/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/integrations/fastapi_monitoring) |
| Grafana + PostgreSQL + Prefect | Run ML monitoring jobs with Prefect and visualize metrics in Grafana.  | [Tutorial](https://www.evidentlyai.com/blog/batch-ml-monitoring-architecture)<br>[Code example](https://github.com/evidentlyai/evidently/tree/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/integrations/postgres_grafana_batch_monitoring/) |
| AWS SES | Send email alerts with attached Evidently Reports (Community contribution). | [Tutorial](https://www.evidentlyai.com/blog/ml-monitoring-with-email-alerts-tutorial)<br>[Code example](https://github.com/evidentlyai/aws_alerting)
| Grafana | Real-time ML monitoring with Grafana. (Old API, not currently supported). | [Code example](https://github.com/evidentlyai/evidently/tree/ad71e132d59ac3a84fce6cf27bd50b12b10d9137/examples/integrations/grafana_monitoring_service) |
