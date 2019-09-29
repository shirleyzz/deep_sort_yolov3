QQ group: 姿态检测＆跟踪 781184396
# Set up Cuda and cudnn on  Ubuntu 16.04/18.04
$ sudo apt-get update

$ sudo apt-get upgrade
$ sudo apt-get dist-upgrade
$ sudo apt-get install git curl cmake
#Install nvidia drivers: Ubuntu 18.04 chooose nvidia-390, Ubuntu 16.04 choose nvidia-375
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt-get update
Step 1: Go to tty1 virtual console :Press Ctrl + Alt + F1
Step 2: Install Nvidia driver (nvidia-375 for Ubuntu 16.04)
$ sudo apt-get install nvidia-375
$ sudo reboot
Step 3: Check
$ nvidia-smi

#Install NVIDIA CUDA toolkit: first download cuda 9.0
$ cd ~/Downloads # assuming that the install files are here
$ sudo sh cuda_9.0.176_384.81_linux.run
Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 384.81?
(y)es/(n)o/(q)uit: n
Others: choose yes

#Install NVIDIA cuDNN: downlaod cudnn-7.3.1 for cuda 9.0
$ sudo tar -xzvf cudnn-9.0-linux-x64-v7.4.1.5.tgz
$ sudo cp cuda/include/cudnn.h /usr/local/cuda/include
$ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
#Setup your paths
$ echo 'export PATH=/usr/local/cuda-9.0/bin:$PATH' >> ~/.bashrc
$ echo 'export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
$ source ~/.bashrc
$ sudo ldconfig

#Verify
$ nvidia-smi
$ nvcc -V
$ cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2

# Reference
1. https://medium.com/@anujonthemove/deep-learning-environment-setup-on-ubuntu-16-04-83078e1cba1f
2. https://gist.github.com/Mahedi-61/2a2f1579d4271717d421065168ce6a73

# Introduction
  Thanks for these projects, this work now is support tiny_yolo v3 but only for test, if you want to train you can either train a model in darknet or in the second following works. It also can tracks many objects in coco classes, so please note to modify the classes in yolo.py. besides, you also can use camera for testing.

  https://github.com/nwojke/deep_sort
  
  https://github.com/qqwweee/keras-yolo3
  
  https://github.com/Qidian213/deep_sort_yolov3

# Quick Start

1. Download YOLOv3 or tiny_yolov3 weights from [YOLO website](http://pjreddie.com/darknet/yolo/).Then convert the Darknet YOLO model to a Keras model. Or use what i had converted https://drive.google.com/file/d/1uvXFacPnrSMw6ldWTyLLjGLETlEsUvcE/view?usp=sharing (yolo.h5 model file with tf-1.4.0) , put it into model_data folder
2. Run YOLO_DEEP_SORT with cmd :
   ```
   python demo.py
   ```

3. (Optional) Convert the Darknet YOLO model to a Keras model by yourself:

  ```
   please download the weights at first from yolo website. 
   python convert.py yolov3.cfg yolov3.weights model_data/yolo.h5
  ```

# Dependencies

  The code is compatible with Python 2.7 and 3. The following dependencies are needed to run the tracker:

    NumPy
    sklean
    OpenCV

  Additionally, feature generation requires TensorFlow-1.4.0.

# Training the model

  To train the deep association metric model on your datasets you can reference to https://github.com/nwojke/cosine_metric_learning  approach which is provided as a separate repository.
  
  Be careful that the code ignores everything but person. For your task do not forget to change :
  
  [deep_sort_yolov3/yolo.py]   Lines 100 to 101 :
  
          if predicted_class != 'person' : 
               continue 

# Note 

  Model file model_data/mars-small128.pb need by deep_sort had convert to tensorflow-1.4.0
 
# Test only

  Speed : when only run yolo detection about 11-13 fps  , after add deep_sort about 11.5 fps (GTX1060 6G)
 
  Test result video : https://www.bilibili.com/video/av23500163/ generated by this project
 

