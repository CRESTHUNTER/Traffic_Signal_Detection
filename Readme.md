We create an object detection program that detect custom objects of multiple classes.

Step 1: Labelling the Images

• Since it’s a multiclass classification, we need to define the object of each class. We use a tool known as labellmg.

• We provide the annotation (xmax,xmin,ymax,ymin) and give each annotation a class and is stored in .xml format.

• These xml files are then converted into csv file using xml_to_csv.py

• Due to the time constraint half of the image data was labelled.

Step 2: Transfer Learning

• Pre-trained weights were used in order to extract the basic features of an image.

• Here after considering many options ‘resnet-50’ is used as the transfer learning algorithm.

• ssd_resnet50_v1_fpn is the pre-trained model that we used that needs to be downloaded from here.

• We also need to download config file for ssd_resnet50_v1_fpn

Step 3: Object Detection Repo

• Clone github repository of object detection.

• Using cmd navigate to research directory in models and run ‘python setup.py’

• This will install all the requirements.

Step 4: Creating tfrecords

• tensorflow record needs to be generated for the model.

• In generate_tfrecord.py is used to define the labels that are used in the training.

• It creates two files, train.record and test.record.

Step 5: Creating a label map

• We create a label_map file that has names of the labels and corresponding ids assigned to them.

Step 6: Pipeline Configuration

• We use a pretrained model known as ssd_resnet50_v1_fpn

• We edit the ssd_resnet50_v1_fpn.config file according to our model.

• We specify the input path of our .ckpt file of model, train.record, test.record and label_map.

Step 7: Training

• We used train.py to train the model.

• The model is given following inputs train_directory, pipeline_config_path.

• We ran this program for around 3800 steps to get a training loss of approximately 0.45.

• We save the final model as frozen_inference_graph.

NOTE: If you get an error for no module named 'nets' Use this: c:\foo\bar\models\research> cd slim c:\foo\bar\models\research\slim> move BUILD BUILD.old c:\foo\bar\models\research\slim> python setup.py build c:\foo\bar\models\research\slim> python setup.py install

Step 8: Exporting Inference graph:

• After training the model we get trained weights.

• This file is stored as model.ckpt-(the number of steps at which the weights were saved)

• We also give it the following inputs

• (running the command : python export_inference_graph.py)

 --input_type image_tensor \
 --pipeline_config_path path/to/ssd_resnet50_v1_fpn.config \
 --trained_checkpoint_prefix path/to/model.ckpt--checkpoint_number \
 --output_directory path/to/exported_model_directory
• We create a .pb file called as frozen_inference_graph in a directory exported_model. • This is the final weight that can be used for the detection.

Step 9: Testing

• We used 210 images as a test dataset.

• We create an inference.py file that takes the following inputs:

--dir == Image_directory (Images to be tested)
--label_dir == This is the path to label_map.pbtxt (do not include label_map.pbtxt)
--model == Path to frozen_inference_graph (do not include frozen_inference_graph.pb)
Step 10: Results

• After running the inference.py, the program exports a csv file (output.csv) with same annotation format as the input
