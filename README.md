# Gaze Based Annotation of Histopathology Images for Training of Deep Convolutional Neural Networks
(under construction)

## Requirements
- Tensorflow 1.15
- Python 3.7
- Anaconda
- Qupath (for viewing Whole Slide Image (WSI) file)
- MATLAB (masks to bounding box conversion)
- Google Colab or Jupyter Notebook

## Setting up Python Environment
1) Create a conda environment and install Tensorflow:
```
conda create -n tensorflow1.15 python=3.7
conda activate tensorflow1.15
conda install cudatoolkit=10.0
conda install cudnn=7.6.5
pip install tensorflow-gpu==1.15
```
2) Install dependencies
```
pip install numpy==1.19.5 lxml pillow matplotlib jupyter contextlib2 cython tf_slim pycocotools
```
(For windows change `pycocotools` to `pycocotools-windows`)


3) Install the [TensorFlow Object Detection API ](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf1.md).
## Dataset
Follow the steps in `\Generate_Annotations` to create your own dataset from gaze and hand annotations. **@komal:** *I don't see the raw gaze files. Have we provided a link to those somewhere? That link should be visible here and/or in the readme file in \Generate_Annotations.*

**@Dr.Hassan** Added the link to raw gaze files and their corresponding csv. Added instructions to visualise gaze annotations in Qupath in /Generate_Annotations readme

Or simply download our dataset here:

**- Gaze:** Images used for training and testing of gaze-based object detectors can be downloaded from the following link: https://1drv.ms/u/s!As_geBXhgCy1qjOElYHo5oWX_OQ0?e=L38qQ6. The labels corresponding to each file in the training and test dataset can be found in "Gaze_Data/labels/train" and "Gaze_Data/labels/test" respectively. 

**- Hand:** Images used for training and testing of object detectors on hand-labelled data can be downloaded from the following link: https://1drv.ms/u/s!As_geBXhgCy1qwa3-NdukNHbLRsb?e=NT3Abi. The labels corresponding to each file in the training and test dataset can be found in "Hand_Data/labels/train" and "Hand_Data/labels/test" respectively. `NOTE:` Hand generated labels were used for performance evaluation of both gaze-based and hand-labelled object detectors. Therefore, the contents of both the "Gaze_Data/labels/test" and the "Hand_Data/labels/test" folders are identical. 

**@komal:** *Please verify that the preceding statement is correct? I recommend putting Test images in the HAND data instead of the GAZE data to avoid confusion. We're testing both detectors on Hand labels so it makes more sense to have the test data in the Hand Data director.*

**@Dr.Hassan**: Please have a look now. test set in both folders is identical and created via hand labels. (Should I only have the test labels in hand-data directory?). Also updated the labels to PASCAL VOC format since its more standardized than csvs. 

**@komal:** Names of test images in Gaze_Data and Hand_Data are slightly different. Images contents are the same though. Names of label files are the same in both label directories. Names of image files in Gaze_Data/images/test_png/ matches the names of the label files in both Gaze_Data and Hand_Data directories. I think you should copy-paste test images from Gaze_Data/images/test_png/ and overwrite images in Hand_Data/images/test/ to make things consistent. I have not done so myself because I am not sure if renaming files will have an impact on your code. The description I added above (under -Gaze and -Hand headings) is written with the assumption that test, label and image files for both Gaze and Hand data are identical. Please delete this comments and preceding 2 comments after image files are overwritten.

**- Masks:** The binary masks used for generating labels for hand and gaze-based object detectors can be downloaded from the following links: https://1drv.ms/u/s!As_geBXhgCy1rBzg_A3ssuabn6TF?e=MD21iT

**- Raw Gaze Data:** https://1drv.ms/u/s!As_geBXhgCy1rkleyurgCn5g3RdX?e=f2OkBb
   1) Every eye gaze data collection session (lasted about 5-10 minutes on average) is contained in a folder named according to the date and time. 
   2) "all_sessions_merged" folder contains all levels from every gaze data collection session merged together.
   3) "all_annotations_on_max_level" contains one csv file of the maximum resolution containing all gaze data points from all levels scaled to the highest resolution of the .svs image.
   4) ".svs" files are the corresponding WSI file.



## Models
- Pre-trained models: https://1drv.ms/u/s!As_geBXhgCy1rSjYGEV7aMdLiYnr?e=qLZOUz
- Raw [Faster RCNN Inception V2 weights](http://download.tensorflow.org/models/object_detection/faster_rcnn_inception_v2_coco_2018_01_28.tar.gz) 

## Training
1. Download raw Faster RCNN Inception V2 Weights from the above link and into the `/training` directory.
2. From utils use the appropriate `generate_tfrecord` script to generate 'train.record' and 'test.record' from your train and test sets.
3. Paste 'train.record' and 'test.record' into the `/training` directory.
4. Set the appropriate paths in `training_pipeline.config`
5. Now initiate a training job by opening a new Terminal, cd inside the `models/research/object_detection` and run the following command:
```
python model_main.py  --logtostderr --model_dir=PATH_TO_BE_CONFIGURED\models\research\object_detection\training\faster_rcnn_inception_v2_coco_2018_01_28 --pipeline_config_path=PATH_TO_BE_CONFIGURED\models\research\object_detection\training\training_pipeline.config           
```
6. After training has completed, export the inference graph using `export_inference_graph.py` in the `models\research\object_detection` directory

## Evaluation
The Faster RCNN model was implemented in tensorflow. `Evaluation.ipynb` can be used to test the Faster RCNN model on the test data.
1. Download and extract Gaze-based and Hand-Annotated trained models (https://1drv.ms/u/s!As_geBXhgCy1rSjYGEV7aMdLiYnr?e=qLZOUz). If you prefer to train your own model then you can do so by following the instructions provided in the Training section above.
3. Paste `Evaluation.ipynb` and `/images` into `models/research/object_detection/`
4. Open terminal and cd `models/research/object_detection/` 
5. Run `Evaluation.ipynb` notebook via Jupyter Notebook

The YOLO models were implemented in PyTorch and can be found at the following repo: link to Osama's repo here.
## Reference
This repo was used to generate the results for the following paper on Gaze-based labelling of Pathology data. 
   
   Komal Mariam, Osama Mohammed Afzal, Wajahat Hussain, Muhammad Umar Javed, Amber Kiyani, Nasir Rajpoot, Syed Ali Khurram and Hassan Aqeel Khan, **"On Smart Gaze based Annotation of Histopathology Images for Training of Deep Convolutional Neural Networks",** *submitted to IEEE Journal of Biomedical and Health Informatics.*


**BibTex Reference:** Available after acceptance.
