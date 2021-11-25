# jetson-nodered-tensorflow

This guide is for using Tensorflow 1 (tfjs-node-gpu) in Node-RED using Jetson Nano or Xavier NX device and run object detection on images. Might work on Xavier AGX also, but I didnt have one to test. At the moment of writing this, tfjs-node(-gpu) directly depends on libtensorflow version 1.15.0, so downgrading CUDA on Jetson Xavier NX is necessary to make things work. If you are using Jetson Nano, you can install Jetpack 4.3 from official NVIDIA Jetpack 4.3 image and start from list item number 9.

*****
UPDATE 31.8.2020

Newest tfjs-node-gpu versions (tested with 3.8.0 and 3.9.0) work with newer libtensorflow versions. Check this guide if you want to use Tensorflow 2.

https://github.com/Lapland-UAS-Tequ/tequ-jetson-nodered-tensorflow/
*****

After running all commands you should have following versions of the components

| Software      | Version       | 
| ------------- |:-------------:| 
| Jetpack       | 4.5.1 or 4.3  | 
| CUDA          | 10.0.326      |  
| cuDNN         | 7.6.3.28	    | 
| libtensorflow | 1.15.0		    | 
| node-red	    | 2.0.5	        |
| tfjs-node-gpu | 1.4.0	        | 

## Installation

### 1. Install Jetpack 4.5.1 for Jetson NX Xavier

https://developer.nvidia.com/embedded/learn/getting-started-jetson

### 2. Run update & upgrade

```
sudo apt update && sudo apt upgrade
```

### 3. Remove current CUDA installation

```
sudo apt purge cuda-tools-10-2 libcudnn8 cuda-documentation-10-2 cuda-samples-10-2 nvidia-l4t-graphics-demos ubuntu-wallpapers-bionic libreoffice* chromium-browser* thunderbird fonts-noto-cjk
```

```
sudo apt autoremove
```

```
sudo reboot
```

### 4. Create folder for files

```
cd /home/
```

```
cd /<your user name>/
```

```
mkdir cuda_files
```

```
cd /home/cuda_files
```

### 5. Download new CUDA & cuDNN files

```
wget https://jetson-nodered-files.s3.eu.cloud-object-storage.appdomain.cloud/cuda-repo-l4t-10-0-local-10.0.326_1.0-1_arm64.deb
```

```
wget https://jetson-nodered-files.s3.eu.cloud-object-storage.appdomain.cloud/libcudnn7_7.6.3.28-1+cuda10.0_arm64.deb
```

### 6. Install CUDA 10

```
sudo dpkg -i cuda-repo-l4t-8-0-local_8.0.34-1_arm64.deb
```

```
sudo apt update
```

```
sudo apt search cuda
```

```
sudo apt install cuda-toolkit-10.0
```

```
sudo apt install cuda-samples-10.0
```

```
export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}
```

```
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
``` 

### 7. Install cuDNN

```
sudo apt install ./libcudnn7_7.6.3.28-1+cuda10.0_arm64.deb
```

### 8. Install jtop and check that everything is installed correctly

```
sudo apt-get install python3-pip
```

```
sudo -H pip3 install -U jetson-stats
```

```
jtop
```

