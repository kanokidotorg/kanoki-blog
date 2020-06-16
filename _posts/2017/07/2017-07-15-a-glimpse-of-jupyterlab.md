---
title: "A Glimpse of Jupyterlab"
date: "2017-07-15"
categories: [ Python ]
tags: [ Python ]
---

**Introduction**

So finally there is a tool which perceive as one stop shop for all the work you do as a DataScientist. Jupterlab, it's still in development phase as mentioned in it's github page. However you can still install and start using and see how amazing this tool is, it has blended File browser, Terminal, Console , Code, editor everything at one place. There are plenty of nice features, Multiple notebooks without lot of tabs. Everything is on your browser(http://localhost:8000). You can plot maps directly from the geojson files in the notebook editor. You can open csv files in the form of table. Edit the Markdown files. Open Terminal, Console and Editor side by side and view everything simultaneously

This is the best what a data scientist or analyst wish to use, it's an IDE in real terms. Being a Data Scientist you have to work on multiple tools at the same time, R with Rstudio, Python with Jupyter notebook and learning every new tool and playing around to get a hands-on and that consumes lot of your productive work. So with Jupyter lab, you can have everything at one Browser platform.

![](/images/2017/07/Screen-Shot-2017-07-15-at-04.59.36.png)

**Installation:**

[Here](https://github.com/jupyterlab/jupyterlab) is the github link where you can get all the instructions to download using conda or pip

if you use conda, you can type this command in your terminal:

conda install -c conda-forge jupyterlab

if you use pip, you can type this command:

pip install jupyterlab
jupyter serverextension enable --py jupyterlab --sys-prefix

Starting Jupyter lab:

jupyter lab

This opens up the browser http://localhost:8000

**Launcher:**

Select the Notbeook , Console of your choice, so you can see in the image I can choose between python and Julia notebooks here

![](/images/2017/07/Screen-Shot-2017-07-15-at-05.04.14-e1500078105370.png)

**File Browser:**

On the left Pane, it Shows the files & folders within the folder from where jupyter lab is opened.

**Commands:**

it has all the list of commands and keyboard shortcuts

![](/images/2017/07/Screen-Shot-2017-07-15-at-05.03.32.png)

**Cell tools:**

you can select the cell types for your notebook if you want to show it as a slide or sub slide or skip. This would be useful while generating the slides for your notebook.

![](/images/2017/07/Screen-Shot-2017-07-15-at-05.44.44-e1500078237736.png)

**CSV:**

CSV files can be directly opened as table and get a quick glance of it.

![](/images/2017/07/Screen-Shot-2017-07-15-at-05.02.30-1.png)

**Multiple windows:**

You can have multiple windows and everything including editor, terminal, console aligned alongside.

![](/images/2017/07/Screen-Shot-2017-07-15-at-05.18.46.png)

There are lot of other amazing features which is explained in this Pydata 2016 video:

https://www.youtube.com/watch?v=CoGCuliGNos
