
# Table of Contents

* TOC
{:toc}

# Overview

This document outlines the key requirements for an enterprise data science platform. In order to support the growing business demand for new and advanced services that are based on ML, data science, data engineering and DevOps teams are looking for ways to increase productivity and accelerate the time it takes to deliver machine learning models all the way from concept to production. Ideally they would like to focus on writing the code and the business logic for their services rather than dealing with building the infrastructure that supports it. This is where an enterprise data science platform plays a part by providing all the relevant building blocks for running ML pipelines all the way from data collection through data preparation, training, deployment and model monitoring. Eventually more and more customers understand the need for building a machine learning factory which automates the heavy lifting of building machine learning pipelines and microservices.


This doc reflects in a very detailed way the various pieces that are needed for such a “factory”. Note that these requirements are not specific to the Iguazio Data Science Platform and they are based on a list of requirements made by our customers. It also outlines the requirements from the point of view of the various stakeholders: Data Scientists, Data Engineers, and DevOps.

# Requirements
## Common API for machine learning models
### Implementation Details
- The output is based on task type (classification, regression, multi-label, seq2seq)
- API should support a form of versioning  
- Underlying prediction values should be available if masked by labels, argmax, etc...

### Stakeholder Benefits
- As a data scientist, I want a common API, so that I can quickly create inference output for my models, which automatically implements production best practices agreed on by the group
- Also, as a data engineer, I want a common API to to allow seamless and stable communication between machine learning models and microservices in my big data pipelines 
-  As a software engineer, I want a common API so that I can support data scientists by providing model output serialization in a SDK and so that my applications can easily consume machine learning predictions
- As a technical manager, I want a common API so that my project teams spend less time fixing breaking jobs and writing custom logic

## Managed notebook server
### Implementation Details
- Provides authenticated access to main data stores
- Can provision infrastructure through a UI (GPU, CPU, RAM, Disk)
- Can scale underlying infrastructure to support large and small workloads
- Separates different users’ workspaces with managed identity built-in
- Supports Docker container-based workspaces
- Has access to a container registry with pre-built workspaces 
- Can easily share notebooks and workspaces between users
- Supports version control for notebooks
- Can scale to zero when not in use
- Allows for budgets and alerts around infrastructure costs to prevent overspending
- Can automatically shutdown idle infrastructure to prevent overspending
- Supports open source notebook server Jupyter to improve export-ability of notebooks and allows for community plug-ins
- Has access to a component registry with best-practice components already implemented for many tasks 

### Stakeholder Benefits
- As a data scientist, I want a notebook server, so that I can focus on analyzing data and modeling, reproduce my peers’ work, easily access internal software tools and popular ML/Analytics libraries “out of the box”, and start working day one — don’t have to worry about infrastructure, overspending or dependencies breaking
- As a data engineer, I want a notebook server, so that data isn’t being downloaded to local workstations and out of the cloud incurring high costs, I can easily see who is using what data sources in what volume to help optimize access, and I only have to add data source access to one place
- As a machine learning engineer, I want a notebook server, so that running computationally demanding experiments is as easy as increasing the number of GPUs and selecting a workspace image which includes parallel training software
- As a technical manager, I want a notebook server, so that I have transparency into costs and the ability to budget across teams based on the project they’re working on
- As a devops engineer, I want a notebook server, so that there is a single location for controlling permissions, infrastructure provisioning, and that leverages containers to minimize manual server configuration

## Data science workspace container registry 
### Implementation Details
- Contains base images which are hardened and use security best practices
- Has images with latest internal data science software pre-installed
- Has lightweight workspace containers for simple notebooks
- Has workspaces with popular open source ML libraries (Tensorflow, Pandas, Plotly) installed and configured
- Namespaces for individual data scientists to store custom workspace images with plug-ins, themes, and specific libraries
- Strict tagging enforcement based on CI/CD build ID 

### Stakeholder Benefits
- As a data scientist, I want a workspace registry, so that I can leverage the newest internal software, reproduce my own work as well as my peers’ work, spend more time experimenting instead of installing potentially complicated software
- As a DevOps engineer, I want a workspace registry, so that I know all work is being done in a secure environment
- As a software engineer, I want a workspace registry, so that I can deliver the most up to date stable version of data science software I’ve developed with confidence that all dependencies (including operating system level) are installed
- As a machine learning engineer, I want a workspace registry, so that I can create workspace images that have all necessary optimizations to work with underlying host infrastructure when running Tensorflow, XGBoost, etc.
- As a technical manager, I want a workspace registry, so that I can request any analysis or experiment be reproduced exactly (or parameters changed) based on business partner requests and when new hire ramp-up time is dramatically decreased

## Production training platform
### Implementation Details
- Has access to production versions of all data sources approved for data science modeling
- Is restricted to only service account triggering — cannot be triggered without review
- Supports pull request, merge to master trigger to start a production training job — ensuring governance, tests pass, build succeeds
- Supports multi-container workflows (preprocessing vs. training containers) and supports different infrastructure (CPU vs. GPU) for those steps
- Able to scale out over multi-CPU/GPU automatically without additional code from data scientists — configuration option
- Has an easy way to define complicated pipelines between different components (DAGs)
- Uniquely identifies pipelines and runs of those pipelines for reproducibility of a production training job
- Has predefined pipeline templates that allow components to be swapped out for customization
- Has access to a component registry with best-practice components already implemented for many tasks 
- Provides concurrent runs for hyper-parameter tuning in parallel
- Has access to the test sets for projects which allows for blinding the data scientists
- Saves models and other artifacts from pipeline runs to versioned stores with metadata for searching automatically without additional code from the data scientists
- Provides access to data on model training internals, explanations, metrics, and metadata (training dataset name/version, environment build, etc.)
- Can be run on preemptible (spot) hardware for reduced costs on long running/expensive jobs
- Supports data set versioning for different types of underlying data: files and tabular
- Allows users to compare candidate production models

### Stakeholder Benefits
- As a data scientist, I want a production training platform, so that I can scale up a few promising experiments using the same code in components, take advantage of prebuilt pipelines, analyze training results for many hyperparameter settings, follow best practices around not seeing the test set, and trigger retraining with ease
- As a data engineer, I want a production training platform, so that I can prioritize access to data sources for the training platform over the experimentation platform and also across different projects, potentially even provide scheduling for data source intensive jobs
- As a DevOps engineer, I want a production training platform, so that expensive training jobs must be approved by Git reviewers, data scientists don’t need to know anything about how to provision infrastructure, tests and build success are mandated for training code, and cost is minimized by supporting “by-component” compute
- As an MLOps engineer, I want a production training platform, so that I can trace data lineage of any model serving predictions in production, can access the model repo for deployment, and can compare training to serving data to track outliers and data drift
- As a technical manager, I want a production training platform, so that I have transparency on costs while allowing my data scientists to quickly iterate on models; also, I have confidence that the model code being run has been reviewed by the team leads for quality








