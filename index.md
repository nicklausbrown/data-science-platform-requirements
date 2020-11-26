
# Table of Contents

* TOC
{:toc}

# Overview

This document outlines the key requirements for an enterprise data science platform. In order to support the growing business demand for new and advanced services that are based on ML, data science, data engineering and DevOps teams are looking for ways to increase productivity and accelerate the time it takes to deliver machine learning models all the way from concept to production. Ideally they would like to focus on writing the code and the business logic for their services rather than dealing with building the infrastructure that supports it. This is where an enterprise data science platform plays a part by providing all the relevant building blocks for running ML pipelines all the way from data collection through data preparation, training, deployment and model monitoring. Eventually more and more customers understand the need for building a machine learning factory which automates the heavy lifting of building machine learning pipelines and microservices.


This doc reflects in a very detailed way the various pieces that are needed for such a “factory”. Note that these requirements are not specific to the Iguazio Data Science Platform and they are based on a list of requirements made by our customers. It also outlines the requirements from the point of view of the various stakeholders: Data Scientists, Data Engineers, and DevOps.

# Seminal Papers and Blogs on DSci Platforms
We stand on the shoulders of giants. Many amazing engineers and scientists have come before to help pave a clearer vision of what MLOps and Data Science platforms should look like. These are many of the resources I have been inspired by, and I encourage addition here. If you are totally unfamiliar with production machine learning, start by reading these articles.

