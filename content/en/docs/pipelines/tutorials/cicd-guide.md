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

## Continuous integration example with the house price prediction pipeline

### 1. Prerequisites

You'll need to
- Create your own Google Cloud Project.
- Under your project, create a GKE cluster and deployment the Kubeflow Pipelines
to it according to our [standalone deployment guide](https://www.kubeflow.org/docs/pipelines/installation/standalone-deployment/).
Please use the default namespace *kubeflow* for the purpose of this example.
- Create your own fork from kubeflow/pipelines.
- Clone your fork to your local workstation.
- Create a local git branch, say ci_example. And later on, any updates to the
sample pipeline will be made on this branch.
- Loadbalancer for ml-pipeline server.
- On your local workstation
    - Install [Docker](https://www.docker.com/get-started).
    - Install the [Kubeflow Pipelines SDK](https://www.kubeflow.org/docs/pipelines/sdk/install-sdk/).

### 2. Compile and upload the initial house price prediction pipeline version

- On your local ci_example branch, [compile](https://www.kubeflow.org/docs/pipelines/sdk/build-component/#compile-the-pipeline) the House Price Prediction pipeline. The source file of House Price Predication pipeline is [samples/contrib/versioned-pipeline-ci-samples/kaggle-ci-sample/pipeline.py](https://github.com/kubeflow/pipelines/blob/master/samples/contrib/versioned-pipeline-ci-samples/kaggle-ci-sample/pipeline.py).

### 3. Set up Google Cloud Build trigger

[Google Cloud Build](https://cloud.google.com/cloud-build/docs) supports
automated builds using triggers. In particular, it supports Github triggers,
for example, the event of pushing changes to a certain Github branch can trigger a
build in Google Cloud Build. Therefore, if we assume the updates to the pipeline
manifest will take place on the branch ci_sample, we can have set a trigger on
the push event on this branch. To set up a github trigger, two simple steps are
needed.

- Connect to a repository. That is, connect to the previous repository you just
forked, \<user\>/pipelines. The following example shows how to connect to a
repository jingzhang36/pipelines in Google Cloud Build. You can use your own
forked repository in the place of jingzhang36/pipelines.
<video width="100%" max-width="100%" height="auto" max-height="100%" controls>
  <source src="/docs/videos/connect_repo.webm" type="video/webm">
</video>
- Create a trigger.
- Give Cloud Build Service account the owner permission to cluster

