# TensorFlow installation guide

TensorFlow installation guide

## Beginning

At first decide on what device you will install TensorFlow.

The typical way is to install it with usage of the [cpu device].
The more modern way is to install it with usage of the [gpu device].

For gpu check if your version is compatible with CUDA:
https://developer.nvidia.com/cuda-gpus

Also check CUDA versions compatibility:
https://docs.nvidia.com/deploy/cuda-compatibility/index.html

## Anaconda installation

In this guide Miniconda3 (basic version of Anaconda3 without packages) will be used.
It is better in case of easy installation and smaller size.

You can install Anaconda later using links on guides below in sections [cpu], [gpu].

Installation:
Delete all previous Miniconda3 (Anaconda3) versions

Install Miniconda3 from https://docs.conda.io/en/latest/miniconda.html
Install for me

Do not add to path for [cpu, gpu w/ CUDA]
Add to path for [gpu w/out CUDA]

Set as default python enviroment [moreover if you don't use pip]

[cpu]

https://github.com/jeffheaton/t81_558_deep_learning/blob/master/t81_558_class01_intro_python.ipynb

by Jeff Heaton

Also videoinstruction included:
https://www.youtube.com/watch?v=59duINoc8GM

Launch Anaconda promt:
conda create --name tensorflowcpu python=3.6 # in (base)
activate tensorflowcpu # now in (tensorflowcpu)
python --vsersion # check version (3.6)

conda install jupyter
jupyter notebook

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

[gpu without CUDA]

Compatibility with CUDA:
https://developer.nvidia.com/cuda-gpus
https://docs.nvidia.com/deploy/cuda-compatibility/index.html

Download NVIDIA driver from (preferably Game Ready Driver):
https://www.nvidia.com/Download/index.aspx


[gpu with CUDA]

https://www.youtube.com/watch?v=KZFn0dvPZUQ


