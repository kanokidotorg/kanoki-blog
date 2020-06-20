---
title: "Draw Pencil Sketches and Play with Photos"
date: "2017-08-15"
categories: [ Python ]
tags: [ Python ]
---

During my childhood, I was always fascinated to have my Pencil sketch and impress my school mates. However at that time it wasn’t an easy job for school goers like me to get this done, I gathered all my courage to check with my drawing teacher if really I can make pencil sketch of someone and She blatantly replied first complete your school drawing homework. I was depressed and over the period of time my desire for the pencil sketch get buried somewhere in the history. Now while working with Python day in-out I thought why not look for anything similar in Python and fulfill my long awaiting desire and to my surprise Python provides flexible and highly amazing library(PIL, Python Image Library) which can do anything with your image. I know many of you will argue there are so many mobile apps and image processing libraries which can do the same thing in a minute or so. But beside these arguments, I wanted to achieve this entire exercise with Python and with the ease of doing it on my laptop.

**1. Creating a Pencil Sketch**

**Original Image:**

![](/images/2017/08/vinay.png)

```
from PIL import Image
from PIL import ImageFilter
im=Image.open("./Desktop/Desktop/vinay.png")
imout=im.filter(ImageFilter.CONTOUR)
imout.show()`
```
**Pencil Sketch using ImageFilter Class of PIL:**

![](/images/2017/08/pencilsktch.png)

**2. Convert your image to Black and White (B&W)**

**Original Image:**

![](/images/2017/08/Varanasi.jpg)

```
from PIL import Image
image_file = Image.open("./Desktop/Varanasi.jpg") # Path of your image
image_file = image_file.convert('L') # convert image to black and white
image_file.show()
```

**B&W Image:**

![](/images/2017/08/BnW.png)

**3. Write on Your Image**

```
from PIL import Image, ImageFont, ImageDraw, ImageEnhance
font = ImageFont.truetype("/Library/Fonts/AmericanTypewriter.ttc",45)
source_img = Image.open('./Desktop/Varanasi.jpg').convert("RGBA")# Path of your image
draw = ImageDraw.Draw(source_img)
draw.text((1000, 20), "Ganges-Varanasi",font=font,fill=(0, 0, 0, 0))# Text to write on the image
source_img.show()
```

![](/images/2017/08/Writeonimg.png)

**4. Enhance the Quality - Contrast, Brightness, Colors**

```
from PIL import ImageEnhance
image = Image.open('./Desktop/Varanasi.jpg')
enhancer = ImageEnhance.Brightness(image) # Enhancing Brightness of the Image
factor = 1.3
enhancer.enhance(factor).show("Sharpness %f" % factor)
```

**Enhanced Image:**

![](/images/2017/08/Enhanced.png)
