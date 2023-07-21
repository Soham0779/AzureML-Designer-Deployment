# Problem Statement

Model building, Model training, Model deployment(to Batch and Realtime Endpoint), and Consuming the deployed model by using Azure Machine Learning Designer tool.

# Objectives

1. To upload the data to and Fetch the data using external ADLS(Azure Data Lake Storage).
1. To create Pipeline in Azure ML Designer tool.
1. To perform feature selection, training the model, model evaluation and create the Job(Experiment) for the model.
1. To register the model
1. To deploy the registered model to the Batch endpoint and the Real-time endpoint.
1. To consume the deployed model.

# Solution Methodology

Before starting with the main objectives of this project there are some pre-requisites.

## 0. Prerequisites
1. Azure Subscription
2. Azure Resource Group
3. Azure Workspace
4. Azure Compute Cluster and Compute Instance

## 1. Create an Azure Storage account (ADLS)
1. Search for **Storage accounts** on Microsoft Azure Portal.

![Search Storage Accounts](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/storage-account-1.png?raw=true)
1. Click on **+ Create** button.
    - Fill in the details under the **Basics** tab.
        - Select **Subscription**.
        - Select **Resource Group**.
        - Give Name to storage account
        - Select **Region** or use the default region.
    
    *Note: Change other details under the "Basics" tab if required.*
    
    - Click on **Advanced** Tab
        - Check **Enable hierarchical namespace** check-box.
        
        ![Check Hierarchical namespace](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/Screenshot%20(739).png?raw=true)