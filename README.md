# jetson-nodered

This guide is for using "node-red-contrib-cloud-annotations-gpu"-node or "node-red-contrib-tf-model"-node on Node-RED in Jetson Nano or Xavier NX device and making predictions with Tensorflow. Might work on Xavier AGX also, but I didnt have one to test. At the moment of writing this tfjs-node-gpu directly depends on libtensorflow version 1.15.0, so downgrading Cuda on Jetson Xavier NX is necessary to make things work. If you are using Jetson Nano, you can install Jetpack 4.3 from official NVIDIA Jetpack 4.3 image and start from list item 9.

After running all commands you should have following versions of the components

| Software      | Version       | 
| ------------- |:-------------:| 
| Jetpack       | 4.5.1         | 
| Cuda          | 10.0.326      |  
| cuDNN         | 7.6.3.28	     | 
| libtensorflow | 1.15.0		      | 
| node-red	     | 1.2.9	        |
| a) tfjs-node-gpu | 1.4.0	        | 
| a) node-red-contrib-cloud-annotations-gpu | 0.0.5 |
| b) tfjs-node | 3.3.0	        | 
| b) node-red-contrib-tf-model-annotations | 0.1.11|

## Installation

1. Install Jetpack 4.5.1 for Jetson NX Xavier

https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#setup

2. Run update & upgrade

```sudo apt update && sudo apt upgrade```

3. Remove current Cuda installation

```sudo apt purge cuda-tools-10-2 libcudnn8 cuda-documentation-10-2 cuda-samples-10-2 nvidia-l4t-graphics-demos ubuntu-wallpapers-bionic libreoffice* chromium-browser* thunderbird fonts-noto-cjk```

```sudo apt autoremove```

```sudo reboot```

4. Create folder for files

```cd /home/```

```cd /<your user name>/```

```mkdir cuda_files```

```cd /home/cuda_files```

5. Download new cuda & cuDnn files

```wget https://jetson-nodered-files.s3.eu.cloud-object-storage.appdomain.cloud/cuda-repo-l4t-10-0-local-10.0.326_1.0-1_arm64.deb```

```wget https://jetson-nodered-files.s3.eu.cloud-object-storage.appdomain.cloud/libcudnn7_7.6.3.28-1+cuda10.0_arm64.deb```

6. Install Cuda 10

```sudo dpkg -i cuda-repo-l4t-8-0-local_8.0.34-1_arm64.deb```

```sudo apt update```

```sudo apt search cuda```

```sudo apt install cuda-toolkit-10.0```

```sudo apt install cuda-samples-10.0```

```export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}```

```export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}``` 

7. Install cuDnn

```sudo apt install ./libcudnn7_7.6.3.28-1+cuda10.0_arm64.deb```

8. Install jtop and check that everything is installed correctly

```sudo apt-get install python3-pip```

```sudo -H pip3 install -U jetson-stats```

```jtop```

