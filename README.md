# Training Donut model on Azure Machine Learning Studio

Due to the fact that Donut model requires image of original dimension, using common 12GB vRAM GPU, the batch size is only 1. In order to speed up the training process, we trained the model on a cluster of 4 12vRam GPUs. For this reason, we used Azure Machine Learning Studio for this task. 

## Prerequisites : 
- An Azure account with an active subscription 
- A workspace on Azure Machine Learning Studio. 
If not readily available, [here's](https://learn.microsoft.com/en-us/azure/machine-learning/quickstart-create-resources?view=azureml-api-2) a tutorial to follow.

## How to start training job

In order to replicate the traininng process, here are the neccesary steps:

    - Step 1: Create a CPU compute instance.
    - Step 2: Choose kernel "Python 3.8 - AzureML" on the list. 
    - Step 3: install neccesary libraries in the requirements.txt file
    - Step 4: go to "Assets", "Data" tab, click on "Create" button to upload the dataset. Choose "Folder(uri_folder)" type for the dataset . Retrieve "Datastore URI" once the dataset is created. It will be used later.
    - Step 5: Retrieve  subscription_id, resource_group_name, workspace_name on the top right tab of the workspace. 
    - Step 6: Launch the "train_job" notebook

## Notebook

In the notebook, using the credentials and link from step 4 and 5, a training job will be submitted in a remote GPU cluster. 
Essentially, cells in the notebook comprise the following tasks: 

    - authenticate user
    - create the cluster environment (the required libraries for the cluster can be found in the "environment.yaml" file)
    - create the GPU compute cluster
    - submit training job to the compute cluster

Once the job is submitted, go to "Jobs" tab, click on the experiment name to follow the progress. 
"user_logs" is another folder that has useful logs to be inspected during the training process. After the few training epochs, there will be graphs of training, evaluation loss, learning rate ect. 

After the training finishes, the model can be found in "Outputs+logs" -> "outputs" folder.

## Interface for inference

We use the configuration for training with the trained model weights and create an interface to perform inferencing using gradio.

Following the template `app.py` ([source code](https://github.com/clovaai/donut/blob/master/app.py)) provided by the ClovaAI team, we have a simple interface that accepts image inputs and outputs the model's prediction of the MRZ value. We can also use the API created by gradio to perform inferencing using other applications.

We combine the above with the requirements file and hosted it in Hugging Face Spaces ([link](https://huggingface.co/spaces/adbcode/donut-mrz)).

## Further exploration

This project is based on DONUT(Document Understanding Transformer) model of ClovaAI team. To learn more about the model (source code + research paper), check out this [repository](https://github.com/clovaai/donut)

