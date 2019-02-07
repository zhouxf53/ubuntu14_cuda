# Summary 
Install CUDA 9.0, tensorflow-gpu 1.12.0, Anaconda 2018.12, and Pycharm on a Ubuntu 14.04 station

# Installaion steps
## Re-image the station with Ubuntu 14.04
Simply following the bootable device's instruction, upon finishing, proceed to the login screen but do not login yet.

## 1. Remove/uninstall the current NVIDIA driver, CUDA, and cuDNN
Press `crtl+alt+F1`, which is refered as "hard terminal" afterwards, start removing the pre-installed nvidia driver and CUDA directories, if any.
Check if NVIDIA System Management Interface gives any output, if yes, it means driver is pre installed
```
nvidia-smi
```
Check CUDA runtime version, again, if gives any output, CUDA is pre-installed
```
nvcc --version
```
Start to remove driver
```
sudo apt remove --purge nvidia*
sudo apt-get autoremove
```
Start to remove CUDA, replace X.Y with the version number you already have, such as 8.0
```
cd /usr/local
sudo rm -r cuda 
sudo rm -r cuda-X.Y
```
(Optional) Reboot and attempt to login, should be able to login but the resolution may be weird because you have no valid driver now

## 2. Install NVIDIA driver
Logout and *go back to hard-terminal*, do not attempt to install it under GUI interface, you may need to add addtional repo by 
```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
```
install nvidia driver version 375 which is a a transitional package for 410 at this time, directly using 384 or 410 is not working in my case
```
sudo apt-get install nvidia-375
```
Reboot 
```
sudo reboot
```
And attempt to login, should be able to login with no issue wahat so ever, check if NVIDIA System Management Interface gives any output, if shows 410, it means the installation is correct. Note here the CUDA driver version is 10.0, but it is ok.
```
nvidia-smi
```
*When smi gives issue such as:*
https://gist.github.com/beci/2a2091f282042ed20cda
*upgrade gcc to 5*

## 3. Download CUDA and cuDNN packages
In this case, CUDA version is 9.0 and cuDNN is 7.4.2, check the following websites, in this tutorior, the CUDA file is downloaded as cuda_9.0.176_384.81_linux.run and cuDNN is cudnn-9.0-linux-x64-v7.3.1.20.solitairetheme8 from the browser, you can also use `wget` if possible. By default, both files are downloaded to ``~/Download`` folder
https://developer.nvidia.com/cuda-toolkit-archive

(NVIDIA account needed) https://developer.nvidia.com/cudnn 

## 4. Install CUDA
In a normal terminal or hard terminal:
```
cd ~/Downloads
sudo bash ./cuda_9.0.176_384.81_linux.run
```
A CUDA run file consists of 
- an NVIDIA driver installer, but usually of stale version;
- the actual CUDA installer;
- the CUDA samples installer;
Skip the driver installaion because we already installed it by selecting *no* under prompt. Others all yes and by default.
Check CUDA runtime version to see if it is really is 9.0
```
nvcc --version
```
### (optional) Build CUDA sample to test 
TODO: add content
https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html

## 5. Install cuDNN
Move cuDNN to CUDA folder then extract, it would save you a lot of trouble.
```
sudo mv ~/Downloads/cudnn-9.0-linux-x64-v7.3.1.20.solitairetheme8 /usr/local
```
Extract
```
cd /usr/local
sudo tar -xzvf cudnn-9.0-linux-x64-v7.3.1.20.solitairetheme8
```
Confirm cuDNN version and instalation
```
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
```
## 6. Add CUDA include to PATH
Edit the ~/.bashrc file
```
sudo gedit ~/.bashrc
```
copy and paste to the end of the file
```
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64"
export CUDA_HOME=/usr/local/cuda
export PATH="$PATH:/usr/local/cuda/bin"
```
Reload it
```
source ~/.bashrc
```
## 7 Add CUDA library to lib_path
Sometimes adding lib_path through editing ~/.bashrc could only affect terminal enviroment. In order for Pycharm to recognize the CUDA library, you need to edit a .conf file.
Create a file named as cuda.conf in anywhere convenient such as ~/Download directory, and adding the following content
```
/usr/local/cuda/lib64
```
Move it to /etc/ld.so.conf.d and config it
```
sudo mv ~/Downloads/cuda.conf /etc/ld.so.conf.d
sudo ldconfig
```
Validate the output from the system
```
echo $LD_LIBRARY_PATH
```

## 8. Download Anaconda from the official website
https://www.anaconda.com/distribution/

## 9. Install Anaconda and create new enviroment
Install anaconda, press s to skip terms, d to fast forward. All options yes.
```
cd ~/Download
sudo bash Anaconda3-2018.12-Linux-x86_64.sh
source ~/.bashrc
```
create a new virtual envrioemnt with the name conda_tf and python version 3.5 (optional)
```
conda create -n conda_tf python=3.5
```
## 10. Install tensorflow-gpu and test
Activate the enviroment and install tensorflow from pip
```
conda activate conda_tf
pip install tensorflow-gpu==1.12.0
```
Enter pytho and test
```
python
# python commands   
import tensorflow as tf   
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```
## 11. Install SNAP

## 12. Install Pycharm