![alt text](https://github.com/Lapland-UAS-Tequ/jetson-nodered-tensorflow/blob/master/images/jtop_image.JPG "jtop")

### 9. Install node-red (start here if you have Jetson Nano with Jetpack 4.3)

```
bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)
```

### 10. Install tensorflow 1.15.0 

https://docs.nvidia.com/deeplearning/frameworks/install-tf-jetson-platform/index.html

```
sudo apt-get update
```

```
sudo apt-get install libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev liblapack-dev libblas-dev gfortran
```

```
sudo pip3 install -U pip testresources setuptools==49.6.0
```

```
sudo pip3 install -U numpy==1.19.4 future==0.18.2 mock==3.0.5 h5py==2.10.0 keras_preprocessing==1.1.1 keras_applications==1.0.8 gast==0.2.2 futures protobuf pybind11
```

```
wget https://jetson-nodered-files.s3.eu.cloud-object-storage.appdomain.cloud/tensorflow_gpu-1.15.0+nv20.1-cp36-cp36m-linux_aarch64.whl
```

```
sudo pip3 install tensorflow-gpu/tensorflow_gpu-1.15.0+nv20.1-cp36-cp36m-linux_aarch64.whl
```

### 11. Check that tensorflow is working in Python

```
python3
```

```
import tensorflow
```

### 12. Install tfjs-node-gpu@1.4.0 and @cloud-annotations/models-node-gpu 

```
cd ~/.node-red
```
```
npm install @tensorflow/tfjs-node-gpu@1.4.0
```
```
npm install @cloud-annotations/models-node-gpu
```

Installation will finish with errors. Ignore errors and continue.

### 13. Move to folder tfjs-node-gpu

```
cd ~/.node-red/node_modules/@tensorflow/tfjs-node-gpu/deps
```

### 14. Download libtensorflow 1.15.0

```
wget https://jetson-nodered-files.s3.eu.cloud-object-storage.appdomain.cloud/libtensorflow-gpu-linux-arm64-1.15.0.tar.gz
```

### 15. Extract libtensorflow package

```
tar xzvf libtensorflow-gpu-linux-arm64-1.15.0.tar.gz
```

### 16. Install libtensorflow package

```
sudo npm install --global node-pre-gyp
```

```
npm run build-addon-from-source
```

test 

```
cd ~/.node-red
```

```
node
```

```
var tf = require('@tensorflow/tfjs-node-gpu')
```


### 17. Install canvas for annotating images

https://www.npmjs.com/package/canvas

Install dependencies first

```
sudo apt-get install build-essential libcairo2-dev libpango1.0-dev libjpeg-dev libgif-dev librsvg2-dev
```

```
npm install canvas
```

### 18. Use Tensorflow in Node-RED

Start Node-RED

```
node-red-start
```

### 19. Import example flow 

Go to:

https://github.com/Lapland-UAS-Tequ/tequ-api-client/

Copy and import 'example-ai-detect-v2.json' to your Node-RED.

You should see something like this in Node-RED log after flow is deployed, if everything regarding to Tensorflow went well:

![alt text](
https://github.com/Lapland-UAS-Tequ/jetson-nodered-tensorflow/blob/master/images/nodered_tf.JPG "Node-RED log")

### 20. Use Tensorflow in Node-RED

Configure model folder

Inject image to flow and start detecting objects.

First inference is slow and it takes something like ~5-30 seconds. After that it should run smoothly.

![alt text](
https://github.com/Lapland-UAS-Tequ/jetson-nodered-tensorflow/blob/master/images/example-1.JPG "Node-RED log")

### 21. Custom object detection model

If you need to build your own model, you can follow this guide:

https://github.com/Lapland-UAS-Tequ/tequ-tf1-ca-training-pipeline

## 22. Some inference benchmarking 

GUI is disabled

```
sudo service gdm stop
```

```
sudo systemctl set-default multi-user.target
```

Inference speeds for MJPEG stream from Raspberry PI4 with HQ-camera

Streaming is started with command 

```
raspivid -v -n -b 25000000 -qp 10 -md 2 -w 1920 -h 1080 -fps 25 -cd MJPEG -n -rot 180 -t 0 -o tcp://127.0.0.1:50001
```

*md (mode) and w and h parameters can vary.

Raspivid MJPEG stream is parsed in Node-RED at RPI4 and rerouted to Jetson via Websocket.

NVIDIA Jetson Xavier NX

| Resolution    | FPS           | Frame size    |
| ------------- |:-------------:|:-------------:| 
| 320 x 240     | 15            | ~35 kB        |
| 1280 x 720    | 10            | ~115 kB       |
| 1920 x 1080   | 8	            | ~122 kB       |
| 4000 x 600    | 8	            | ~112 kB       |
| 2028 x 1520   | 5             | ~121 kB       |
| 4056 x 1520   | 3		          | ~137 kB       |

NVIDIA Jetson Nano

| Resolution    | FPS           | Frame size    |
| ------------- |:-------------:|:-------------:| 
| 320 x 240     | 8             | ~35 kB        |
| 1280 x 720    | 6             | ~115 kB       |
| 1920 x 1080   | 5	            | ~122 kB       |
| 4000 x 600    | 4             | ~112 kB       |
| 2028 x 1520   | 3             | ~121 kB       |
| 4056 x 1520   | 2		          | ~137 kB       |


