# CheatingDetectionJetsonNano
Cheating Detection using the Nvidia Jetson Nano and the YOLO Darknet framework.


Make sure cuda path
```Shell
 nano /home/$USER/.bashrc
```
Add these lines to ```.bashrc```
```Vim
 export PATH="/usr/local/cuda-X.X/bin:$PATH"
 export LD_LIBRARY_PATH="/usr/local/cuda-X.X/lib64:$LD_LIBRARY_PATH"
```


Clone AlexeyAB's Darknet repository:

```Shell
git clone https://github.com/AlexeyAB/darknet
```

```Makefile
GPU=1
CUDNN=1
CUDNN_HALF=0
OPENCV=1
AVX=0
OPENMP=0
LIBSO=1
ZED_CAMERA=0
ZED_CAMERA_v2_8=0
```

And uncomment this line in the Makefile
```Makefile
ARCH= -gencode arch=compute_53,code=[sm_53,compute_53]
```
