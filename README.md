# Banana Pi M2 zero + OV5640

## A Banana Pi image with OV5640 camera, WiringPi and OpenCV
![output image]( https://qengineering.eu/images/armbian.png )<br/><br/>
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)<br/><br/>

------------

## Installation.

- Get a 16 GB (minimal) SD-card which will hold the image. 
- Download the image (**5.5 GByte!**) from our [Gdrive](https://drive.google.com/file/d/1oQrsXJ0fXAyKS1lGSZAyrpy-uXZCaG8w/view?usp=sharing) site. 
- Flash the image on the SD card with the [Imager](https://www.raspberrypi.org/software/) or [balenaEtcher](https://www.balena.io/etcher/).
- Insert the SD card in your Banana Pi and enjoy.
- Login: ***pi***
- Password: ***pass1234***
- ***Do not*** `$ sudo apt-get upgrade` as the newly installed software will ***remove the OV5640 drivers***!
- Read the instructions below!<br/><br/>

------------

### Split image.

Because the image is large (5.5 GB), the download may take quite some time. It makes downloading vulnerable.<br/>
That's why we split the file into smaller chunks. These are more manageable than one huge download.<br/>
If you prefer this partial download over one large one, download the following 12 files (500 MB each) and place them in one folder.</br>
- [BananaPi_M2_zero_OV5640.7z.001](https://drive.google.com/file/d/1sEcsguK7ymAknjKK56Fwv1VAeLO4M2jp/view?usp=sharing)
- [BananaPi_M2_zero_OV5640.7z.002](https://drive.google.com/file/d/1Qcl9ElFSOCt0E9v9iCzAKmb1HQv-Ul2U/view?usp=sharing)
- [BananaPi_M2_zero_OV5640.7z.003](https://drive.google.com/file/d/1h8ytekk0tzd3aWJbjXkUYTNrG3meD-Ua/view?usp=sharing)
- [BananaPi_M2_zero_OV5640.7z.004](https://drive.google.com/file/d/1aWZ1u87SOcxqgM4sU1t7sRvgtuzxO-0g/view?usp=sharing)
- [BananaPi_M2_zero_OV5640.7z.005](https://drive.google.com/file/d/1P4PUvnXAqogC9twRKL3ogJ1VumXEkghF/view?usp=sharing)
- [BananaPi_M2_zero_OV5640.7z.006](https://drive.google.com/file/d/1R1rWaP5yrtmfHkhxR4Sp1NE-qY97a3eA/view?usp=sharing)
- [BananaPi_M2_zero_OV5640.7z.007](https://drive.google.com/file/d/1o3Tb2MqQz8Ayakuega1ApcrKQkViCS2X/view?usp=sharing)
- [BananaPi_M2_zero_OV5640.7z.008](https://drive.google.com/file/d/1pK0COokVk5grMYC_taOZg4sJ5g7NVx08/view?usp=sharing)
- [BananaPi_M2_zero_OV5640.7z.009](https://drive.google.com/file/d/1ys7TvsZ38RJ-z9HeEnQbatEsHuiD68yu/view?usp=sharing)
- [BananaPi_M2_zero_OV5640.7z.010](https://drive.google.com/file/d/1CqOTKp8QynT_4q_33CcMJAR6xHVF6L1i/view?usp=sharing)
- [BananaPi_M2_zero_OV5640.7z.011](https://drive.google.com/file/d/15j_NZ65VaSOfZl2O-eq6e5aAITlLUnEU/view?usp=sharing)
- [BananaPi_M2_zero_OV5640.7z.012](https://drive.google.com/file/d/128drF7RwU5NZi1IQ6076nRT1wvqmmm0A/view?usp=sharing)<br/>

Once you have all the files run
```
7z x BananaPi_M2_zero_OV5640.7z.001
```
7Z will start extracting the first file (`*.001`) and then automatically the next files in order.</br>
You will endup with `BananaPi_M2_zero_OV5640.zip`, the original image which you now can flash on a SD card with [Imager](https://www.raspberrypi.org/software/) or [balenaEtcher](https://www.balena.io/etcher/).<br/><br/>
If you get the error `'7z' is not recognized as an internal or external command, operable program or batch file.` please give the full path to 7z. For instance,
```
"C:\Program Files\7-Zip\7z.exe" x BananaPi_M2_zero_OV5640.7z.001
```



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
#### :point_right: There are several connector layouts of the OV5640 camera. Buy one specifically for the Banana Pi, like [this one](https://nl.aliexpress.com/item/32660117929.html).<br/>
![output image]( https://qengineering.eu/github/Connectors2.png )<br/>
With the wrong connector you are facing [issue #6](https://github.com/Qengineering/BananaPi-M2-Zero-OV5640/issues/6).<br/><br/>
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

--------

## Pre-installed frameworks.

- [Armbian_21.02.1_Bananapim2zero_buster_current_5.10.12_desktop.img.xz](https://armbian.hosthatch.com/archive/bananapim2zero/archive/)
- [BPI-WiringPi2](https://forum.banana-pi.org/t/banana-pi-m2-zero-wiringpi2/5517/7) 
- [OpenCV Lite](https://qengineering.eu/install-opencv-lite-on-raspberry-pi.html) 4.5.4
- [Code::Blocks](https://qengineering.eu/opencv-c-examples-on-raspberry-pi.html) 16.01

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

------------

[![paypal](https://qengineering.eu/images/TipJarSmall4.png)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=CPZTM5BB3FCYL) 
