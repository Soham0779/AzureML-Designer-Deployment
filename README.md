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

## 3. Create Designer Model
Once the data asset is created, we can create the model inside Azure ML Designer tool to train the model.

1. Click on **'Designer'** tab.
2. Click on **'+'** Create a new pipeline using classic prebuilt components.
3. Under **'Asset Library'** tab select **'Data'** tab(From Data and Componenet), then select the data asset name (eg. customer-transaction in my case).

![Importing dataset](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/Screenshot%20(746).png?raw=true)

4. Create the basic pipeline to check how many features are important from available features.
    - Click on **'Component'** Tab.
    - Search, Drag & Drop: "Select Columns in Dataset"
        - Connect the component to the dataset.
        - Double-click the component.
        - Click on **'Edit column'**.

        ![Select Columns](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/Screenshot%20(747).png?raw=true)
        - Follow steps given in image to **DROP UNWANTED COLUMNS**.
        *Note: To drop columns use **Exclude** and to select useful columns use **Include**.*

        ![Select columns](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/Screenshot%20(748).png?raw=true)

    - Search, Drag & Drop: "Filter Based Feature Selection"
        - Connect the component to the 'Select Columns in Dataset' component.
        - Double-click the component.
        - Provide **"Number of desired features"** (will select the top n features which are affecting the target column).
        - Provide **'Target'** column by clicking on **'Edit Column'**.
    *Note: Perform this step after analysing data carefully as it can drop certain important feature which may affect the accuracy of the model.*

    - Search, Drag & Drop: "Split data"
        - Connect the node 'Filtered dataset' of "Filter Based Feature Selection" container to 'Dataset' of 'Split Data' Container.
        - Double-click the component.
        
        ![Split data](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/Screenshot%20(749).png?raw=true)

    - Search, Drag & Drop: "Two-Class Logistic Regression" Algorithm for train model
    
    - Search, Drag & Drop: "Train Model"
        - Connect the node 'Untrained model' of "Two-Class Logistic Regression" container to 'Untrained model' of 'Train Model' Container.
        - Connect the node 'Result Dataset1' of "Split data" container to 'Dataset' of 'Train Model' Container.
        - Double click 'Train Model'.
        - Click on **"Edit column"**.
        - Select/Type name of target column.

    - Search, Drag & Drop: "Score Model"
        - Connect the node 'Trained model' of "Train Model" container to 'Trained model' of 'Score Model' Container.
        - Connect the node 'Result Dataset2' of "Split data" container to 'Dataset' of 'Score Model' Container.

    - Search, Drag & Drop: "Evaluate Model"
        - Connect the node 'Scored Dataset' of "Score Model" container to 'Scored Dataset' of 'Evaluate Model' Container.

5. Click on **'Settings'** > **"Select Compute cluster"**. > `Save`

![Select compute cluster](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/Screenshot%20(750).png?raw=true)

6. Click on `Validate` button. (Check for errors)
7. Click on `Submit` button

![Submit job](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/Screenshot%20(751).png?raw=true)

*Note: Check 'Select existing experiment' if exists. This window will create New Experiment and will create a Pipeline Job under that experiment. To check the Job details click on **'Jobs'** under 'Assets' tab and Select name of your experiment under **'Experiment'** tab and click on Job name.*

*Jobs > All experiments -> name_of_your_Experiment -> name_of_your_Job*

## 4. Evaluate Job
Once the job gets executed successfully, we can evaluate the model.
1. Right Click on **'Evaluate Model'** component.
2. Preview Data -> Evaluation results

![Evaluate model](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/Screenshot%20(752).png?raw=true)

3. Evaluation Results

![Evaluation Results](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/Screenshot%20(753).png?raw=true)

## 5. Register Model
Once the Job gets executed susscessfully we can register the model which we can use for Batch Deployment.

1. Follow the steps given below
    - Double click on **'Train Model'** component.
    - Select Output and Logs > Show data outputs > Register Model
    ![Evaluation Results](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/Screenshot%20(755).png?raw=true)

2. Give 'Model name' and `Create`.
3. Check for your model name under **'Models'** tab (from side Menu).

## 6. Create Environment for Deployment
1. Click on **'Environments'** tab (from side menu)
2. Click on **'Custom environments'**.
3. Follow the steps given below
    - Settings
        - Fill Name
        - **Select environment source**: Use existing docker image with optional conda file.

        ![Evaluation Results](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/Screenshot%20(757).png?raw=true)
        - Click `Next`
    - Customize
        - Copy paste the **'conda_env.yml'** file code here (The file is downloaded while registering the model. See the image in step 5. Register Model).
    - Review and Create the Environment.

## 7. Create Data Asset for Batch-Deployment
1. Create Folder in ADLS and upload batch data in .csv file(s).
2. Under **'Data assets'** tab click on `+ Create` button.
    - Data Type
        - Give 'Name' to Data asset.
        - Select 'Type' as **'Folder'**(For data in .csv/.xlsx etc).
        - Click on `Next`
    - Data Source
        - **"From Azure storage"** and `Next`
    - Storage type: Created in Step 2(eg. crain_adls in my case)
    - Storage Path: Select FOLDER(not files) in which data is present in batches.
    - Review and Create

## 8. Create Batch Deployment
1. Select **'Models'** tab.
2. Select Model to deploy
3. Deploy -> Batch Endpoint
![Batch ep](https://github.com/Soham0779/AzureML-Designer-Deployment/blob/main/media/Screenshot%20(758).png?raw=true)
4. Follow steps given below
    - Endpoint
        - Endpoint: New
        - Endpoint Name: "Give name to endpoint"
    - Model: Check and `Next`
    - Deployment: Change details else keep default and `Next`
    - Environment
        - Create score.py with following code and select that file
        ```
        import os
        import json
        import pandas as pd
        from typing import List, Any, Union

        from azureml.studio.core.io.model_directory import ModelDirectory
        from pathlib import Path
        from azureml.studio.modules.ml.score.score_generic_module.score_generic_module import ScoreModelModule
        from azureml.designer.serving.dagengine.converter import create_dfd_from_dict
        from collections import defaultdict
        from azureml.designer.serving.dagengine.utils import decode_nan
        from azureml.studio.common.datatable.data_table import DataTable


        model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'trained_model_outputs')
        schema_file_path = Path(model_path) / '_schema.json'
        with open(schema_file_path) as fp:
            schema_data = json.load(fp)


        def init():
            global model
            model = ModelDirectory.load(model_path).model

        def run(mini_batch: List[str]):
            data = pd.concat(
                map(
                    lambda fp: pd.read_csv(fp).assign(filename=os.path.basename(fp)), mini_batch
                )
            )

            # Predict over the input data, minus the column filename which is not part of the model.
            pred = model.predict(data.drop("filename", axis=1))

            return pred
        ```
