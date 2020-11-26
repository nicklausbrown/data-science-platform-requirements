
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

