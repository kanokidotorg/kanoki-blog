---
title: "Merge Images with Python"
date: "2017-07-12"
categories: [ Python ]
tags: [ Python ]
---

**Introduction**

Merging Images was never an easy task and you always hit the expensive softwares or you have to upload your personal images on someone's website to do it online. It was my property document with 128 pages tif files and I was not ready to upload these images to someone's website and get it merged vertically, Also I was working on a mac and doesn't know any freeware which can help me to do this. Then after some research I stumbled upon using python to achieve this task and somewhere in the corner of my heart I was optimistic that this can be achieved using my favorite Python. So I'm going to use PIL, it's a Python Imaging Library which helps in image processing.

So here are the 3 images of my terrace garden which I want to merge both horizontally and vertically using Python, Isn't my garden images are beautiful?

 ![](/images/2017/07/terracegarden1.jpg) ![](/images/2017/07/terracegarden2.jpg) ![](/images/2017/07/terracegarden3.jpg)

Since I'm using Python scientific libraries frequently, I thought of using power of numpy to achieve this, So there are two numpy methods vstack & hstack which stacks the arrays in a sequence vertically & horizontally respectively. Now you must be wandering, what is a stack in numpy, it's helps to join sequence of array along a new axis.

Before we jump into image merging, let's discuss out how vstack and hstack works in numpy

**Stack Vertically**

a = np.array(\[1, 2, 3\])
b = np.array(\[4, 5, 6\])

Will vertically stack both these arrays vertically using vstack

np.vstack((a,b))

and here is the output:

array(\[\[1, 2, 3\],
\[4, 5, 6\]\])

Check the shape of this stack

np.vstack((a,b)).shape()
(2,3)

So exactly it has two rows and three columns.

**Stack Horizontally**

Now let's stack both these array horizontally using hstack

np.hstack((a,b))

and here is the output:

array(\[1, 2, 3, 4, 5, 6\])

Check the shape of the stack

np.hstack((a,b)).shape
(6,)

it has only 6 elements stacked horizontally.

**Image Merging**

We have 3 images(my terrace garden images) and would like to merge it horizontally first and then vertically, So before we look into the code let's understand the steps we are going to follow for merging these images.

Step 1: Load all the Images using Image module, which represent a PIL image. There are other functions to load images from files or to create a new image

Step 2: Pick the smallest of all the images

Step 3: Re-size the other images to match the smallest image from Step 2

Step 4: Use Numpy vstack and hstack to align the images Vertically & horizontally

Step 5: Save the processed image

In the below code for merging the images the horizontally merged images are saved with the name terracegarden\_h.jpg and vertically merged images are saved as terracegarden\_v.jpg

**\# Import Numpy & Python Imaging Library(PIL)**

import numpy as np
from PIL import Image

**\# Open Images and load it using Image Module**

images\_list = \['terracegarden1.jpg', 'terracegarden2.jpg', 'terracegarden3.jpg'\]
imgs = \[ Image.open(i) for i in images\_list \]

**\# Find the smallest image, and resize the other images to match it**

min\_img\_shape = sorted( \[(np.sum(i.size), i.size ) for i in imgs\])\[0\]\[1\]
img\_merge = np.hstack( (np.asarray( i.resize(min\_img\_shape,Image.ANTIALIAS) ) for i in imgs ) )

**\# save the horizontally merged images**

img\_merge = Image.fromarray( img\_merge)
img\_merge.save( 'terracegarden\_h.jpg' )

**\# Merge the images using vstack and save the Vertially merged images**

img\_merge = np.vstack( (np.asarray( i.resize(min\_img\_shape,Image.ANTIALIAS) ) for i in imgs ) )
img\_merge = Image.fromarray( img\_merge)
img\_merge.save( 'terracegarden\_v.jpg' )

And here are the beautiful Images after Merge

**Vertically Merged:**

![](/images/2017/07/terracegarden_v.jpg)

**Horizontally Merged:**

![](/images/2017/07/terracegarden_h.jpg)
