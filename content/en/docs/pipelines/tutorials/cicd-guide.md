+++
title = "Implement A Continuous Integration Process With the Kubeflow Pipelines"
description = "How to implement a continuous integration process with the Kubeflow Pipelines"
weight = 10
+++

There are many ways to implement a continuous integration process with the
Kubeflow Pipelines. This guide shows one approach that leverages Google Cloud
Build, the Kubeflow Pipelines, and an example pipeline of [house price prediction](https://github.com/kubeflow/pipelines/tree/master/samples/contrib/versioned-pipeline-ci-samples/kaggle-ci-sample).

## Background

The [house price prediction](https://www.kaggle.com/c/house-prices-advanced-regression-techniques/data)
is an entry level competition at Kaggle. We have prepared a [sample pipeline](https://github.com/kubeflow/pipelines/tree/master/samples/contrib/versioned-pipeline-ci-samples/kaggle-ci-sample#pipeline-overview)
to how a competitor can use the Kubeflow Pipelines to facilitate their work.

This sample pipeline has the following steps: downloads training data and test
data from Kaggle website, processes and visualizes data, trains model and
submits results to Kaggle website. Assume the initial version of this sample
pipeline manifest is stored in Github repository. The competitor can upload this
sample pipeline to the Kubeflow Pipelines instance. When the competitor has an
improved model, they can update the sample pipeline manifest in Github
repository. The new version of the sample pipeline in Github repository will be
automatically uploaded to the Kubeflow Pipelines instance and automatically run
by the Kubeflow Pipelines, and in the end, the updated results will be
automatically submitted to the Kaggle website. In other words, the automatic
process from uploading new versions of the sample pipeline to submitting the
updated results to the Kaggle website is a particular use case of continuous
integration with the Kubeflow Pipelines.

The following example shows how to set up a continuous integration using the
Kubeflow Pipelines and Google Cloud Build. To try this example out, we assume
our users have the basic understanding of Github, Google Cloud Build and the
Kubeflow Pipelines.

## An Example of Setting Up A Continuous Integration With The House Price Prediction Pipeline

### 1. Create Your Own Fork of Pipelines Repository

- Our sample pipeline of housing price prediction is stored at
kubeflow/pipelines repository. To try this example of a continuous integration
with this sample pipeline, you can first create your own fork from
kubeflow/pipelines. Assume that your user name with Github is \<user\>, the fork
will then be \<user\>/pipelines.
- When you have your own fork, you can create a branch other than the master
branch, say ci_sample. And later on, any updates to the sample pipeline will be
made on this branch.

### 2. Set up Google Cloud Build Trigger

[Google Cloud Build](https://cloud.google.com/cloud-build/docs) supports
automated builds using triggers. In particular, it supports Github triggers,
e.g., the event of pushing changes to a certain Github branch can trigger a
build in Google Cloud Build.











