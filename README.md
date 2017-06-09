# This repository is now deprecated
The Tensorflow install process is now stable enough that the use of the script is no longer required.

To install on your own machine (CPU support), follow these instructions (check OS specific instructions):
https://www.tensorflow.org/get_started/os_setup

To install on the CLSP machines follow these instructions: 
http://www.statmt.org/jhu/?n=NeuralMT.HomePage


# Wheel install on the CLSP machines
``pip install --upgrade pip --user``

``pip install --upgrade numpy --user``

For the CPU version

``pip install --user /export/a16/gkumar/code/tensorflow/pip_packages/tf_cpu_pkg/tensorflow-0.9.0-py2-none-any.whl``

For the GPU version

``pip install --user /export/a16/gkumar/code/tensorflow/pip_packages/tf_gpu_pkg/tensorflow-0.9.0-py2-none-any.whl``

The GPU version requires the GPUs to be set in DEFAULT or EXCLUSIVE_PROCESS mode (all GPUs on the CLSP cluster, now operate on the EXCLUSIVE_PROCESS mode).

# Install Tensorflow from source

Installs Tensorflow from source (with Bazel) with a non-system GCC. This is useful
when the system GCC is outdated and cannot be updated easily.

## Requirements
1. >= GCC 4.8 (/home/gkumar/.local)
2. Python 2.7
3. >= Numpy 0.9
4. CUDA 7.0
5. CuDNN2 (/home/gkumar/.local/cudnn)

## Status
This script works with
* Tensorflow commit : 19bda20635c91060996fe44fe201875b3b9ae7a2
* Bazel commit : 0e17046797f6cfed21755f2eed7c52c0a340d40c

## Installation
For the CLSP cluster, more variables may need to be changed for other clusters

1. Change ``PY_USER_SITE`` in tf_install.sh to point to your python user-site
2. Make CUDA visible to ``LD_LIBRARY_PATH`` : ``export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64``
3. Run tf_install.sh
4. Add bazel to ``$PATH`` : ``export PATH=$PATH:`pwd`/bazel/output``
5. Try the examples on https://www.tensorflow.org/versions/master/get_started/os_setup.html to make
    sure that everything is set up correctly.
6. (Optional) Builds with both CPU and GPU support by default. This can be disabled in the script.
    Check the last few lines in the file.

## Instructions for the CLSP cluster
It is very important that you follow these steps if you plan on using a GPU. If you don't, you may kill other scheduled jobs and get very angry emails.

1. Reserve a GPU : ``qlogin -l gpu=1``. This will open up a session with access to 1 GPU.
2. **IMPORTANT** : The NVIDIA drivers are set up in exclusive mode. This will cause your training script to grab all GPUs on the machine and will kill other jobs. Use ``CUDA_VISIBLE_DEVICES=1`` with your script. See next item for USAGE.
2. Train you model. Eg. ``CUDA_VISIBLE_DEVICES=1 python tensorflow/models/image/mnist/convolutional.py``
3. ssh to the machine on which the session was scheduled. Run ``nvidia-smi`` to verify that your job is running.
4. Delete the qlogin session when you are done.

#### *Try your first TensorFlow program (GPU)*
```python
$ CUDA_VISIBLE_DEVICES=3 python #3 is the id of the free GPU. Use : /home/gkumar/scripts/free-gpu

>>> import tensorflow as tf
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> sess.run(hello)
Hello, TensorFlow!
>>> a = tf.constant(10)
>>> b = tf.constant(32)
>>> sess.run(a+b)
42
>>>
```

Optimized for the CLSP@JHU cluster.