- [Hidden Technical Debt in Machine Learning Systems](https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems.pdf)
- [The ML Test Score: A Rubric for ML Production Readiness and Technical Debt Reduction](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/aad9f93b86b7addfea4c419b9100c6cdd26cacea.pdf)
- [The Winding Road to Better Machine Learning Infrastructure Through Tensorflow Extended and Kubeflow](https://labs.spotify.com/2019/12/13/the-winding-road-to-better-machine-learning-infrastructure-through-tensorflow-extended-and-kubeflow/)
- [Meet Michelangelo: Uber’s Machine Learning Platform](https://eng.uber.com/michelangelo-machine-learning-platform/)
- [Michelangelo PyML: Introducing Uber’s Platform for Rapid Python ML Model Development](https://eng.uber.com/michelangelo-pyml/)
- [Turbocharging Analytics at Uber with our Data Science Workbench](https://eng.uber.com/dsw/)

# Requirements
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

## Machine learning pipeline component container registry
### Implementation Details
- Provides parameterized images which provide common machine learning functionality (preprocessing [i.e. tokenization])
- Has documentation around the interfaces for the containers as well as recommended compute parameters
- Ensures that common components are written only once and can be reviewed by top experts in that functionality
- Enables software engineers and data engineers to provide highly optimized components (potentially compiled) for typically slow or compute intensive tasks
- Has a place where data scientists can request components be built, optimized, and updated

### Stakeholder Benefits
- As a data scientist, I want a component registry, so that I can spend less time writing the same pre/post-processing code and focus on models/features/ideas; when I solve a custom problem, I can share it as a component with my peers; I can take advantage of optimized components that are written for performance
- As a data engineer, I want a component registry, so that I can write components that interact with data sources using best practices and avoids data scientists improperly or over/under utilizing a data sources capabilities
- As a software engineer, I want a component registry, so that I can solve problems for my data science team that is shared across all teams and I know what data scientists are asking for from a component feature standpoint
- As a devops engineer, I want a component registry, so that I have a single place to run tests and builds which should create more consistent production performance for data scientists using the components in pipelines
- As a technical manager, I want a component registry, so that problems are being solved once and by those who are the experts in that components functionality

## Production inference deployment system
### Implementation Details
- Pipelines should replicate any training logic
- Pipelines allow for definition through standard conditional, aggregating, and dependency config file that is built behind the scenes for micro-batch or stream processing
- Is the only source of machine learning model prediction (aside from direct software embedded models which should be use case justified)
- is only accessible through automated build/deploy pipelines from code, meaning supports full software automation of deployment process
- Is triggered through merge to master
- Is different from experimental model deployments; this layer has SLAs and governance
- Is integrated with production monitoring system 
- Allows for complex inference pipelines which can have multiple models (which can also have their own pre/post-processing pipelines) 
- Only supports containerized model components in the inference pipelines which include hardening and security
- Supports low latency stream inference for time sensitive prediction requests that is capable of auto-scaling in response to load (would be cool to use machine learning to predict spiking load)
- Supports micro-batch processing for large datasets that is capable of scale to zero when there is no load (reducing cost) and can run on preemptible (spot) hardware
- Supports GPU/CPU for both batch and stream inference with profiling done in the pre-deployment build step to choose hardware type
- Has access to the pipeline component container registry to use optimized prebuilt processing components 
- Performs production readiness checks and assigns score for models prior to release pending approval
- By default logs component input and output payloads to monitoring service as well as to a persistent raw layer for long term storage
- Requires that models are served using the common api 
- Supports A/B testing of similar models behind a common endpoint and performs multi-arm bandit to find the optimal model(s)
- supports canary roll out of model(s) with automated testing to determine any training/serving performance skew with automated rollback
- Uses an SDK or component to do schema and feature validation/expectation checking with outlier/error logging to production monitoring 
- Has direct access to the model artifact store for building the final Docker images that will be shipped
- Has permissions to access data sources built in at the service level (meaning images don’t come with secrets baked in)
- Has reporting on costs at the component, pipeline, project, and business line levels
- Generates documentation around inference pipelines input/outputs as well as SLAs

### Stakeholder Benefits
- As a data scientist, I want an inference system, so that I can quickly and safely ship a model to production which has been thoroughly vetted and uses the components I used in development (preventing code rewrites); I want to use use simple configuration files to express complicated infrastructure requirements reducing DevOps knowledge requirements; I can also do analytics on model inputs and outputs to further improve them and debug issues
- As a data engineer, I want an inference system, so that I can prioritize data source access based on SLAs in a straightforward way that allows me to prioritize at the division level — if necessary avoiding local optimization on projects, leveraging data access components again here also allows me to solve the problem once
- As a DevOps engineer, I want an inference system, so that I can govern model deployment and upgrading, define scaling and cost policies by default, guarantee security is being followed with model pipelines (and push it up to higher levels reducing likelihood of secret leaks), and provide fault tolerance with rollbacks and canaries
- As a software engineer, I want an inference system, so that I can have a centralized location to request predictions from and that I have confidence my SLAs will be supported at the organizational level rather than team by team which allows me to develop software with confidence
- As a technical manager, I want an inference system, so that I have confidence that models in production come with full production support and vetting, no “thin-air” models, I can also monitor spending on inference for a given task or pipeline to ensure ROI requirements are met
- As a MLOps engineer, I want an inference system, so that the data necessary to perform data lineage, governance, and performance monitoring is guaranteed to be collected for all production models and that all pipelines (current and historical) are perfectly reproducible — due to containerization, model versioning, etc.

## Production monitoring system
### Implementation Details
- Reads directly from all model inference system output streams 
- Has access to production model training platforms to be able to trigger retraining 
- Has access to git repos or configuration management solution to perform a model version update for retrained models (this then triggers an inference deployment build/test/approval workflow)
- Has access to feature data sources to support data drift monitoring
- Performs data drift monitoring on incoming inference data compared to schema definitions on the input features for all production models
- Tracks concept for all models in production and can trigger retraining
- Tracks “staleness” for models either via concept drift or that is manually specified for a model component
- Provides alerting functionality via email, teams, etc.
- Monitors performance and SLA requirements such as defined budget, component/pipeline runtime, exception tolerance, batch prediction delivery time
- Performs monitoring on data source dependency changes which alerts when a model has lost support for a feature from a dataset
- Persists all monitoring data for later more complex analysis

### Stakeholder Benefits
- As a data scientist, I want a monitoring system, so that I have confidence that models in production are still performing according to business/technical specifications; my time is saved when models/data drifts outside of requirements due to automated retraining, allowing me to focus on current projects instead of having to "stop the world" for retraining; I can look for more complex relationships between model performance by analyzing longer term trends that have been persisted.

## Feature store for models
### Implementation Details
- Provides a way for data scientists to expose and share features between projects and for better collaboration. All features should be available to everyone with minimal effort based on security policies
- Serves compound features online mainly for prediction and recommendation
- Prepares feature for offline training
- Has a standard interface for getting data and share between DS and DE ensuring that training and inference have the same features
- Enables loading features from streaming engines, files and databases 
- Easily able to serve large volume of historical features for training
- Supports real time serving with low latency requirements
- Ensures feature consistency between training and serving
- Provides centralized feature management 
- Accessible by Python SDK (P1) followed by Go and Java 
- Manages data access and data versioning
- Supports any data store (S3, Hadoop, JDBC)
- Tracks feature metadata and lineage (creation, update, deletions)

### Stakeholder Benefits
- As a data scientist, I want a feature store for models to ensure that the data I train with and the data the model performs inference on don’t diverge, that I have access to features that are already engineered (reducing workload and duplicative data), and there is ideally a single source of truth for a feature. 
- As a data engineer, I want a feature store so that I can support multiple teams’ models in a singular location - meaning I can optimize access, priority, and monitoring
- As a software engineer, I want a feature store so that I can make changes to my code that might affect a feature downstream and notify the teams using that feature of the change before pushing into production and breaking machine learning models.
- As a technical manager, I want a feature store so that I can release new models recently trained with confidence into inference mode, delivering results to the business faster and more consistently 

## Annotation and sampling system
### Implementation Details
- Supports all standard target annotation types (category, image segmentation, named entity, etc.) in an attractive UI 
- Provides a simple way to ingest training records into the feature store if the data is external to the platform
- Annotations are automatically attached to training records in the feature store along with additional metadata about: who did the annotation, when it was done, if more than one person annotated, if the annotation was done by a software system, if there are discrepancies on the annotation
- Supports active learning for annotations to reduce time to first model training and prototyping
- Connects to a simplistic modeling backend which provides baseline model training and evaluation with boosting algorithms (or pretrained NN in some cases) to evaluate if annotation is achieving baseline “accuracy” requirements - indicating that annotation may slow down or stop
- Samples training records for annotation in simple, stratified, or cluster random sampling accord to data scientists requirements for macro vs micro model evaluation
- Allows data scientists to register a model with the annotation system when in inference mode to perform ongoing sampling for annotation to feed to the production monitoring system to track “accuracy” in production 

### Stakeholder Benefits
- As a data scientist, I want an annotation and sampling system to allow me to enjoyably and easily create new training/validation data for problems that are not automatically labeled (or take too long to get labeled). I want the system to allow me to train a baseline model and understand problem feasibility as quickly as possible. I also want to automate inference evaluation for models as much as possible to allow me to move on to new exciting tasks.
- As a machine learning engineer, I want an annotation and sampling system to formalize how any model will be evaluated in production prior to release. I want sound statistical processes put in place to guarantee confidence of model performance.
- As a technical manager, I want an annotation and sampling system so that my data scientists can quickly try out new ideas and innovate for the business without having to wait for external annotation.

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

