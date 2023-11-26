# CheatingDetectionJetsonNano
Cheating Detection using the Nvidia Jetson Nano and the YOLO Darknet framework.

### Making sure cuda is set on your Jetson Nano
Use this command on your terminal:
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

if not then run the following:
```Shell
 nano /home/$USER/.bashrc
```
And add these lines to ```.bashrc```:
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
