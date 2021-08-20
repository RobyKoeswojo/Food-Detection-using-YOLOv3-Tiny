# Food Detection using YOLOv3-Tiny

HBO's Silicon Valley is my favourite tech themed TV series about a programmer who runs a startup in Silicon Valley, California. 
In the fourth episode of the fourth season, a character named Jian-Yang, who is supposed to create an app like the Shazam for food (an app to detect food using phone camera), presents the demo of the app in front of other residents of the incubator he stays in. The funny thing, the app he makes is only able to distinguish a 'hotdog' and 'not hotdog'. So it is basically an image classification task in machine learning with two classes, i.e. 'hotdog' and 'not hotdog'. The clip is here https://www.youtube.com/watch?v=pqTntG1RXSY  

Based on this scene, I created an object detection task that classifies the food in the image, predicts its location and draws the box around it. Since this is just a trial project, I limit the classes (food) to be detected to 3 classes: hotdog, hamburger and pizza.  
The object detection algorithm used is [YOLOv3](https://arxiv.org/abs/1804.02767), specifically, the tiny version of it for 2 reasons: 
(1) because the image datasets used are small datasets, and (2) to save time.

## Data  
Only 50 images per class (food) are used to train the YOLOv3-Tiny network. The images are downloaded from:
- [Caltech101](http://www.vision.caltech.edu/Image_Datasets/Caltech101/) (pizza) 
- [Food-101 dataset at Kaggle](https://www.kaggle.com/kmader/food41) (hotdog and hamburger)

The ground truth bounding box is needed to train the YOLOv3-Tiny network. The [LabelImg](https://github.com/tzutalin/labelImg) app is used to create the ground truth bounding box (set the annotation to **YOLO**).

The image preparation step can be seen in ``0 Image preparation``

| ![Labelling hotdog](https://github.com/RobyKoeswojo/Personal-Projects/blob/main/Food%20Detection%20using%20YOLOv3-Tiny/readmeImage/labellingHotdog.JPG?raw=true) |
|:--:| 
| Creating ground truth annotation (bounding box) using [LabelImg](https://github.com/tzutalin/labelImg) |

## Training  
The YOLOv3-Tiny is trained using Google Colab. The preparation to train the YOLOv3-Tiny:
- Create a working folder in google drive
- Upload zipped dataset (consisting of all food images and their corresponding annotations) to the working folder
- Upload and run the commands in ``1 Training YOLOv3-Tiny_Food Detection`` in Google Colab with GPU hardware accelerator on  

Brief overview of the training step:
- Clone the ``darknet`` repository from ``https://github.com/AlexeyAB/darknet.git``
- Modify ``Makefile`` to use ``OpenCV``, and turning on ``GPU`` and ``CUDNN``. Compile the ``Makefile``
- Copy ``.cfg`` configuration file, modify the variables according to number of classes to be detected
- Unzip the zipped dataset, copy it to `object` folder in the cloned repository
- Create new files defining the classes, and paths to the images
- Download YOLOv3-Tiny pre-trained weights available here https://pjreddie.com/media/files/yolov3-tiny.weights
- Start training

The training lasted +- 3 hours (6000 epochs) with average loss value of 0.373 at the last epoch.

*Note*  
Detail explanation about using YOLOv3 for custom dataset can be found here: https://github.com/AlexeyAB/darknet

## Detection
The trained YOLOv3-Tiny weights file is downloaded and used to detect the food. 
Run the notebook ``2 Food Detection using YOLOv3-Tiny`` to run the food detection. 
The test images are random images downloaded from google.  

The detection results:
- The confidence value (probability of object detected) at some images is low (+- 0.39)
- For food that are located close together, the food detector fails to detect all food

| ![Succesful detection](https://github.com/RobyKoeswojo/Personal-Projects/blob/main/Food%20Detection%20using%20YOLOv3-Tiny/readmeImage/detectFood.JPG?raw=true) |
|:--:| 
| Succesful food detection, but some food is detected with low confidence value (+- 0.39) |

| ![Fail detection](https://github.com/RobyKoeswojo/Personal-Projects/blob/main/Food%20Detection%20using%20YOLOv3-Tiny/readmeImage/detectFail.JPG?raw=true) |
|:--:| 
| Fail food detection: not all food in the image is detected |

To improve the food detector:
- Increase the number of training images
- Use YOLOv3 (not the tiny version) -- computational cost also increases
