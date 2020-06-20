---
title: "How to share files in same network"
date: "2017-06-25"
---

if you have a file with huge size to share to your colleagues, friends then you have only two options either zip, attach and send thru email or share it on any file hosting services like Google Drive, dropbox etc., it is good to use these options when you have to share the files over the internet. But if you want to share the file locally within your network then why so much of hassle of zip, attachments and sharing on file hosting services, you Just need to have python installed on your system and you can start a simple HTTP servers (Web servers) to share the file.

Follow the steps shown here:

**Step 1:** Ensure python is installed on your machine (latest version is Python 3)

**Step 2:** Open Command Prompt(in Windows) or Terminal(in mac or Linux) Go to the Folder where the file is located for example C:\Users\ron\datafiles\

**Step 3:** Type following command on the Terminal: python -m http.server <port number>

![Screen Shot 2017-06-25 at 23.36.49](https://techpickup.files.wordpress.com/2017/06/screen-shot-2017-06-25-at-23-36-49.png)

**Step 4:** Open browser and enter the url: **http://localhost:8090**

**![Screen Shot 2017-06-25 at 23.38.07](https://techpickup.files.wordpress.com/2017/06/screen-shot-2017-06-25-at-23-38-07.png)****Step 5:** Now change local host with your machine ip and share this link with anyone in your network to share the files.
