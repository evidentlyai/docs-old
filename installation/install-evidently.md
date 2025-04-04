---
description: How to install the open-source Python library.
---

{% hint style="info" %}
**You are looking at the old Evidently documentation**. Check the newer version [here](https://docs.evidentlyai.com/introduction).
{% endhint %}

# Evidently 

`Evidently` is available as a Python package. To install it using the **pip package manager**, run:

```python
pip install evidently
```

To install `evidently` using **conda installer**, run:

```sh
conda install -c conda-forge evidently
```

# Evidently LLM

To run evaluations specific to LLMs that include additional dependencies, run:

```python
pip install evidently[llm]
```

# Tracely

To use tracing based on OpenTelemetry, install the sister package **tracely**:

```sh
pip install tracely
```
