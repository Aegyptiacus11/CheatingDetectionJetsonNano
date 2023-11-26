# CheatingDetectionJetsonNano
Cheating Detection using the Nvidia Jetson Nano and the YOLO Darknet framework.


## I. Setting up Jetson Nano:
### 1. Making sure CUDA PATH is set on your Jetson Nano (assuming you have CUDA installed):
1. Use this command on your terminal:
```Shell
nvcc --version
```
   The output should look something like this:
```Shell
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2022 NVIDIA Corporation
Built on Wed_Jun__8_16:49:14_PDT_2022
Cuda compilation tools, release 10.2, V10.2.0
Build cuda_10.2.r10.2/compiler.31442593_0
```
2. If you instead get:
```Shell
Command 'nvcc' not found.
```
   Then run the following command to open the ```.bashrc``` file:
```Shell
 nano /home/$USER/.bashrc
```
And add these lines to ```.bashrc```:
```Vim
 export PATH="/usr/local/cuda-X.X/bin:$PATH"
 export LD_LIBRARY_PATH="/usr/local/cuda-X.X/lib64:$LD_LIBRARY_PATH"
```
### 2. Installing Darknet:
1. Clone AlexeyAB's Darknet repository:

```Shell
git clone https://github.com/AlexeyAB/darknet
cd darknet
```

2. Set ```GPU=1``` ```CUDNN=1``` and ```OPENCV=1``` (assuming you have OpenCV and CUDNN installed) in the ```Makefile```.  
   (optional: you can set ```LIBSO=1``` to use darknet with pyhton)

4. Uncomment this line in the ```Makefile``` by removing ```#```:
```Makefile
ARCH= -gencode arch=compute_53,code=[sm_53,compute_53]
```

4. run ```make``` command on your terminal (while you're still in the ```darknet``` folder).

5. An executable named ```darknet``` will be created inside the folder.

## II. Training of the model:
### 1. Model configuration & dataset:
We chose to use ```YOLOv4-tiny``` architecture for this task.  

The following colab notebook [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1G188PEVGwdTbPvotCFRM_l2mluUhShwy#scrollTo=Xx5PnGZCz3fy/) shows how we prepared our dataset and split it into train and validation.  

We also added to the folder of the dataset the configuration file for the YOLO model ```yolov4-tiny.cfg``` and modified it to suit our training data as following the darknet repository:  
i. change line batch to batch=64.  
ii. change line subdivisions to subdivisions=16.  
iii. change line max_batches to (classes*2000, but not less than number of training images and not less than 6000), ```max_batches=6000```.  
iv. change line steps to 80% and 90% of max_batches, ```steps=4800,5400```.  
v. experiment with other values that are used for data augmentation like ```angle``` ```saturation``` ```exposure``` ```hue```.  
vi. experiment also with other values such as ```momentum``` ```decay``` ```learning_rate``` ```burn_in```.  
vii. set network size ```width=416``` ```height=416``` or any value multiple of 32.
ix. change line ```classes=80``` to your number of objects in each of 2 ```[yolo]``` layers:
```cfg
[yolo]
...
classes=2
...
```
x. change [```filters=255```] to filters=(classes + 5)x3 in the 2 ```[convolutional]``` before each ```[yolo]``` layer, keep in mind that it only has to be the last ```[convolutional]``` before each of the ```[yolo]``` layers.  
So if ```classes=1``` then should be ```filters=18```. If ```classes=2``` then write ```filters=21```.  
(Do not write in the cfg-file: filters=(classes + 5)x3)

```cfg
[convolutional]
...
filters=21
...

[yolo]
...
classes=2
...
```

Content of the file ```cheating.data``` should be:
```Vim
classes = 2
train  = <replace with your path>/train.txt
valid = <replace with your path>/valid.txt
names = cheating.names
backup = backup
```


### 3. Training on colab:
Using this colab notebook [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1D4iJc1YrJQu-HGLapft7IjGYrwMHJVOH/) we trained our YOLO model.


## III. Testing and deployment:
Unlike for a computer camera (or USB camera), when we use Nividia Jetson Nano with a Raspberry Pi camera we have to use a ```gstreamer link``` instead of ```0``` (or ```usb/video0```) to read frames with OpenCV.

```Shell
./darknet detector demo cheating.data model.cfg model.weights " nvarguscamerasrc ! video/x-raw(memory:NVMM), width=(int)1920, height=(int)1080,format=(string)NV12, framerate=(fraction)30/1 ! nvvidconv flip-method=0 ! video/x-raw, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! Appsink"
```
