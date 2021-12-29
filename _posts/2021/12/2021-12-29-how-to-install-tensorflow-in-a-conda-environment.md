---

title: "How to install tensorflow in a conda environment"
date: "2021-12-29"
categories: [ Python, tensorflow, conda]
tags: [ Python, tensorflow, conda]

---

In this post, we will learn how to install tensorflow 2 in a conda environment. I would be installing tensorflow in two steps, first we will create a conda python environment and then install the tensorflow 2 using pip  

Ensure that the anaconda is installed properly in your mac/windows laptop and path is setup and conda commands can be accessed from your windows cmd or mac terminals

### Step 1: Create python conda environemt
A conda environment with a specific python version 3.7 can be created using the following command

```python
conda create -n tf-venv python=3.7
```
Here tf-env is my tensorflow environment name, this is a user-defined name

Activate this environment by typing below command in your terminal

```python
conda activate tf-venv
```

### Install tensorflow 2 in conda environment
In tensorflow 2, CPU and GPU are packaged as one single bundle so you don't have to install it separately. 

we can use the following pip command to install the latest tensorflow version in the conda environment created above

```python
# Requires the latest pip
pip install --upgrade pip

# Current stable release for CPU and GPU
pip install tensorflow
```

if you want to install the current tf-nightly build then use the below command

```python
pip install tf-nightly
```

Once tensorflow is installed then check the version of the tf package, As of writing this article the latest tensorflow version was 2.7.0

```python
>>> import tensorflow as tf
>>> tf.__version__
'2.7.0'
```





