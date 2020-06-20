---
title: "Selenium script to login gmail and logout successfully"
date: "2017-05-18"
tags: [ Selenium, Python ]
---

https://youtu.be/awJ16Ec-3ak

**Java Code for Gmail Login:**

```
\[code language="Java"\]

package mypackage;

import org.openqa.selenium.By;

import org.openqa.selenium.WebDriver; import org.openqa.selenium.WebElement; import org.openqa.selenium.support.ui.ExpectedConditions; import org.openqa.selenium.chrome.ChromeDriver; import org.openqa.selenium.support.ui.WebDriverWait; import java.util.List; import org.openqa.selenium.firefox.\*; import java.util.concurrent.\*;

public class myclass {

public static void main(String\[\] args) {

//initialize Chrome driver System.setProperty("webdriver.chrome.driver", "C:\\\\selenium-java-2.35.0\\\\chromedriver\_win32\_2.2\\\\chromedriver.exe"); WebDriver driver = new ChromeDriver();

//Open gmail driver.get("http://www.gmail.com");

// Enter userd id WebElement element = driver.findElement(By.id("Email")); element.sendKeys("xyz@gmail.com");

//wait 5 secs for userid to be entered driver.manage().timeouts().implicitlyWait(5, TimeUnit.SECONDS);

//Enter Password WebElement element1 = driver.findElement(By.id("Passwd")); element1.sendKeys("Password");

//Submit button element.submit();

WebElement myDynamicElement = (new WebDriverWait(driver, 15)).until(ExpectedConditions.presenceOfElementLocated(By.id("gbg4"))); driver.findElement(By.id("gbg4")).click();

//press signout button driver.findElement(By.id("gb\_71")).click();

}

}
```
```
\[/code\]

**Python Code for Facebook Login:**

\[code language="Java"\]

from selenium import webdriver from selenium.webdriver.common.keys import Keys

//Username user = "\*\*\*\*\*\*"

//Password pwd = "\*\*\*\*\*"

//Initialize firefox driver driver = webdriver.Firefox()

//Open Facebook Page driver.get("http://www.facebook.com")

//Enter Username or Email elem = driver.find\_element\_by\_id("email") elem.send\_keys(user)

//Enter Password elem = driver.find\_element\_by\_id("pass") elem.send\_keys(pwd)

//Press submit button for login elem.send\_keys(Keys.RETURN)

//Close Driver driver.close()

\[/code\]
```
