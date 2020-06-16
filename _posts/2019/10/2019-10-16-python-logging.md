---
title: "Python Logging"
date: "2019-10-16"
---

Log is an important tool for any developer. it helps in debugging and log important information or exceptions that emits while the code executes

Python provides a logging system as a part of its standard library, so you can quickly add logging to your application

log is a cleaner alternative to writing print statement in the code. You can set up a format and define levels when you have to print the log messages

lt is important to understand at what point in your code you want to log the messages.

If there is a decision point in the code and you want to capture any exception at that point then you can add a DEBUG message there

Another example would be if you just wanted to know what is the value of a variable at the entry and exit of a function then add a log Info there.

There are some messages on which you want to act immediately and trigger an email or slack message when such error occurs, Ensure that you are not setting those log messages as info or Debug instead an Error or a Critical message.

So long story short, you should have done a clear analysis of your code and then add the log messages with different levels

In this post, we will start with basic logging and then take a deep dive on how to set up logging for your project. Here are the topics which we will cover in this post

## **Basic logging**

Python has a logging module which you can import with the simple command

```
import logging
```

Now in your code to implement logging initialize logging.basicConfig() class and then anywhere in the code use logging.info(msg) to print the message. the basic configuration creates StreamHandler with a default Formatter and adds it to the root logger

```
logging.basicConfig(level=logging.DEBUG)
```

Here is an example where we are logging the division by zero error, Multiplication by zero and addition by zero information

```
import logging

class opr_on_num:

    def __init__(self,a,b):
        self.a = a
        self.b = b

    def add_two_no(self):
        if self.a==0 or self.b==0:
            logging.info("You are adding zero to a number")
        return self.a+self.b


    def divide_two_no(self):
        try:
            divide=self.a/self.b
        except Exception as e:
            logging.info(e)
            divide = 0

        return divide

    def multiply_two_no(self):
        if self.a==0 or self.b==0:
            logging.debug("You are multiplying zero to a number, Result will
              be a Zero")
        multi = self.a * self.b
        return multi

if __name__ == '__main__':
    logging.basicConfig(level=logging.DEBUG)
    obj=opr_on_num(5,0)
    obj.divide_two_no()
    obj.add_two_no()
    obj.multiply_two_no()
```

When you run the above code this is the output that you see

> _INFO:root:division by zero
> INFO:root:You are adding zero to a number
> DEBUG:root:You are multiplying zero to a number, Result will be a Zero_

### **Log to a File**

In the above code just add filename parameter to basicconfig() settings. This will create a File Handler instead of a default Stream handler and all logs message will be written to my\_logs.log file

```
logging.basicConfig(level=logging.DEBUG,filename='./my_logs.log')
```

### **Log Levels**

Let's look at different log levels.

You have to set a log level in your code so that all the log levels above the setup level should be shown

For example if log level is set to Warning then all the info and debug message in the code will not be executed and only levels having value greater than equal to info will be executed. In this case, INFO, Warning, Error and Critical

if you check the above code the log level is set to logging.DEBUG i.e. DEBUG level, so any level equal or above this will be executed by the system

Here is the table you can refer for different log levels and their values

![](/images/2019/10/image-16.png)

### **Log Format**

This will initialize the format class with a new format string for the log message, if no format is specified then the default %(message)s i.e. the plain message will be shown

Here we are using the format string '%(asctime)s : %(levelname)s : %(message)s' which has following attributes that will be printed in the log message

```
format='%(asctime)s : %(levelname)s : %(message)s'
```

**%(asctime)s** : asctime to show the timestamp when the information is logged,
**%(levelname)s**: Level name is the log level whether it is Info, Debug, Error etc.
**%(message)s**: Finally message will print the Exception message.

In the above code update the logging.basicConfig() code to:

```
logging.basicConfig(level=logging.DEBUG,format='%(asctime)s : %(levelname)s : %(message)s')
```

**Output:**

> _2019-10-15 17:01:09,739 : ERROR : division by zero
> 2019-10-15 17:01:09,745 : INFO : You are adding zero to a number
> 2019-10-15 17:01:09,746 : DEBUG : You are multiplying zero to a number, Result will be a Zero_

## **Logging configuration**

basicconfig() uses root logger and its not a good idea to use the root logger when you are working with big projects and different modules where you want to implement the logs. Its better to configure your log at one place and call the logger in every modules with a different name so that each module will log it separately and it would be easier for a developer to understand the source of the log and this way you don't have to configure the log for each module separately