![alt text](https://github.com/juhaautioniemi/jetson-nodered/blob/main/images/jtop_image.JPG "jtop")

9. Install node-red (start here if you have Jetson Nano with Jetpack 4.3)

```bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)```

10. Install tensorflow 1.15.0 

https://docs.nvidia.com/deeplearning/frameworks/install-tf-jetson-platform/index.html

```sudo apt-get update```

```sudo apt-get install libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev liblapack-dev libblas-dev gfortran```

```sudo pip3 install -U pip testresources setuptools==49.6.0```

```sudo pip3 install -U numpy==1.19.4 future==0.18.2 mock==3.0.5 h5py==2.10.0 keras_preprocessing==1.1.1 keras_applications==1.0.8 gast==0.2.2 futures protobuf pybind11```

```wget https://jetson-nodered-files.s3.eu.cloud-object-storage.appdomain.cloud/tensorflow_gpu-1.15.0+nv20.1-cp36-cp36m-linux_aarch64.whl```

```sudo pip3 install tensorflow-gpu/tensorflow_gpu-1.15.0+nv20.1-cp36-cp36m-linux_aarch64.whl```

11. Check that tensorflow is working

```python3```

```import tensorflow```

![alt text](https://github.com/juhaautioniemi/jetson-nodered/blob/main/images/tf_python3_image.JPG "tf_python")


You should select either node-red-contrib-cloud-annotations-gpu or node-red-contrib-tf-model. Installing both might work, or not. If you wish to change at some point, easiest way is to uninstall npm packages and remove `~/.node-red/node_modules/@tensorflow/` folder and install again option a or option b. If there are problems uninstalling packages, you can stop node-red (node-red-stop) and remove everything manually.

For example if removing node-red-contrib-cloud-annotations-gpu and installed dependencies

```sudo rm -r -f /.node-red/node_modules/@cloud-annotations```

```sudo rm -r -f /.node-red/node_modules/node-red-contrib-cloud-annotations-gpu```

```sudo rm -r -f /.node-red/node_modules/@tensorflow/```

```Edit  /.node-red/package.json and remove reference to node-red-contrib-cloud-annotations-gpu```

## Option a) Install node-red-contrib-cloud-annotations-gpu to Node-RED

12a. Install node-red-contrib-cloud-annotations-gpu

```cd ~/.node-red```

```npm install node-red-contrib-cloud-annotations-gpu```

Installation will finish with errors. Ignore errors and continue.

13a. Move to folder tfjs-node-gpu

```cd ~/.node-red/node_modules/@tensorflow/tfjs-node-gpu/deps```

14a. Download libtensorflow 1.15.0

```wget https://jetson-nodered-files.s3.eu.cloud-object-storage.appdomain.cloud/libtensorflow-gpu-linux-arm64-1.15.0.tar.gz```

15a. Extract libtensorflow package

```tar xzvf libtensorflow-gpu-linux-arm64-1.15.0.tar.gz```

16a. Install libtensorflow package

```npm run build-addon-from-source```


## Option b) Install node-red-contrib-tf-model to Node-RED

12b. Install tfjs-node@3.3.0

```cd ~/.node-red```

```npm install @tensorflow/tfjs-node@3.3.0```

Installation will finish with errors. Ignore errors and continue.

13b. Move to folder tfjs-node

```cd ~/.node-red/node_modules/@tensorflow/tfjs-node/deps```

14b. Download libtensorflow 1.15.0

```wget https://jetson-nodered-files.s3.eu.cloud-object-storage.appdomain.cloud/libtensorflow-gpu-linux-arm64-1.15.0.tar.gz```

15b. Extract libtensorflow package

```tar xzvf libtensorflow-gpu-linux-arm64-1.15.0.tar.gz```

16b. Install libtensorflow package

```npm run build-addon-from-source```

17b. Install node-red-contrib-tf-model and useful tools

```cd ~/.node-red```
```npm install node-red-contrib-tf-model```
```npm install node-red-contrib-post-object-detection```
```npm install node-red-contrib-tf-function```


Finally Start Node-RED

```node-red-start```

Starting Node-RED and loading Tensorflow seems to take 1-2 minutes at first start.

You should see something like this in Node-RED log, if everything went well:

![alt text](
https://github.com/juhaautioniemi/jetson-nodered/blob/main/images/nodered_tf.JPG "Node-RED log")

First inference also has slow start and it takes something like ~5-30 seconds. After that it should run smoothly.

## Inference results

Inference speeds for MJPEG stream from Raspberry PI4 with HQ-camera

Streaming is started with command 

```raspivid -v -n -b 25000000 -qp 10 -md 2 -w 1920 -h 1080 -fps 25 -cd MJPEG -n -rot 180 -t 0 -o tcp://127.0.0.1:50001```

*md (mode) and w and h parameters can vary.

Raspivid MJPEG stream is parsed in Node-RED at RPI4 and rerouted to Jetson via Websocket.

| Resolution    | FPS           | Frame size    |
| ------------- |:-------------:|:-------------:| 
| 320 x 240     | 15            | ~35 kB        |
| 1280 x 720    | 10            | ~115 kB       |
| 1920 x 1080   | 8	            | ~122 kB       |
| 4000 x 600    | 8	            | ~112 kB       |
| 2028 x 1520   | 5             | ~121 kB       |
| 4056 x 1520   | 3		          | ~137 kB       |
