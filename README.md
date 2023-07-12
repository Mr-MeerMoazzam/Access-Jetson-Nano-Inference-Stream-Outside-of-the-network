# Access Jetson Nano Inference Stream Outside of the Network

This repository provides a solution to access the inference stream of a Jetson Nano device from outside of the network. By following the instructions and using the provided scripts, you can securely access the Jetson Nano's inference stream remotely, allowing you to monitor and analyze the real-time inference results from anywhere.

## Table of Contents

- [Introduction](#introduction)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Security](#security)
- [Contributing](#contributing)
- [License](#license)

## Introduction

The Jetson Nano is a powerful single-board computer designed for AI and machine learning applications. It is commonly used for inference tasks, where it can process real-time data and make predictions using trained models. However, accessing the Jetson Nano's inference stream from outside of the network can be challenging due to network configurations and security concerns.

This repository provides a step-by-step guide and scripts to set up a secure and reliable connection to the Jetson Nano's inference stream, allowing you to access it remotely without compromising security.

## Requirements

To use this solution, you need the following:

1. Jetson Nano device with an Internet connection.
2. A remote computer or device with an Internet connection to access the inference stream.
3. Basic knowledge of Linux commands and network configurations.
4. Must cover all the Environment Steps
## Environment Steps
1. Create a  virtual environment in jetson nano
  ``` sudo apt-get install python3-pip 

pip3 install virtualenv 

python3 -m virtualenv -p python3 env --system-site-packages    

source env/bin/activate```

2. Check swap space

```free -h```

3. Create a SwapFile by running the following commands one by one:
```sudo fallocate -l 4G /var/swapfile 

sudo chmod 600 /var/swapfile

sudo mkswap /var/swapfile

sudo swapon /var/swapfile

sudo bash -c 'echo "/var/swapfile swap swap defaults 0 0"  >> /etc/fstab' 
```

4. Reboot your PC
```sudo reboot```
5. Now check swap space  by running this command
```free -h```
6. Install these Dependencies before installing OpenCV:
``` sudo sh -c "echo '/usr/local/cuda/lib64' >> /etc/ld.so.conf.d/nvidia-tegra.conf"

sudo ldconfig

sudo apt-get install build-essential cmake git unzip pkg-config

sudo apt-get install libjpeg-dev libpng-dev libtiff-dev

sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev

sudo apt-get install libgtk2.0-dev libcanberra-gtk*

sudo apt-get install python3-dev python3-numpy python3-pip

sudo apt-get install libxvidcore-dev libx264-dev libgtk-3-dev

sudo apt-get install libtbb2 libtbb-dev libdc1394-22-dev

sudo apt-get install libv4l-dev v4l-utils

sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev

sudo apt-get install libavresample-dev libvorbis-dev libxine2-dev

sudo apt-get install libfaac-dev libmp3lame-dev libtheora-dev

sudo apt-get install libopencore-amrnb-dev libopencore-amrwb-dev

sudo apt-get install libopenblas-dev libatlas-base-dev libblas-dev

sudo apt-get install liblapack-dev libeigen3-dev gfortran

sudo apt-get install libhdf5-dev protobuf-compiler

sudo apt-get install libprotobuf-dev libgoogle-glog-dev libgflags-dev
```

7. Download OpenCV
``` cd ~

wget -O opencv.zip https://github.com/opencv/opencv/archive/4.5.1.zip 

wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.5.1.zip 

unzip opencv.zip 

unzip opencv_contrib.zip
```

8. Now rename the directories and remove the unnecessary files
```mv opencv-4.5.1 opencv

mv opencv_contrib-4.5.1 opencv_contrib

rm opencv.zip

rm opencv_contrib.zip
```

9. Build OpenCV
```cd ~/opencv

mkdir build

cd build ```

10. Execute the entire block of commands below into your terminal.
``` cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules -D EIGEN_INCLUDE_PATH=/usr/include/eigen3 -D WITH_OPENCL=OFF -D WITH_CUDA=ON -D CUDA_ARCH_BIN=5.3 -D CUDA_ARCH_PTX="" -D WITH_CUDNN=ON -D WITH_CUBLAS=ON -D ENABLE_FAST_MATH=ON -D CUDA_FAST_MATH=ON -D OPENCV_DNN_CUDA=ON -D ENABLE_NEON=ON -D WITH_QT=OFF -D WITH_OPENMP=ON -D WITH_OPENGL=ON -D BUILD_TIFF=ON -D WITH_FFMPEG=ON -D WITH_GSTREAMER=ON -D WITH_TBB=ON -D BUILD_TBB=ON -D BUILD_TESTS=OFF -D WITH_EIGEN=ON -D WITH_V4L=ON -D WITH_LIBV4L=ON -D OPENCV_ENABLE_NONFREE=ON -D INSTALL_C_EXAMPLES=OFF -D INSTALL_PYTHON_EXAMPLES=OFF -D BUILD_NEW_PYTHON_SUPPORT=ON -D BUILD_opencv_python3=TRUE -D OPENCV_GENERATE_PKGCONFIG=ON -D BUILD_EXAMPLES=OFF ..```

11. Build OpenCV (will take a long time around 2 hours)
``` make -j4```

12. Finish the install:
```cd ~

sudo rm -r /usr/include/opencv4/opencv2

cd ~/opencv/build

sudo make install

sudo ldconfig

make clean

sudo apt-get update ```
13. Verify OpenCV Installation
```
python -c 'import cv2; print(cv2.__version__)
```

14. Install GStreamer and GObject introspection

```sudo apt-get install -y libgstreamer1.0-dev libgirepository1.0-dev
```
15 Install PyGObject

```pip install pygobject```

## Installation

Follow these steps to install and configure the solution:

1. Clone this repository to your local machine:

```shell
git clone https://github.com/your-username/Access-Jetson-Nano-Inference-Stream-Outside-of-the-network.git