### **Configuration and Adding Stream Handler**

logger should always be instantiated using module level function logging.getLogger(name). As per the official documentation

> _The_ `_name_` _is potentially a period-separated hierarchical value, like_ `_foo.bar.baz_` _(though it could also be just plain_ `_foo_`_, for example). Loggers that are further down in the hierarchical list are children of loggers higher up in the list. For example, given a logger with a name of_ `_foo_`_, loggers with names of_ `_foo.bar_`_,_ `_foo.bar.baz_`_, and_ `_foo.bam_` _are all descendants of_ `_foo_`_. The logger name hierarchy is analogous to the Python package hierarchy, and identical to it if you organise your loggers on a per-module basis using the recommended construction_ `_logging.getLogger(__name__)_`_. That’s because in a module,_ `___name___` _is the module’s name in the Python package namespace._

In the below example we have use \_\_name\_\_ as the module name

```
logger = logging.getLogger(__name__)
```

You can use Handler to setup where the log messages should be printed either a File using FileHandler or to a Console using StreamHandler. There are other types of Handlers which you can check in this [link](https://docs.python.org/3/library/logging.handlers.html)

```
ch = logging.StreamHandler()
logger.addHandler(ch)
```

Finally a custom format string is defined and added to the handler

```
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
 ch.setFormatter(formatter)
```

if no other handler is provided in the code then basicConfig() will be called

**Here is the Full code:**

```
import logging

class opr_on_num:

    def __init__(self,a,b):
        self.a = a
        self.b = b

    def add_two_no(self):
        if self.a==0 or self.b==0:
            logger.info("You are adding zero to a number")
        return self.a+self.b


    def divide_two_no(self):
        try:
            divide=self.a/self.b
        except Exception as e:
            logger.error(e)
            divide = 0

        return divide

    def multiply_two_no(self):
        if self.a==0 or self.b==0:
            logger.debug("You are multiplying zero to a number, Result will be a Zero")
        multi = self.a * self.b
        return multi

if __name__ == '__main__':
    logger = logging.getLogger(__name__)
    # create console handler with a higher log level
    ch = logging.StreamHandler()

    # Create Formatter and setup in logging
    formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
    ch.setFormatter(formatter)

    # Set the log level and add handler
    logger.setLevel(logging.DEBUG)
    logger.addHandler(ch)

    obj=opr_on_num(5,0)
    obj.divide_two_no()
    obj.add_two_no()
    obj.multiply_two_no()
```

### **File Handler**

Instead of writing the code to console you can directly write all the log from all the modules to a file using FindHandler.

Only thing you have to do is change the StreamHandler to a FileHandler with the path where you want to save the file.

```
ch = logging.FileHandler('my_file_1.log')
logger.addHandler(ch)
```

The above code will write the log to a file my\_file\_1.log in the parent directory

**Output in a log file:**

![](https://i0.wp.com/kanoki.org/wp-content/uploads/2019/10/image-17.png?fit=640%2C78&ssl=1)

## **Adding** **Extra Fields to a Log Message**

We will add a **`Filename`** and `**lineno**` attribute to the log message just to understand which module and line number is throwing the error or emitting that message

> _2019-10-15 18:15:07,745 -**test.py:19** -main – INFO – division by zero_

For the complete list of attributes that can be used for log messages check this [link](https://docs.python.org/3/library/logging.html#logrecord-attributes)

Just update this format string in the above code:

```
formatter = logging.Formatter('{"time":%(asctime)s,"file_name": "%(filename)s:%(lineno)d" ,' \
        '"level": "%(levelname)s" ,"msg":"%(message)s" }')
```

**Output:**

> _2019-10-15 18:15:07,745 -test.py:19 -main - INFO - division by zero
> 2019-10-15 18:15:07,746 -test.py:11 -main - INFO - You are adding zero to a number
> 2019-10-15 18:15:07,747 -test.py:26 -main - DEBUG - You are multiplying zero to a number, Result will be a Zero_

## **Log Format as JSON Object**

So far we have seen the format is a String and the output is also a string of log attributes

> _2019-10-15 18:15:07,745 -test.py:19 -_**_main_** _- INFO - division by zero_

if you want a JSON object with Key Value pair for each of these attributes then just update the Format String to this:

```
formatter = logging.Formatter('{"time":%(asctime)s,"file_name": "%(filename)s:%(lineno)d" ,' '"level": "%(levelname)s" ,"msg":"%(message)s" }')
```

**Output:**

> _{"time":2019-10-15 18:20:50,773,"file\_name": "test.py:19" ,"level": "INFO" ,"msg":"division by zero" }
> {"time":2019-10-15 18:20:50,773,"file\_name": "test.py:11" ,"level": "INFO" ,"msg":"You are adding zero to a number" }
> {"time":2019-10-15 18:20:50,773,"file\_name": "test.py:26" ,"level": "DEBUG" ,"msg":"You are multiplying zero to a number, Result will be a Zero" }_

## **Custom fields in Log Format**

The filter class is used to filter the log records So that a logger will output the desired log message.

You can also add keyword arguments(kwargs) to a log message using the Filter method and print that in the log messages directly

> _{"time":2019-10-16 06:48:23,585,"file\_name": "test.py:27" ,"level": "INFO" ,**"msg":"Division","User":"min2bro"** }_

Here is an example of Filter which add additional blocks in the message for **`User`** and **`Custom_msg`**

```
class AppFilter(logging.Filter):
    def filter(self, record):
        record.User = record.args.get("User")
        record.custommsg = record.args.get("custom_msg")
        return True
```

**Add the filter to logger instance:**

```
logger.addFilter(AppFilter())
```

**Add kwargs in the log messages:**

```
logger.info("You are adding zero to a number,{"User":'min2bro',"custom_msg":'addition'})
```

**Full code:**

```
import logging
# Add Filter
class AppFilter(logging.Filter):
    def filter(self, record):
        record.User = record.args.get("User")
        record.custommsg = record.args.get("custom_msg")
        return True



class opr_on_num:

    def __init__(self,a,b):
        self.a = a
        self.b = b

    def add_two_no(self):
        if self.a==0 or self.b==0:
            logger.info("You are adding zero to a number",{"User":'min2bro',"custom_msg":'addition'})
        return self.a+self.b


    def divide_two_no(self):
        try:
            divide=self.a/self.b
        except Exception as e:
            logger.info(e,{"User":'min2bro',"custom_msg":'Division'})
            divide = 0

        return divide

    def multiply_two_no(self):
        if self.a==0 or self.b==0:
            logger.debug("You are multiplying zero to a number, Result will be a Zero",{"User":'min2bro',"custom_msg":'Multiplication'})
        multi = self.a * self.b
        return multi

if __name__ == '__main__':
    logger = logging.getLogger(__name__)
    # create console handler with a higher log level
    ch = logging.StreamHandler()
    logger.addFilter(AppFilter())
    formatter = logging.Formatter('{"time":%(asctime)s,"file_name": "%(filename)s:%(lineno)d" ,' \
        '"level": "%(levelname)s" ,"msg":"%(custommsg)s","User":"%(User)s" }')
    ch.setFormatter(formatter)

    logger.setLevel(logging.DEBUG)
    logger.addHandler(ch)

    obj=opr_on_num(5,0)

    obj=opr_on_num(5,0)
    obj.divide_two_no()
    obj.add_two_no()
    obj.multiply_two_no()
```

**Output:**

> _{"time":2019-10-16 06:48:23,585,"file\_name": "test.py:27" ,"level": "INFO" ,"msg":"Division","User":"min2bro" }
> {"time":2019-10-16 06:48:23,589,"file\_name": "test.py:19" ,"level": "INFO" ,"msg":"addition","User":"min2bro" }
> {"time":2019-10-16 06:48:23,590,"file\_name": "test.py:34" ,"level": "DEBUG" ,"msg":"Multiplication","User":"min2bro" }_

## **Logging Variable Data**

This can help to format the log message with a variable data. You can either use a style parameter with the formatter and set it to either '{' or '$'

```
formatter = logging.Formatter('{asctime} {name} {levelname:8s} {message}',
                       style='{')
```

Or use the f-string which is introduced in Python 3.6, and it works great for the string formatting. Here `**num**` and `**example**` are the variables which are directly embedded in the log message without setting any style for the formatter string

```
logger.error(f'This is message {num} for {example}')
```

**Full Code:**

```
import logging


class opr_on_num:

    def __init__(self,a,b):
        self.a = a
        self.b = b

    def add_two_no(self):
        return self.a+self.b


    def divide_two_no(self):
        try:
            divide=self.a/self.b
        except Exception as e:
            logger.error(f'This is message {num} for {example}')
            divide = 0

        return divide

    def multiply_two_no(self):
        multi = self.a * self.b
        return multi

if __name__ == '__main__':
    num=10
    example = 'logging_example'
    logger = logging.getLogger(__name__)
    # create console handler with a higher log level
    ch = logging.StreamHandler()
    formatter = logging.Formatter('{asctime} {name} {levelname:8s} {message}',
                       style='{')
    ch.setFormatter(formatter)

    logger.setLevel(logging.INFO)
    logger.addHandler(ch)

    obj=opr_on_num(5,0)

    obj=opr_on_num(5,0)
    obj.divide_two_no()
    obj.add_two_no()
    obj.multiply_two_no()
```

**Output:**

> _2019-10-15 19:26:22,464_ **_main_** _ERROR This is message 10 for logging\_example_

## **Loading the log configuration from a JSON file**

Here is an example of logging configuration dictionary which can be stored in a flat json file

```
log_config = {
    "version": 1,
    "disable_existing_loggers": False,
    "formatters": {
        "simple": {
            "format": "%(asctime)s - %(name)s - %(levelname)s - %(message)s"
        }
    },

    "handlers": {
        "file_handler": {
            "class": "logging.FileHandler",
            "level": "DEBUG",
            "formatter": "simple",
            "filename": "my_file.log",
            "encoding": "utf8"
        }
    },

    "root": {
        "level": "DEBUG",
        "handlers": ["file_handler"]
    }
}
```

And this dictionary is passed to the dictconfig() to setup the log configuration

```
with open('C:/Personal/Kanoki/logging/log_config.json', 'rt') as f:
            config = json.load(f)
logging.config.dictConfig(config)
```

**Full code:**

```
import logging,json
import logging.config

class opr_on_num:

    def __init__(self,a,b):
        self.a = a
        self.b = b

    def add_two_no(self):
        return self.a+self.b


    def divide_two_no(self):
        try:
            divide=self.a/self.b
        except Exception as e:
            logger.error(f'This is message {num} for {example}')
            divide = 0

        return divide

    def multiply_two_no(self):
        multi = self.a * self.b
        return multi

if __name__ == '__main__':
    num=10
    example = 'logging_example'
    with open('C:/Personal/Kanoki/logging/log_config.json', 'rt') as f:
            config = json.load(f)
    logging.config.dictConfig(config)

    logger = logging.getLogger(__name__)
    # create console handler with a higher log level
    # ch = logging.StreamHandler()
    # formatter = logging.Formatter('{asctime} {name} {levelname:8s} {message}',
    #                    style='{')
    # ch.setFormatter(formatter)

    # logger.setLevel(logging.INFO)
    # logger.addHandler(ch)

    obj=opr_on_num(5,0)

    obj=opr_on_num(5,0)
    obj.divide_two_no()
    obj.add_two_no()
    obj.multiply_two_no()
```

## **Rotating File Handlers**

Rotating file handler is used to rotate the log files. if a log files exceeds a certain defined size then a new log file is created.

> `_logging.handlers.RotatingFileHandler_`_(filename, mode='a', maxBytes=0, backupCount=0,_
>
> _encoding=None, delay=False)_

`**maxByte**` size is set to write the logs to a new file when existing log file size goes beyond that limit

**`backupCount`** when set to non-zero the system will append the existing files with extension 1, 2 etc.

Here is the code you have to replace in the above code to create a rotating file handler

```
handler = RotatingFileHandler('my_log.log', maxBytes=2000, backupCount=10)
logger.addHandler(handler)
```

Read the official Python Logging documentation [here](https://docs.python.org/3/library/logging.handlers.html#rotatingfilehandler) to understand how the log files are renamed and what are the other parameters used for

## **Conclusion**

Logging is an important feature for any project and implementing logs in an efficient way really helps to debug the code and catch errors and exceptions without much hassle. This post gives a very detailed overview of the logging module in Python and demonstrates various features with the help of examples. For further reading you can refer to the logging official documentation [here](https://docs.python.org/2/library/logging.html). Hope you enjoyed this blog and if you have any comments or suggestions please drop a note in the comments section below
