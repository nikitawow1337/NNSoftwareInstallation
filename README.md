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
