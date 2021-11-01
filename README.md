# BananaPi M2 zero + OV5640

## A Banan Pi image with OV5640 camera and OpenCV
![output image]( https://qengineering.eu/images/armbian.png )<br/><br/>
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)<br/><br/>

------------

## Installation.

- Get a 16 GB (minimal) SD-card which will hold the image. 
- Download the image (**5.1 GByte!**) from our [Gdrive](https://drive.google.com/file/d/19gT6Okn-u_mnab_e0JZHuNyq_EDz9DJI/view?usp=sharing) site. 
- Flash the image on the SD card with the [Imager](https://www.raspberrypi.org/software/) or [balenaEtcher](https://www.balena.io/etcher/).
- Insert the SD card in your Banana Pi and enjoy.
- Password: ***pass1234***
- Read the instructions below!<br/>
![output image]( https://qengineering.eu/images/BananaPiM2zero.webp )<br/>
------------

## Good to know.

* The Banana Pi M2 zero has only 512 MByte RAM onboard, which limits the performance even with the quad-core A7 Allwinner H2+. Don't expect high-resolution live video, although we managed to transfer a 1280x720 UDP stream at 30 FPS using GStreamer.
* This Armbian OS is the first we tested that keeps the CPU temperature fairly cold. All other BPi operating systems heats the CPU at 65°C or higher, even when idle.
* We limited the screen resolution to 1280x720 to avoid flickering.
* If you require extra space, you can delete the ~/opencv and /usr/src (1.9 GB) folder from the SD card. There are no longer needed since all libraries are in the /usr directory. The /usr/src keeps all the code required for building the Linux kernel.
* Use a tool like [GParted](https://gparted.org/) `sudo apt-get install gparted` to expand the image to larger SD cards.<br/><br/>

------------

## OV5640.

First, we would like to thank **Wim van ‘t Hoog** for the many hours of work rebuilding the Linux device tree on the Banana Pi to get the OV5640 drivers installed.
Please visit his [website](https://wvthoog.nl/nanopi-ov5640-camera/), if you want more information on the subject. Also, if you like to get the Cedrus encoder (used for FFmpeg and GStreamer) up and running. Note, Wim is using the NanoPi with the Allwinner H3, instead of the BananaPi with the H2+.<br/><br/>
![output image]( https://qengineering.eu/images/OV5640.webp )<br/><br/>


<!--
## Pre-installed frameworks.

- [JetPack](https://developer.nvidia.com/embedded/jetpack) 4.6.0
- [OpenCV](https://qengineering.eu/deep-learning-with-opencv-on-raspberry-pi-4.html) 4.5.3
- [TensorFLow](https://qengineering.eu/install-tensorflow-2.4.0-on-raspberry-64-os.html) 2.4.1
- [TensorFlow Addons](https://qengineering.eu/install-tensorflow-2.4.0-on-raspberry-64-os.html) 0.13.0-dev
- [Pytorch](https://qengineering.eu/install-pytorch-on-raspberry-pi-4.html) 1.8.1
- [TorchVision](https://qengineering.eu/install-pytorch-on-raspberry-pi-4.html) 0.9.1
- [LibTorch](https://qengineering.eu/install-pytorch-on-raspberry-pi-4.html) 1.8.1 
- [ncnn](https://qengineering.eu/install-ncnn-on-jetson-nano.html) 20210720
- [MNN](https://qengineering.eu/install-mnn-on-jetson-nano.html) 1.2.1
- [JTOP](https://github.com/rbonghi/jetson_stats) 3.1.1 

Tensorflow 2.5 and above require CUDA 11. CUDA version 11 cannot be installed on a Jetson Nano due to incompatibility between the GPU and low-level software at this time, hence Tensorflow 2.4.1. Only when NVIDIA releases a JetPack with CUDA 11 will we be able to upgrade Tensorflow.

![output image]( https://qengineering.eu/images/Software_Jetson.png )<br/><br/>
![output image]( https://qengineering.eu/images/JTOP_jetson.png )

------------

## OpenCV + TensorFlow.

Importing both TensorFlow and OpenCV in Python can throw the error: _cannot allocate memory in static TLS block_.<br/>
This behaviour only occurs on an aarch64 system and is caused by the OpenMP memory requirements not being met.<br/>
For more information, see GitHub ticket [#14884](https://github.com/opencv/opencv/issues/14884).<br/>

![output image](https://qengineering.eu/images/SwapImportOpenCVJetson.webp)

There are a few solutions. The easiest is to import OpenCV at the beginning, as shown above.<br/>
The other is disabling OpenMP by setting the -DBUILD_OPENMP and -DWITH_OPENMP flags OFF.<br/>
Where possible, OpenCV will now use the default pthread or the TBB engine for parallelization.<br/>
We don't recommend it. Not all OpenCV algorithms automatically switch to pthread.<br/>
Our advice is to import OpenCV into Python first before anything else.<br/>

-->
Under construction

