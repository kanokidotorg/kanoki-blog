---
title: "Use Python Decorators to find Page load of your Web Application"
date: "2017-06-29"
tags: [ Python ]
---

![dec](https://techpickup.files.wordpress.com/2017/06/dec.png)

Last week I had a challenging task of finding the Page load time for my application and somehow I was not convinced to use the Navigation Timing API's for this, So coming from a background of Selenium Automation tester, I thought why not use the Selenium Webdriver for this, which can help to internally navigate between the pages in my application. But the challenge still remains, how to calculate the page load time. After an intense think through, I decided to use Decorators to overcome this challenge.

Look for the last event in your page and use Selenium webdriver to put a wait instance until this element is visible on the page. For example, I would like to know the page load time of the login page and the elements are loaded with a sequence of HTML events, and the last element to load is "Submit" button, so my func to post the URL and wait for the login page to load will look something like this:

![Screen Shot 2017-06-29 at 22.50.26](https://techpickup.files.wordpress.com/2017/06/screen-shot-2017-06-29-at-22-50-26.png)

The entire action for which you want to record the time is wrapped in a function. Now the next challenge was to capture the time for which this functions runs and finds the "Submit" button on the page. Let's use the decorator to calculate the function run time and eventually calculate the page load time.![Screen Shot 2017-06-29 at 23.16.14](https://techpickup.files.wordpress.com/2017/06/screen-shot-2017-06-29-at-23-16-14.png)

Here is how you use this decorator to find the Page load time by capturing the function run time

![Screen Shot 2017-06-29 at 22.59.56](https://techpickup.files.wordpress.com/2017/06/screen-shot-2017-06-29-at-22-59-56.png)
