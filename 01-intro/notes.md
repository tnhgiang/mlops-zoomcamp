# 1. Introduction

## 1.1 What is MLOps

`MLOps`, short for Machine Learning Operations, refers to the practices, methodologies, and tools used to streamline and operate the lifecycle of machine learning (ML) models. It aims to bridge the gap between data scientists, who develop and train models, and production environments, where models are deployed and maintained.

Traditionally, the development and deployment of ML models have been treated as separate stages, leading to challenges when transitioning from experimentation to production. MLOps seeks to address these challenges by introducing a set of best practices for managing ML workflows, collaboration, and automation.

By adopting MLOps practices, organizations can enhance the efficiency, scalability, and reliability of their ML deployments. It promotes a systematic approach to managing ML models, accelerates time to market, and facilitates collaboration between different stakeholders involved in ML development and deployment.

Three levels of change: Data, Model, and Code

![ThreeLevelsOfChanges](./assets/ThreeLevelsOfChange.jpg)

**Read more**:

- [Comprehensive-MLOps](https://ntp3105.github.io/Comprehensive-MLOps/Week-1/Introduction%20to%20MLOps.html)
- [Google cloud](https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning#devops_versus_mlops)

## 1.2 Steps in Machine Learning Project

1. Design: Do we need to use machine learning model to solve the problems?
2. Model: Train and optimize the model.
3. Operate: Model deployment, management and monitoring.

## 1.2 Environment

- Install python package manager: Anaconda, poetry, pyenv, ...
- Docker

## 1.4 Course overview

## 1.5 MLOps maturity model

Reference: [MLOps Maturity Model: Microsoft Docs](https://docs.microsoft.com/en-us/azure/architecture/example-scenario/mlops/mlops-maturity-model)

| Level | Description             | Overview                                                                                                                                                                                                                                                             | When Should You Use?                                                                                                                                                                                        |
| ----- | ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0️⃣    | No Automation 😢        | <ul><li>All code in Jupyter Notebook</li><li>No pipeline, experiment tracking, and metadata</li> </ul>                                                                                                                                                               | <ul><li>Academic projects</li><li>Proof of Concept is the end goal, not production-ready models</li></ul>                                                                                                   |
| 1️⃣    | Yes! DevOps😀, No MLOps | <ul><li>Best engineering practices followed</li><li>Automated releases</li><li>Unit \& Integration Tests</li><li>CI/CD pipelines</li><li>No experiment tracking and reproducibility</li><li>Good from engineering standpoint, models are not ML-aware yet!</li></ul> | <ul><li>Moving from proof of concept to production</li><li>When you need some automation</li><ul>                                                                                                           |
| 2️⃣    | Automated Training 🛠    | <ul><li>Training pipelines</li><li>Experiment tracking</li><li>Model registry (track of currently deployed models)</li><li>Data scientists work in tandem with the engineering team</li><li>Low friction deployment</li></ul>                                        | <ul><li>When you have increasing number of use cases</li><li>Three or more use cases, you should definitely consider automating!</li><ul>                                                                   |
| 3️⃣    | Automated Deployment 💬 | <ul><li>Model deployment simplified!</li><li>Prep data >> Train model >> Deploy model</li><li>A/B testing</li><li>Model X: v1, v2 >> v2 is deployed; how to ensure v2 performs better?</li><li>Model monitoring</li></ul>                                            | <ul><li>Multiple use cases</li><li>More mature + important use cases</li><ul>                                                                                                                               |
| 4️⃣    | Full MLOps Automation ⚙ | <ul><li>Automated training</li><li>Automated retraining</li><li>Automated deployment</li></ul>                                                                                                                                                                       | <ul><li>Check if level 2, 3 won't suffice</li><li>Should model retraining and deployment be automated as well?</li><li>Super important to take a pragmatic decision! Do you really need level 4?😄</li><ul> |
