# [Experiment tracking](https://drive.google.com/file/d/1YtkAtOQS3wvY7yts_nosVlXrLQBq5q37/view)

## 1. Experiment tracking

### 1.1 Definitions

- ML experiment: the process of building an ML model
- Experiment run: each trial in an ML experiment
- Run artifact: any file that associated with a specific ML run
- Experiment data: all the information related to overall experiment

### 1.2 What's experiment tracking

Experiment tracking is the process of keeping track of all the relevant information
from an ML experiment, which includes:

- Source code
- Environment
- Data
- Model
- Hyperparameter
- Metrics
- ...

### 1.3 Why is experiment tracking so important?

- Reproducibility
- Organization
- Optimization

### 1.4 Tracking experiment in spreadsheets

Why it is not enough:

- Error prone
- No typical standard format
- Visibility and collaboration

## 2. MLflow

### 2.1 What is MLflow?

In reality it's just a pip-installable Python package that contains four modules:

- Tracking
- Models
- Model registry
- Project

More details: [official documentation](https://mlflow.org/docs/latest/introduction/index.html)

### 2.2 MLflow tracking

The MLflow Tracking module allows you to organize your experiments into runs, and to keep track of:

- Parameters: any information that have an effect on the model performance like: hyperparameter, path to the training dataset, preprocessing to the input data, ..
- Metrics
- Metadata: any information doesn't have effect on the model performance like: tags, or developer name to easily search and filter runs
- Artifacts: any files associated to a run, like: visualization, ...
- Models

Along with this information, MLflow automatically logs extra information about the run:

- Source code
- Version of the code (git commit)
- Start and end time
- Author

### 2.3 Experiment tracking with MLflow

```python
with mlflow.start_run():
    mlflow.set_tag("developer", "giang")
    mlflow.log_param("train-data-path", "../data/green_tripdata_2021-01.parquet")
    mlflow.log_param("valid-data-path", "../data/green_tripdata_2021-02.parquet")

    alpha = 0.1
    mlflow.log_param("alpha", alpha)

    lr = Lasso(alpha)
    lr.fit(X_train, y_train)

    y_pred = lr.predict(X_val)

    rmse = root_mean_squared_error(y_val, y_pred)
    mlflow.log_metric("rmse", rmse)
```

Autolog will allow you to "log metrics, parameters, and models without the need for explicit log statements". However this is currently supported for a few libraries (but most of the ones you would normally use)

### 2.4 Model management (Saving and Loading models with MLflow)

#### 2.4.1 Machine learning lifecycle

Recommended blog: [Neptune.ai model management blog post](https://neptune.ai/blog/machine-learning-model-management)

#### 2.4.2 Model management with folders system

It's not efficient due to:

- Error prone: accidentally override an old models, copy and paste the wrong models, ...
- No versioning: the folder's name isn't a efficient and clear versioning when the growing the number of models
- No model lineage: it's not easy to understand how the model created (hyperparameter, training data, test data, ..)

### 2.4.3 Model management with MLflow

#### 2.4.3.1 MLflow log_artifact

```python
mlflow.log_artifact(local_path="path/to/model.bin", artifact_path="folder/for/models/")
```

Then, you can download model in MLflow UI and use it.

#### 2.4.3.2 MLflow log_model

```python
mlflow.xgboost.log_model(booster, artifact_path="./path/to/artifact/")
```

**Note** Saving model with `log_model` is more efficient than `log_artifact`. `log_model` allows MLflow to stores more information being relevant to the saved model. So, we can load model more easily.

You are also able to log any pre-processing steps as an artifact

```python
# Save the dictionary vectoriser
with open("models/preprocessor.b", "wb") as f_out:
    pickle.dump(dv, f_out)
mlflow.log_artifact("models/preprocessor.b", artifact_path="preprocessor")
```

#### 2.4.3.3 Loading model with MLflow

```python
xgb_model = mlflow.xgboost.load_model(model_URI)
```

### 2.5 Model registry

How can we take a model and run it in production environment? But, if there is new model to be deployed, what do we need to know?

- Model lineage
- Any new preprocessing?
- New set of dependencies?

Model registry is a centralized model store that save all production-ready models. It provides:

- Model lineage
- Model versioning
- Model aliasing and tagging
- Annotations

Roles:

- Data scientist decides which model is ready for production and registers the models.
- MLOps engineer inspects the registered model and move this model to the appropriate stage

Model registry is not responsible for deploy model. It just stores all production-ready models into appropriate stages.

#### 2.5.1 Registering and transitioning models with MLflow UI

[Details](https://mlflow.org/docs/latest/model-registry.html#ui-workflow)

#### 2.5.2 Registering and transitioning models with MLClient class

[Adding an MLflow Model to the Model Registry](https://mlflow.org/docs/latest/model-registry.html#adding-an-mlflow-model-to-the-model-registry)

### 2.6 MLflow in Practice

Each scenario will have different requirements

- Single Data Scientist participating in an ML competition

  - Can store everything locally.
  - Sharing between others is not necessary.
  - No model registry needed because the model isn't going to deployment.

- A Cross-functional team with one Data Scientist working on an ML model

  - Need to share the exp information.
  - No specific need to run the trafficking server remotely. It might be ok to be run on the local computer.
  - Using the model registry would probably be a good idea (remotely/locally).

- Multiple Data Scientists working on multiple models

  - Here collaboration is vital.
  - Remote tracking is vital, as multiple people will contribute to a single experiment.
    The model registry will be vital.

These three examples will serve as examples for each situation. And your MLflow set up will change depending what your needs are.But broadly there are three things you need to consider

- Backend Store: Where MLflow stores all the metadata: params, metrics, tags, .... By default, it will store it locally

  - Local file system:
  - SQLAlchemy compatible database (e.g. SQLite)

- Artifact Storage: Images, model, etc. Default is locally

  - Store artifacts locally?
  - Remotely (e.g. S3 bucket)

- Tracking Server: The mlflow ui etc. to capture the data. If you are just working on your own this is probably not necessary
  - None
  - Localhost
  - Remote

Check `running-mlflow-examples` folders for more details.

### 2.7 MLflow: benefits, limitations, and alternatives

#### 2.7.1 Benefits

The tracking server can be easily deployed to the cloud. Some benefits:

- Share experiments with other data scientists
- Collaborate with others to build and deploy models
- Give more visibility of the data science efforts

#### 2.7.2 Issues with running a remote (shared MLflow server)

Security

- Restrict access to the server (e.g. access through VPN)

Scalability

- Check [Deploy MLflow on AWS Fargate](https://github.com/aws-samples/amazon-sagemaker-mlflow-fargate)
- Check MLflow at Company Scale by Jean-Denis Lesage

Isolation

- Define standard for naming experiments, models and a set of default tags
- Restrict access to artifacts (e.g. use s3 buckets living in different AWS accounts)

#### 2.7.3 Limitations

- Authentication & Users: The open source version of MLflow doesn’t provide any sort of authentication

- Data versioning: to ensure full reproducibility we need to version the data used to train the model. MLflow doesn’t provide a built-in solution for that but there are a few ways to deal with this limitation

- Model/Data Monitoring & Alerting: this is outside of the scope of MLflow and currently there are more suitable tools for doing this

#### 2.7.4 Alternatives

- Neptune
- Comet
- Weights & Biases
- [Many more](https://neptune.ai/blog/best-ml-experiment-tracking-tools)

### 3. Read more

[Comprehensive notes](https://github.com/mleiwe/mlops-zoomcamp/blob/Ch2_Marcus/cohorts/2024/02-experiment-tracking/Ch2_notes.md)
