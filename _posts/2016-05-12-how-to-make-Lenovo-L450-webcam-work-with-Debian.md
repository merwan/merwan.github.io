---
layout: post
title: How to mage Lenovo L450 webcam work with Debian
tags:
- Debian
---

My Lenovo L450 webcam does not work out of the box on my Debian machine.
To make it work, I have to patch and build a new module for `uvcvideo` every time I update the linux image.

# Setup

I'm running Debian testing on a Lenovo L450. The following instructions are valid for the webcam ID `5986:055a`.
You can check the webcam device ID on your machine using `lsusb`:

```
$ lsusb
(...)
Bus 002 Device 005: ID 5986:055a Acer, Inc
(...)
```

# Download patch

```
mkdir -p ~/debian/src
cd ~/debian/src
wget -O webcam.patch https://launchpadlibrarian.net/229627414/0001-uvcvideo-Acer-Integrated-Camera-5986-055a-add-UVC_QU.patch
```

# Get the kernel source

```
apt-get source linux-image-$(uname -r)
cd linux-* # Should be linux-image source folder
cp ~/debian/src/webcam.patch ./
```

# Apply the patch

```
patch -p1 < webcam.patch
cd drivers/media/usb/uvc/
```

# Compile the module

```
sudo apt-get install linux-headers-$(uname -r)
make -C /lib/modules/$(uname -r)/build M=$(pwd) modules
```

# Install the module

```
sudo make -C /lib/modules/$(uname -r)/build M=$(pwd) modules_install
```

If depmod from modules_install fails, redo it:

```
sudo depmod
```

# Unload/load the module

```
sudo rmmod uvcvideo
sudo modprobe uvcvideo
```

The camera should now work properly.
