# Banana Pi M2 zero + OV5640

## A Banana Pi image with OV5640 camera and OpenCV
![output image]( https://qengineering.eu/images/armbian.png )<br/><br/>
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)<br/><br/>

------------

## Installation.

- Get a 16 GB (minimal) SD-card which will hold the image. 
- Download the image (**5.1 GByte!**) from our [Gdrive](https://drive.google.com/file/d/19gT6Okn-u_mnab_e0JZHuNyq_EDz9DJI/view?usp=sharing) site. 
- Flash the image on the SD card with the [Imager](https://www.raspberrypi.org/software/) or [balenaEtcher](https://www.balena.io/etcher/).
- Insert the SD card in your Banana Pi and enjoy.
- Login: pi
- Password: ***pass1234***
- Read the instructions below!<br/><br/>
![output image]( https://qengineering.eu/images/BananaPiM2zero_2.webp )<br/>
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
Please visit his [website](https://wvthoog.nl/nanopi-ov5640-camera/), if you want more information on the subject. Also, if you like to get the Cedrus encoder (used for FFmpeg and GStreamer) up and running. Note, Wim is using the Nano Pi with the Allwinner H3, instead of the Banana Pi with the H2+.<br/><br/>
![output image]( https://qengineering.eu/images/OV5640_2.webp )<br/><br/>
#### :point_right: You **cannot** use a Raspberry Pi camera! The OV56**40** has a parallel output port, while the RPi OV56**47** has a MIPI-lane interface.<br/>
:point_right: There are several connector layouts of the OV5640 camera. Buy one specifically for the Banana Pi, like [this one](https://nl.aliexpress.com/item/32660117929.html).<br/><br/>
Before using the ov5640 camera, it must be initialized. It is done by the command `$ sudo media-ctl --device /dev/media1 --set-v4l2`<br/>
The command allows you to set the resolution, framerate, compression and other parameters. You can issue a new command at any time to change the parameters. You can also set the initialization during boot, for example by placing the command in `/etc/rc.local`.<br/>
The camera is located at `/dev/video1`, not `/dev/video0` where the cedrus engine lives.<br/>
Some examples
```
$ sudo media-ctl --device /dev/media1 --set-v4l2 '"ov5640 2-003c":0[fmt:YUYV8_2X8/1280x720]'
or
$ sudo media-ctl --device /dev/media1 --set-v4l2 '"ov5640 2-003c":0[fmt:YUYV8_2X8/640x480]'
```
With framerate:
```
$ sudo media-ctl --device /dev/media1 --set-v4l2 '"ov5640 2-003c":0[fmt:YUYV8_2X8/640x480@1/30]'
or
$ sudo media-ctl --device /dev/media1 --set-v4l2 '"ov5640 2-003c":0[fmt:YUYV8_2X8/1280x720@1/15]'
```
### GStreamer
You can use GStreamer. However, be aware of the limited resources on the Banana Pi M2 zero.<br/>
Streaming to screen:
```
$ gst-launch-1.0 v4l2src device=/dev/video1 ! video/x-raw, width=640, height=480, framerate=30/1 ! videoconvert ! autovideosink
```
UDP streaming to another computer. (192.168.178.84 is the address of the recieving host)
```
$ gst-launch-1.0 -v v4l2src device=/dev/video1 num-buffers=-1 ! video/x-raw, width=1280, height=720, framerate=30/1 ! videoconvert ! jpegenc ! rtpjpegpay ! udpsink host=192.168.178.84 port=5200
```
### OpenCV
Once the camera is set up, you can receive the video stream in OpenCV for further processing.<br/>
We've added an example to the SD image. It is almost identical to the OpenCV camera example for the Raspberry Pi. See our [webpage](https://qengineering.eu/opencv-c-examples-on-raspberry-pi.html) for more information.<br/><br/>
![output image]( https://qengineering.eu/images/BananaStreet.webp )<br/><br/>

------------

### Python 
Again, you need to initialize the camera beforehand.<br/>
`$ sudo media-ctl --device /dev/media1 --set-v4l2 '"ov5640 2-003c":0[fmt:YUYV8_2X8/640x480]'`<br/>
Then you can run this little Python demo program to capture your OV5640.<br/>
```
import cv2
import numpy as np

# Create a VideoCapture object and read from input file
# If the input is the camera, pass 0 instead of the video file name
cap = cv2.VideoCapture(1)

# Check if camera opened successfully
if (cap.isOpened()== False): 
  print("Error opening video stream or file")

# Read until video is completed
while(cap.isOpened()):
  # Capture frame-by-frame
  ret, frame = cap.read()
  if ret == True:

    # Display the resulting frame
    cv2.imshow('Frame',frame)

    # Press Q on keyboard to  exit
    if cv2.waitKey(25) & 0xFF == ord('q'):
      break

  # Break the loop
  else: 
    break

# When everything done, release the video capture object
cap.release()

# Closes all the frames
cv2.destroyAllWindows()
```
--------

## Pre-installed frameworks.

- [Armbian_21.02.1_Bananapim2zero_buster_current_5.10.12_desktop.img.xz](https://armbian.hosthatch.com/archive/bananapim2zero/archive/)
- [OpenCV Lite](https://qengineering.eu/install-opencv-lite-on-raspberry-pi.html) 4.5.4
- [Code::Blocks](https://qengineering.eu/opencv-c-examples-on-raspberry-pi.html)

![output image]( https://qengineering.eu/images/Banana_OS_2.png )<br/><br/>
![output image]( https://qengineering.eu/images/MediaBananaPi.webp )<br/><br/>

--------

## Tips.
If you want to change the password, here are the commands( <username>=pi ).<br/>
```
$ sudo -i
# passwd <username>
New password: 
Retype new password: 
passwd: password updated successfully
# exit
$ sudo reboot
```
