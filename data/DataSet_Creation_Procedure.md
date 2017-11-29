# DataSet Creation procedure.
-----------------------------

##Step: 1  

=> Tensorflow Object Detection API uses the [TFRecord file format](https://www.tensorflow.org/api_guides/python/python_io#tfrecords_format_details), so at the end we need to convert our dataset to this file format.


##Step: 2

=> There are several options to generate the TFRecord files. Either you have a dataset that has a similar structure to the [PASCAL VOC dataset](http://host.robots.ox.ac.uk/pascal/VOC/) or the [Oxford Pet dataset](http://www.robots.ox.ac.uk/~vgg/data/pets/), then they have ready-made scripts for this case (see [create_pascal_tf_record.py](https://github.com/tensorflow/models/blob/master/research/object_detection/dataset_tools/create_pascal_tf_record.py) and [create_pet_tf_record.py](https://github.com/tensorflow/models/blob/master/research/object_detection/dataset_tools/create_pet_tf_record.py)). 

=> If you don’t have one of those structures you need to write your own script to generate the TFRecords (they also provide an [explanation for this](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/using_your_own_dataset.md)). This is what I did!


##Step: 3 

=> To prepare the input file for the API you need to consider two things. Firstly, you need an RGB image which is encoded as jpeg or png and secondly you need a list of bounding boxes (xmin, ymin, xmax, ymax) for the image and the class of the object in the bounding box. In terms of me, this was easy as I only had one class.


##Step: 4 

=> Afterwards, I hand-labeled them manually with [LabelImg](https://github.com/tzutalin/labelImg). The Binary version is availble in data folder of this repo.


##Step: 5 

=> The Binary version of it is in [LabelImg](https://tzutalin.github.io/labelImg/). LabelImg is a graphical image annotation tool that is written in Python und uses Qt for the graphical interface. It supports Python 2 and 3 but I built it from source with Python 2 and Qt4 as I had problems with Python 3 and Qt5. 

=> It’s super easy to use and the annotations are saved as XML files in the PASCAL VOC format which means that I could also use the create_pascal_tf_record.py script but I didn’t do this as I wanted to create my own script.


##Step: 6 

=> Use the LabelImg software to create the bounding box to all the images in the images folder. This will create the xml file for each image with the bounding box details. These xml images are stored in annotaions folder of this repository.


##Step: 7 

=> There is another tool available in this link also for annotating (https://github.com/christopher5106/FastAnnotationTool)


##Step: 8 

=> After labeling the images I wrote a script that converted the XML files to a csv (i.e run Convert_xml_to_csv.py file). Then Split the generated csv file to train_labels.csv and test_labels.csv by running split_labels_to_train_test.ipynb file.

 
##Step: 9 

=> Finally, run generate_tfrecord.py (python generate_tfrecord.py --csv_input=data/train_labels.csv  --output_path=train.record), (python generate_tfrecord.py --csv_input=data/test_labels.csv  --output_path=test.record)  to create the train and test TFRecords. Cut and paste the .csv and tfrecord files generated into the data folder.


##Step: 10 

=> Now the datasets needed for feeding the object detection api in required format is available.  

----------------------------------------------------------------------------------------------------------------------------------------------------------------------

