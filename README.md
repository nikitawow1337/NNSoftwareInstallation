# Windows Ubuntu subsystem installation guide

This guide contains similar steps as described [by NVIDIA](https://docs.nvidia.com/cuda/wsl-user-guide/index.html) but with more details. You can also follow [YouTube guide](https://www.youtube.com/watch?v=mWd9Ww9gpEM) made by Jeff Heaton.

Windows10 *prerelease* build 

## WSL2 Installation

Complete installation guide [by Microsoft](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

To install WSL2 you must be sure that your CPU supports *CPU Virtualization feature* and also this option must be enabled in UEFI.

If you want to install  WSL2 you must have at least ***version 1903 (build 18362)***.

If you want to also install CUDA in your WSL2 you must have at least ***version 2004 with kernel 4.19.121+ (build 20145, prerelease)***.

WSL2

As long as Windows prerelease builds are the part of Windows Insider Program you need to register in this program.
In Windows:
**Start** > **Settings** > **Privacy** > **Diagnostics & feedback** > :ballot_box_with_check: ✔**Optional**

[//]: # ":ballot_box_with_check:"

Enable Dev Channel to get latest update from Inside Program.
**Start** > **Settings** > **Update & security** > **Windows 10 Insider Preview** > ✔**Dev Channel**

Windows will switch build and download new updates. It requires multiple reboots.

Hyper-V in Windows Features must be disabled if you want to use WSL version 2.
```
dism.exe /Online /Disable-Feature:Microsoft-Hyper-V /All
```

Microsoft Windows Subsystem for Linux and Virtual Machine Platform must be enabled.
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
After that you will need to reboot your system.

You can check your Kernel with following command:
```
wsl cat /proc/version
```

Set your WSL to version 2.
```
wsl --set-default-version 2
```
If there are warnings related to virtualization, then you have not enabled CPU virtualization/disabled Hyper-V.

Next step is to install Ubuntu from Microsoft Store (20.04 is more preferable). Download and install it (choose user login/password).

You can then check subsystems.
```
wsl --list
```

## Setting up CUDA Toolkit

[Download](https://developer.nvidia.com/cuda/wsl/download) and install NVIDIA CUDA on WSL driver on Windows.

In Ubuntu:
```bash
sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/7fa2af80.pub
sudo sh -c 'echo "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64 /" > /etc/apt/sources.list.d/cuda.list'
sudo apt-get update
```

Install cuda-toolkit last version.
```bash
apt-get install -y cuda-toolkit-11-0
```

New version of CUDA must appear
```bash
nvidia-smi.exe
```

Make CUDA example of Black-Scholes executing on GPU kernel 
```bash
cd /usr/local/cuda/samples/4_Finance/BlackScholes
make -j12
./BlackScholes
```
Test must be passed if everything was installed properly.

## Setting up Miniconda3 on Ubuntu

Download and install Miniconda3.
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```
Agree with license terms and add it using conda init if you want.

## Setting up monitor forwarding

[Install VcXsrv](https://sourceforge.net/projects/vcxsrv/) on host machine (Windows).

Thereafter follow the [guide](https://dev.to/rescenic/comment/obea).

```bash
sudo apt update && sudo apt -y upgrade
sudo apt-get purge xrdp
sudo apt install xrdp
sudo apt install -y xfce4
sudo apt install -y xfce4-goodies

sudo cp /etc/xrdp/xrdp.ini /etc/xrdp/xrdp.ini.bak
sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini
sudo sed -i 's/max_bpp=32/#max_bpp=32\nmax_bpp=128/g' /etc/xrdp/xrdp.ini
sudo sed -i 's/xserverbpp=24/#xserverbpp=24\nxserverbpp=128/g' /etc/xrdp/xrdp.ini
echo xfce4-session > ~/.xsession

sudo nano /etc/xrdp/startwm.sh
-comment these lines to:
#test -x /etc/X11/Xsession && exec /etc/X11/Xsession
#exec /bin/sh /etc/X11/Xsession
	
-add these lines:
# xfce
startxfce4

sudo /etc/init.d/xrdp start

export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0
```

# TensorFlow installation guide

TensorFlow installation guide

## Beginning

At first decide on what device you will install TensorFlow.

The typical way is to install it with usage of the [cpu device].
The more modern way is to install it with usage of the [gpu device].

For gpu check if your version is [compatible with CUDA](https://developer.nvidia.com/cuda-gpus)

Also check [CUDA versions compatibility](https://docs.nvidia.com/deploy/cuda-compatibility/index.html)

## Anaconda installation

In this guide Miniconda3 (basic version of Anaconda3 without packages) will be used.
It is better in case of easy installation and smaller size.

You can install Anaconda later using links on guides below in sections *`cpu`*, *`gpu`*.

Installation:
Delete all previous Miniconda3 (Anaconda3) versions

Install [Miniconda3](https://docs.conda.io/en/latest/miniconda.html)

Install for me

Do not add to path for [cpu, gpu w/ CUDA]

Add to path for [gpu w/out CUDA]

Set as default python enviroment [moreover if you don't use pip]

## [cpu]

You can follow [detailed instruction for tensorflow-cpu installation](https://github.com/jeffheaton/t81_558_deep_learning/blob/master/t81_558_class01_intro_python.ipynb)
by [Jeff Heaton](https://sites.wustl.edu/jeffheaton/) (Ph.D., Data scientist and adjunct instructor).
Also check included [videoinstruction](https://www.youtube.com/watch?v=59duINoc8GM)

Launch Anaconda promt (it will be installed in OS).

In (base) create new enviroment _tensorflowcpu_ with python3.6 version:
```bash
conda create --name tensorflowcpu python=3.6
```

Activate enviroment and check python version:
```bash
activate tensorflowcpu 
python --vsersion 
```

Install jupyter for better editing:
```bash
conda install jupyter
jupyter notebook
```

Install all necessary packages with specific versions:
```bash
conda install scipy
pip install --upgrade sklearn
pip install --upgrade pandas
pip install --upgrade pandas-datareader
pip install --upgrade matplotlib
pip install --upgrade pillow
pip install --upgrade requests
pip install --upgrade h5py
pip install --upgrade pyyaml
pip install --upgrade psutil
pip install --upgrade tensorflow==1.12.0
pip install --upgrade keras==2.2.4
```

## [gpu without CUDA]

Compatibility with CUDA:
https://developer.nvidia.com/cuda-gpus
https://docs.nvidia.com/deploy/cuda-compatibility/index.html

Download [NVIDIA driver](https://www.nvidia.com/Download/index.aspx) (preferably Game Ready Driver).

Then you can follow [full guide of how to install TF without CUDA on Win10](https://www.pugetsystems.com/labs/hpc/How-to-Install-TensorFlow-with-GPU-Support-on-Windows-10-Without-Installing-CUDA-UPDATED-1419/) written by [Dr Donald Kinghorn](https://www.pugetsystems.com/bios.php?name=donkinghorn).

Short version of how to install:

At first install Anaconda (if you still didn't complete it) that was described in section ## Anaconda installation.

Then in Anaconda promt:

```bash
conda create --name tf-gpu
conda activate tf-gpu
conda install tensorflow-gpu
```

Install jupyter if you didn't and ipykernel (including with tf-gpu-version).
```bash
conda install ipykernel jupyter
python -m ipykernel install --user --name tf-gpu --display-name "TensorFlow-GPU-1.13"
jupyter notebook
```

Try to check the [gpu devices](https://stackoverflow.com/questions/38559755/how-to-get-current-available-gpus-in-tensorflow) using:

```python
from tensorflow.python.client import device_lib
local_device_protos = device_lib.list_local_devices()
print([x.name for x in local_device_protos if x.device_type == 'GPU'])
```

You must see something like:
> name: Gece GTX XXX major: X minor: X memoryClockRate(GHz): ...
>
> ['/device:GPU:0']

## [gpu with CUDA]

https://www.youtube.com/watch?v=KZFn0dvPZUQ

# Caffe installation guide

Caffe installation guide

## Beginning

For gpu check if your version is [compatible with CUDA](https://developer.nvidia.com/cuda-gpus).

Install last version of [gpu driver](https://www.nvidia.com/Download/Find.aspx).

Caffe requires CUDA packages before main installation. So it is better to [download CUDA](https://developer.nvidia.com/cuda-downloads)
and [cuDNN](https://developer.nvidia.com/cudnn). Also check the compatibility of:
1. NVIDIA GPU drivers
1. CUDA
1. cuDNN

Unpack cuDNN and copy files:
/cuda/
1. bin/
1. include/
1. lib/
1. txt file (not necessary)
in the directory of the CUDA (e.g C:/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v10.1/

## Installation in conda

Check open [issue on github](https://github.com/BVLC/caffe/issues/6569).

Be sure that CUDA (and its directories) are visible for your system (e.g check PATH variable on Win10).

In conda promt:
```bash
conda create --name caffe-gpu
conda activate caffe-gpu
conda config --add channels anaconda
conda install caffe -c willyd
```

In Python console try:
```bash
import caffe
```

## By hands

Follow [guide](https://www.youtube.com/watch?v=SeCE757egcE)
