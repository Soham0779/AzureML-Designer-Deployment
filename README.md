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
1. Search for **'Storage accounts'** on Microsoft Azure Portal.

   ![Search Storage Accounts](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/storage-account-1.png?raw=true)

2. Click on `+ Create` button.
    - Fill in the details under the **'Basics'** tab.
        - Select **Subscription**.
        - Select **Resource Group**.
        - Give **Name** to storage account (eg. 'adls1crainintern')
        - Select **Region** or use the default region.
    
    *Note: Change other details under the "Basics" tab if required.*
    
    - Click on **'Advanced'** Tab
        - Check **"Enable hierarchical namespace"** check-box.
        
          ![Check Hierarchical namespace](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/Screenshot%20(739).png?raw=true)

      *Note: Change other options if required*
    - Click on `Review and Create`

3. Once the ADLS is created Create Container inside ADLS as follows
    - Click on your ADLS by searching the name of ADLS(eg. 'adls1crainintern' in my case)
    - Click on **'Containers'** under **'Data Storage'** Tab.
    - Click on `+ Container` to create new container.
    - Give the Name to the container.
    - Click on `Create`.
    - The container gets created('crain-adls' in my case).

    ![Create Container](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/CreateContainer.png?raw=true)

4. Upload Files or Folders inside the container.
    - Click `+ Add Directory` to Create the Folder and upload the file inside that Folder.
    - Click `Upload` and **"Browse for files"** to select the files from local machine.

By Following the above steps upload the necessary dataset file (eg. .csv files) inside the folder present in the container.

## 2. Create the Data Asset
To use the dataset in ADLS we must create the data asset in Azure ML Studio, so that we can use that dataset or file for training or testing purpose.

1. Go to your Azure ML Workspace and **'Launch Studio'**.
2. Click on **'Data'** tab under Section Assets.
3. Under **'Data assets'** tab click on `+ Create` button.
    - Data Type
        - Give 'Name' to Data asset.
        - Select 'Type' as **'Tabular'**(For data in .csv/.xlsx etc).
        - Click on `Next`
    - Data Source
        - **"From Azure storage"** and `Next`
    - Storage Type
        If the Newly Created ADLS is not visible under data stores then follow these steps
        - Click `+ Create new datastore` button.
        - Give Datastore 'Name' (eg. crain_adls in my case)
        - Select 'Subscription ID' if not selected by default.
        - Select 'Storage Account' from drop down (eg. adls1crainintern in my case).
          *Check 3rd Substep of **1. Create an Azure Storage account (ADLS)***.
        - Select 'Blob container' from drop down (eg. crain-adls in my case). 
          *Check 3rd Substep of **1. Create an Azure Storage account (ADLS)***.
        - Select 'Authentication type' as **'Account Key'**.
        - Account key
            - Go to **"Azure Storage Account"** Created in 1st Step.
            - Select your **'Storage account'** (eg. adls1crainintern in my case)
            - Select **'Access Keys'**
            - Copy any key by clicking on `Show` button.
            - Goto "Azure ML Workspace" Paste the key under **"Account Key"** and `Create`.

            ![Select Storage account key](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/Screenshot%20(743).png?raw=true)

            ![Create Data Store](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/AzureStorage.png?raw=true)
        - Select Newly created Data Store as Storage type (eg. crain_adls in my case)
    - Storage Path
        - Select Folder where dataset file(.csv) is present in Azure Storage Account
        - Select the .csv file
        *Note: The Folder/File which is created under 4th sub-step of **## 1. Create an Azure Storage account (ADLS)***
    - Settings
        - Change if required else click on `Next`.
    - Schema
        - Change if required else click on `Next`.
    - Review
        - Review the changes.
        - Click on `Create`.

