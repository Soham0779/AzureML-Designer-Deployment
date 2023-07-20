# Set up Visual Studio Code desktop with the Azure Machine Learning extension

The Azure Machine Learning extension for VS Code provides a user interface to:

- Manage Azure Machine Learning resources (experiments, virtual machines, models, deployments, etc.)
- Develop locally using remote compute instances
- Train machine learning models
- Debug machine learning experiments locally

## Prerequisites

Before you get started, you will need:

1. An Azure Machine Learning workspace and compute instance.
1. Sign in to studio and select your workspace if it's not already open.

## Install the extension

1. Open Visual Studio Code.
1. Select **Extensions** icon from the **Activity Bar** to open the Extensions view.
1. In the Extensions view search bar, type "Azure Machine Learning" and select the first extension.
1. Select **Install**.

![Extension Installation](media/2023-06-12.png)

## Sign in to your Azure Account

To sign into your Azure account, select the **Azure: Sign In** button in the bottom right corner on the Visual Studio Code status bar to start the sign in process.

## Choose your default workspace

To choose your default workspace, select the **Set Azure Machine Learning Workspace** button on the Visual Studio Code status bar and follow the prompts to set your workspace.

### Azure Machine Learning Extension

1. In VS Code, launch the Azure Machine Learning extension.
1. Expand the **Compute instances** node in your extension.
1. Right-click the compute instance you want to connect to and select **Connect to Compute Instance**.

![Connect to compute instance](<media/2023-06-12 (1).png>)

A new window of VS Code opens with the remote connection to the compute instance that you selected.

# Git integration for Azure Machine Learning

[Git](https://git-scm.com/) is a popular version control system that allows you to share and collaborate on your projects.

Azure Machine Learning fully supports Git repositories for tracking work - you can clone repositories directly onto your shared workspace file system, use Git on your local workstation, or use Git from a CI/CD pipeline.

When submitting a job to Azure Machine Learning, if source files are stored in a local git repository then information about the repo is tracked as part of the training process.

Since Azure Machine Learning tracks information from a local git repo, it isn't tied to any specific central repository. Your repository can be cloned from GitHub, GitLab, Bitbucket, Azure DevOps, or any other git-compatible service.

## Clone Git repositories into your workspace file system

It is recommended that you clone the repository into your user directory so that others will not make collisions directly on your working branch.

You can clone any Git repository you can authenticate to (GitHub, Azure Repos, BitBucket, etc.)

## Authenticate your Git Account with SSH

### Step 1: Generate a new SSH key

1. Open the terminal in Visual Studio Code by going to `Terminal > New Terminal`.

![Terminal with remote connection](<media/2023-06-12 (2).png>)

2. Paste the text below, substituting in your email address.

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

This creates a new ssh key, using the provided email as a label.

```
> Generating public/private rsa key pair.
```

3. When you're prompted to "Enter a file in which to save the key" press Enter. This accepts the default file location.

4. Verify that the default location is '/home/azureuser/.ssh' and press enter. Otherwise specify the location '/home/azureuser/.ssh'.

```
> Enter a file in which to save the key (/home/azureuser/.ssh/id_rsa): [Press enter]
```

5. At the prompt, type a secure passphrase. Press Enter if no passphrase needs to be added.

```
> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]
```

![Generate SSH key in Terminal](<media/2023-06-12 (3).png>)

### Step 2: Add the public key to Git Account

1. In your terminal, copy the contents of your public key file. If you renamed the key, replace id_rsa.pub with the public key file name.

```bash
cat ~/.ssh/id_rsa.pub
```

2. Select and copy the SSH key output to your clipboard.

![Copy SSH key from Terminal](<media/2023-06-12 (5).png>)

### Step 3: Add the public key to Azure DevOps

Associate the public key generated in the previous step with your user ID.

1. Open your security settings by browsing to the web portal and selecting your avatar in the upper right of the
   user interface. Select **SSH public keys** in the menu that appears.

![Azure Repos settings](<media/2023-06-12 (6).png>)

2. Select **+ New Key**.

![New Key](<media/2023-06-12 (7).png>)

3. Paste the contents of the public key (for example, id_rsa.pub) that you generated into the **Public Key Data** field.

4. Give the key a useful description (this description is displayed on the **SSH public keys** page for your profile) so that you can remember it later. Select **Save** to store the public key.
   Once saved, you can't change the key. You can delete the key or create a new entry for another key. There are no restrictions on how many keys you can add to your user profile. Also note that SSH keys stored in Azure DevOps expire after one year. If your key expires, you may upload a new key or the same one to continue accessing Azure DevOps via SSH.

![Enter key and description](<media/2023-06-12 (8).png>)

5. Test the connection by running the following command: `ssh -T git@ssh.dev.azure.com`.
   If everything is working correctly, you'll receive a response that says: `remote: Shell access is not supported.`

![Check in Terminal](<media/2023-06-12 (9).png>)

### Step 4: Clone the Git repository with SSH

1. Copy the SSH clone URL from the web portal.

![Clone using SSH from Azure Repos](<media/2023-06-12 (10).png>)

2. Run `git clone` from the command prompt.

![Run git clone command in Terminal](<media/2023-06-12 (12).png>)

When you're asked if you want to continue connecting, enter `yes`. Git clones the repo and sets up the `origin` remote to connect with SSH for future Git commands.

The following changes can be seen in Azure Machine Learning Studio. Further development can be done remotely on VS Code itself including the use of Git commands in the Terminal.

![Changes reflected in Azure ML Studio](media/2023-06-13.png)

To connect to a compute instance:

1. Create a new Notebook or open an existing Notebook in Visual Studio Code.
1. When the integrated notebook experience loads, choose **Select Kernel**.

![Select Kernel](<media/2023-06-13 (1).png>)

1. Choose `Azure ML Compute Instances` from the list of Jupyter server options.
1. Select your workspace.
1. Select your compute instance from the list.

![Select compute instance from the list](<media/2023-06-13 (2).png>)

1. Now you can run a cell.

Now that you've launched Visual Studio Code remotely connected to a compute instance, you can prep your data, edit and debug your code, and submit training jobs with the Azure Machine Learning extension.
