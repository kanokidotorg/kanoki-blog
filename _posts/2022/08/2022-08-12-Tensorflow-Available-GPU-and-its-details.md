---
title: "Tensorflow available GPU and it's details"
date: "2022-08-12"
categories: [ google-colab, tensorflow, python]
tags: [  google-colab, tensorflow, python]

---

In this post we will see how to find all the available CPU and GPU devices on the host machine and get the device details and other info like it's Memory usage and compute capability.

Following Tensorflow functions can be used to find the available GPU devices on host machine and get its details :

- list_logical_devices(): Return a list of logical devices created by runtime.
- list_physical_devices(): Return a list of physical devices visible to the host runtime.
- get_visible_devices(): Get the list of visible physical devices.

We will use a google-colaboratory notebook to run the code in this post.

## Available GPUs and CPUs 

*tf.config.list_physical_devices()* function returns the list of available devices in host runtime. The Physical devices are hardware devices present on the host machine

We can use that to find both the CPU and GPU devices on your host.

```python
physical_devices = tf.config.list_physical_devices('CPU')
physical_devices
```
**Out:**

[PhysicalDevice(name='/physical_device:CPU:0', device_type='CPU')]

It shows there are just one CPU available in the google-colab notebook.

Let's find the available GPU in the notebook, For this you need to first change the google-colab runtime to CPU. Navigate to Runtime > Change runtime type and set the Hardware accelerator to GPU.

![](/images/2022/08/tf-gpu-2.png)

![](/images/2022/08/tf-gpu-1.png)

After changing the Runtime hardware accelerator to GPU the google-colab notebook is running on a host with physical GPU.

Let's check out how many GPU's are available now



```python
physical_devices = tf.config.list_physical_devices('GPU')
print("Num GPUs Available: ", len(physical_devices))
```

**Out:**

[PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]

Num GPUs Available:  1

## Get GPU details

We can also  get the available GPU details like the type of device and it's compute capability using *tf.config.experimental.get_device_details()* function, It takes in a [`tf.config.PhysicalDevice`](https://www.tensorflow.org/api_docs/python/tf/config/PhysicalDevice) returned by [`tf.config.list_physical_devices`](https://www.tensorflow.org/api_docs/python/tf/config/list_physical_devices) and returns a dict with string keys containing various details about the device

Let's check out the device details of GPU that comes with Google-colaboratory notebook

```python
tf.config.experimental.get_device_details(physical_devices[0])
```

**Out:**

{'compute_capability': (7, 5), 'device_name': 'Tesla T4'}

The device_name: Tesla T4 is the name of the device and compute_capability of the device as a tuple of two ints, in the form `(major_version, minor_version)`. It's only available for NVIDIA GPUs

## Get GPU Memory Info

We can also get the memory info of the device(GPU) using the *tf.config.experimental.get_memory_info()* function, It returns a dict containing information about the device's memory usage. This function currently only GPU's and TPU's

Let's check out the memory info for the GPU device available with google-colab notebook

```python
if physical_devices:
  print(tf.config.experimental.get_memory_info('GPU:0'))
```

**Out:**

{'current': 0, 'peak': 0}

The output dict specifies only the current and peak memory used by the device across the run of the program, in bytes that TensorFlow is actually using, not the memory that TensorFlow has allocated on the GPU

For GPUs, TensorFlow will allocate all the memory by default, unless changed with [`tf.config.experimental.set_memory_growth`](https://www.tensorflow.org/api_docs/python/tf/config/experimental/set_memory_growth).

Also, you can set the peak memory for a device to the device's current memory usage, This allows you to measure the peak memory usage for a specific part of your program

```python
tf.config.experimental.reset_memory_stats('GPU:0')
```

