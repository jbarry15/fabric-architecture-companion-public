# Anti-Pattern: Untracked ML Experiments & Unregistered Models

## What it looks like

- Training models in notebooks with no experiment tracking — parameters, metrics, and artifacts live only in cell output
- Saving trained models as loose pickle/joblib files in a Lakehouse folder
- Scoring references a hand-copied model file rather than a versioned, governed artifact
- No way to reproduce, compare, or roll back a model

## Why it's a problem

- **No reproducibility** — Without logged parameters and metrics you can't recreate or explain a result.
- **No comparison** — You can't tell which run/model is best across experiments.
- **No governance or lineage** — Loose files have no versioning, ownership, or auditability, and don't inherit OneLake/Purview governance.
- **Fragile serving** — Batch scoring against a copied file breaks silently when the file changes or moves; there's no promotion path.

## What to do instead

✅ **Track with MLflow** — Fabric has [built-in MLflow](https://learn.microsoft.com/en-us/fabric/data-science/machine-learning-experiment); enable [autologging](https://learn.microsoft.com/en-us/fabric/data-science/mlflow-autologging) so every run captures parameters, metrics, and artifacts automatically.

✅ **Register models in the model registry** — version [ML models](https://learn.microsoft.com/en-us/fabric/data-science/machine-learning-model) via MLflow so scoring references a governed, versioned artifact.

✅ **Use current MLflow artifacts deliberately** — log models, prompts/traces, metrics, and GenAI evaluation artifacts where relevant; don't treat notebook output as the system of record.

✅ **Score the registered model with `PREDICT`** — the scalable [universal Spark PREDICT](https://learn.microsoft.com/en-us/fabric/data-science/model-scoring-predict) runs registered MLflow models over big data; write predictions to OneLake as Delta.

✅ **Manage lifecycle** — put notebooks and ML artifact definitions/metadata under Git and promote Dev → Test → Prod. Experiment runs and model versions still need MLflow/model-registry governance; source control doesn't replace them.

## When minimal tracking is OK (Silver pattern)

- Throwaway exploration or a one-off analysis that will never be operationalized
- Early prototyping before a model is chosen — but register once it heads toward production

## Severity

🔴 **High** — Unreproducible, ungoverned models are a serious operational and compliance risk in production.

## References

- [Machine learning experiments (MLflow)](https://learn.microsoft.com/en-us/fabric/data-science/machine-learning-experiment)
- [MLflow autologging](https://learn.microsoft.com/en-us/fabric/data-science/mlflow-autologging)
- [Machine learning models (registry)](https://learn.microsoft.com/en-us/fabric/data-science/machine-learning-model)
- [Model scoring with PREDICT](https://learn.microsoft.com/en-us/fabric/data-science/model-scoring-predict)
- [ML artifacts in Git and deployment pipelines](https://learn.microsoft.com/en-us/fabric/data-science/machine-learning-artifacts-git-deployment-pipelines)
