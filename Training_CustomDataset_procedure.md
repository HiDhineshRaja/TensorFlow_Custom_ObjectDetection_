# Training with Custom Data Set.
--------------------------------

##  For training, we need the following:
-----------
### Step 1:

=> An [object detection training pipeline](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/configuring_jobs.md). 

=> They also provide [sample config files](https://github.com/tensorflow/models/tree/master/research/object_detection/samples/configs) on the repo. 

=> For my training, I used ssd_mobilenet_v1_pets.config as basis. I needed to adjust the num_classes to one and also set the path (PATH_TO_BE_CONFIGURED) for the model checkpoint, the train and test data files as well as the label map. In terms of other configurations like the learning rate, batch size and many more, I used their default settings.

=> This ssd_mobilenet_v1_pets.config file I put it inside the models/model folder of this repository.

----------
### Step 2:

=> The data_augmentation_option is very interesting if your dataset doesn’t have much of variability like different scale, pose etc.. 

=> A full list of options can be found [here](https://github.com/tensorflow/models/blob/a4944a57ad2811e1f6a7a87589a9fc8a776e8d3c/object_detection/builders/preprocessor_builder.py) (see PREPROCESSING_FUNCTION_MAP)

----------
### Step 3:

=> The dataset (TFRecord files) and its corresponding label map. Example of how to create label maps can be found [here](https://github.com/tensorflow/models/tree/master/research/object_detection/data). It’s very important that your label map should always start from id 1. 

=> I created my own label map object-detection.pbtxt and put it in the data folder.

----------
### Step 4:

=> Pre-trained model checkpoint. It is recommended to use a checkpoint as it’s always better to start from from pre-trained models as training from scratch can take days before we get good results. 

=> They provide [several model checkpoint](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md) on their repo. In my case, I used the [ssd_mobilenet_v1_coco model](https://drive.google.com/open?id=1qQzhinqwfDM8-KARTkkVqPCUJRRP4N1p) as the model speed was more important for me than accuracy.

=> Extract the downloaded pretrained model to the pretrainedCheckpoint foler inthis repo. I have already done it.

=> Now edit the ssd_mobilenet_v1_pets.config file accordingly by giving the path to train.record, test.record and model.ckpt file.

-----------
###  Step 5:

=> For running locally  [Instruction](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/running_locally.md)

=> Now you can start the training:

=> Training can be either done locally or on the cloud (AWS, Google Cloud etc.). If you have GPU (at least more than 2 GB) at home then you can do it locally otherwise I would recommend to go with the cloud. In my case, I went with Google Cloud this time and essentially followed all the steps described in their documentation.

=> For Google Cloud, you need to define a YAML configuration file. A sample file is also provided and I basically just took the default values.
	
=> It is also recommended during the training to start the evaluation job. You can then monitor the process of the training and evaluation jobs by running Tensorboard on your local machine.
		
tensorboard — logdir=gs://${YOUR_CLOUD_BUCKET}
		
=> From the tensorflow/models/research/ directory.

python object_detection/train.py \
    --logtostderr \
    --pipeline_config_path=${PATH_TO_YOUR_PIPELINE_CONFIG which is in models/model folder here} \
    --train_dir=${PATH_TO_TRAIN_DIR which is in models/model folder here}
	
=> where ${PATH_TO_YOUR_PIPELINE_CONFIG} points to the pipeline config and ${PATH_TO_TRAIN_DIR} points to the directory in which training checkpoints and events will be written to. By default, the training job will run indefinitely until the user kills it.
	
=> Evaluation is run as a separate job. The eval job will periodically poll the train directory for new checkpoints and evaluate them on a test dataset. The job can be run using the following command:

=> From the tensorflow/models/research/ directory.
  
python object_detection/eval.py \
    --logtostderr \
    --pipeline_config_path=${PATH_TO_YOUR_PIPELINE_CONFIG} \
    --checkpoint_dir=${PATH_TO_TRAIN_DIR which is in models/model folder here} \
    --eval_dir=${PATH_TO_EVAL_DIR which is in models/model folder here}
	
=> where ${PATH_TO_YOUR_PIPELINE_CONFIG} points to the pipeline config, ${PATH_TO_TRAIN_DIR} points to the directory in which training checkpoints were saved (same as the training job) and ${PATH_TO_EVAL_DIR} points to the directory in which evaluation events will be saved. As with the training job, the eval job run until terminated by default.

----------
### Step 6:

=> Progress for training and eval jobs can be inspected using Tensorboard. If using the recommended directory structure, Tensorboard can be run using the following command:

tensorboard --logdir=${PATH_TO_MODEL_DIRECTORY}
	
=> where ${PATH_TO_MODEL_DIRECTORY} points to the directory that contains the train and eval directories. Please note it may take Tensorboard a couple minutes to populate with data.	

---------	
### Step 7:

=> Exporting the model:

=> After finishing with training, I exported the trained model to a single file (Tensorflow graph proto) so that I can use it for inference.
	
=> In my case, I had to  use the [provided script](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/exporting_models.md) to export the model. The model can be found on my repo inside output_inference_graph folder , We could now use this to test the any images with elephant in it to see how well our App works

-----------	
### Step 8:

=> Go through the Testing_Elephant_Detector.ipynb file to test any image with elephant in it.