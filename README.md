# Tensorflow Object Detection API Example With Custom Dataset

##Elephant Detector App

[TensorFlow's Object Detection API](https://github.com/tensorflow/models/tree/master/research/object_detection)

-----------------------------------------------------------------------------------------------------------------------------------------
## Steps to be followed for this app to be working on windows:

### Step 1:

=> Install Tensorflow latest version using the below command.
=> To install the CPU-only version of TensorFlow, enter the following command.

     C:\> pip3 install --upgrade tensorflow.	
=> To install the GPU version of TensorFlow, enter the following command.

     C:\> pip3 install --upgrade tensorflow-gpu


		
### Step 2:

=> Object Detection Api will not be installed when we use pip installation of tensorflow. 

=> So clone the Object detection tensorflow gitub repository  by going to this link [TensorFlow's Object Detection API] (https://github.com/tensorflow/models).

=> This should be cloned to the location where Tensorflow is installed. In my laptop it is installed in this location.

   (C:\Users\bizruntime45\AppData\Local\Programs\Python\Python35\Lib\site-packages\tensorflow).
   
=> So I cloned the  [TensorFlow's Object Detection API] (https://github.com/tensorflow/models) into the above location.


   
### Step 3:

=> Install the following through pip install command.

pip install pillow.

pip install lxml.

pip install jupyter.

pip install matplotlib.

pip install protobuffer.


		
### Step 4:

=> Protobuf Compilation:

The Tensorflow Object Detection API uses Protobufs to configure model and training parameters. Before the framework can be used, the Protobuf libraries must be compiled. This should be done by running the following command from the tensorflow/models/research/ directory:

=> From tensorflow/models/research/  folder in command line   run the below command.

protoc object_detection/protos/*.proto --python_out=.  

=> If you are getting error in running the above then run the above command for individual .proto files one by one in that location as follows:
   protoc object_detection/protos/anchor_generator.proto --python_out=.

   
  
### Step 5:

=>From tensorflow/models/research/  folder in command line   run the below command :

python setup.py install
	 
=>From tensorflow/models/research/slim  folder in command line   run the below command :

python setup.py install.



	 
### Step 6:

=> Testing the Installation.

#From tensorflow/models/research/

=> You can test that you have correctly installed the Tensorflow Object Detection API by running the following command:

python object_detection/builders/model_builder_test.py

=> If everything in the above steps go fine then Tensorflow object detection Api is successfully installed in your machine. 
   Now we will	proceed to create our own Object detection system with our custom data set.

-------------------------------------------------------------------------------------------------------------------------------------------------

## Steps for creating our own object detection app with custom data set:

### Custom Dataset creation steps:

=> In this repository I have created the Elephant detection app.So I downloaded the images of the elephants from the google and put it in the images folder of this repository.

=>Go to the data folder of this repository and follow the instructions given in the DataSet_Creation_Procedure.md file.
  and create the dataset which could be feed to the Tensorflow object detection Api.

-------------------------------------------------------------------------------------------------------------------------------------------------

	
### Some of the Limitations of my App:

=> Since I trained this with less data set and for only 40 minutes the accuracy will be sometimes less.But if you train with large dataset and with huge GPU power then accuracy will be high.

=> I trained to detect only elephant. similarly we could train with defferent class of images to detect different objects.



