# jetson-nodered
Installing node-red-contrib-cloud-annotations-gpu to Nvidia Jetson

This guide is for using node-red-contrib-cloud-annotations-gpu node on Node-RED and making predicitions with tensorflow.

After running all commands you should have following versions of the components

Jetpack	4.5.1	
Cuda 	10.0.326	
cuDNN	7.6.3.28	
tfjs-node-gpu 3.3.0	
libtensorflow	1.15.0	
node-red	1.3.1	
node-red-contrib-cloud-annotations 0.0.5



1. Install Jetpack 4.5.1 for Jetson NX Xavier
https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#setup

run
sudo apt update && sudo apt upgrade

run
sudo apt purge cuda-tools-10-2 libcudnn8 cuda-documentation-10-2 cuda-samples-10-2 nvidia-l4t-graphics-demos ubuntu-wallpapers-bionic libreoffice* chromium-browser* thunderbird fonts-noto-cjk

run
sudo apt autoremove

run
sudo reboot

Download Cuda 10 & cuDnn files
wget 

Install Cuda 10
sudo dpkg -i cuda-repo-l4t-8-0-local_8.0.34-1_arm64.deb
sudo apt update
sudo apt search cuda 
sudo apt install cuda-toolkit-10.0
sudo apt install cuda-samples-10.0
export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}} 

Install cuDnn
sudo apt install ./libcudnn7_7.6.3.28-1+cuda10.0_arm64.deb

Check with jtop that everything is installed correctly





