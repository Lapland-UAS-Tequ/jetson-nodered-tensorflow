# jetson-nodered

This guide is for using node-red-contrib-cloud-annotations-gpu node on Node-RED and making predicitions with Tensorflow. At the moment of writing this tfjs-node depends on libtensorflow version 1.15.0, so downgrading Cuda on Jetson Xavier NX is necessary to make things work. If you are using Jetson Nano, you can install Jetpack 4.3 and skip removing and installing Cuda in this guide.

After running all commands you should have following versions of the components

| Software      | Version       | 
| ------------- |:-------------:| 
| Jetpack       | 4.5.1         | 
| Cuda          | 10.0.326      |  
| cuDNN         | 7.6.3.28	     | 
| tfjs-node-gpu | 3.3.0	        | 
| libtensorflow | 1.15.0		      | 
| node-red	     | 1.3.1	        |
| node-red-contrib-cloud-annotations | 0.0.5 |


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

```mkdir cuda_files```

```cd /home/cuda_files```

4. Download new cuda & cuDnn files

```wget```


```wget```


5. Install Cuda 10

```sudo dpkg -i cuda-repo-l4t-8-0-local_8.0.34-1_arm64.deb```

```sudo apt update```

```sudo apt search cuda```

```sudo apt install cuda-toolkit-10.0```

```sudo apt install cuda-samples-10.0```

```export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}```

```export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}``` 

6. Install cuDnn

```sudo apt install ./libcudnn7_7.6.3.28-1+cuda10.0_arm64.deb```

Check with jtop that everything is installed correctly 

```jtop```





