# CheatingDetectionJetsonNano
Cheating Detection using the Nvidia Jetson Nano and the YOLO Darknet framework.


## Setting up Jetson Nano:
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

4. Uncomment this line in the ```Makefile```:
```Makefile
ARCH= -gencode arch=compute_53,code=[sm_53,compute_53]
```

4. run ```make``` command on your terminal (while you're still in the ```darknet``` folder).

5. An executable named ```darknet``` will be created inside the folder.

## 2. Training of the model:
using this colab notebook [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1D4iJc1YrJQu-HGLapft7IjGYrwMHJVOH/) add
