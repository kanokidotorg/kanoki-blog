---
title: "How to Tame a Python"
date: "2019-01-30"
categories: [ Data Science, Python ]
tags: [ DataScience, Pandas, Python ]
---

All of those out there who claim that you can learn python in 3,6 or 9 days or 1 month are just fooling around and f\*\* up with your mind. Get it straight you can get a gist of python in 3 hours but you cannot be a data scientist with your this knowledge on Python and this I'm telling you from my decade of experience as a Python Developer and a Data Scientist now.

I practice yoga everyday and I learnt Yoga from a renowned teacher in India. Does it make me a Yoga expert? Still I'm struggling and sure can never reach to this stage where Yoga Guruji [BKS Iyengar](https://en.wikipedia.org/wiki/B._K._S._Iyengar) had been performing unless I quit everything in life and become a full time Monk.

![](/images/2019/01/image-31.png)

I never thought that one day I would be writing a blog to de-motivate the audience or the python beginners or aspiring data scientist. But the kind of buzz going around on the internet world compelled me to open my notepad and let everyone know that Sorry! you can't learn python in 1 month. The time and effort which a python developer has spent to understand the intricacies of the language cannot be so simple that it can be learnt in few weeks and months. It's a beautiful language and provides simplicity and flexibility to welcome everyone to learn it but nowhere it guarantees to be a data scientist or a developer in few weeks or months. The code you write after learning python for 1 months is good to run on your own laptop or notebook but not on a server. You cannot process millions of data what a data scientist is doing with that code with memory leaks and non cpu intensive blocks.

Software programming is an art and you cannot specialize an art in 1 week or a month. it takes lot of effort and time to understand and implement and learn this art. Read this excerpt from the famous book by Donald Knuth "The Art of Computer Programming"

> We have seen that computer programming is an art, because it applies accumulated knowledge to the world, because it requires skill and ingenuity, and especially because it produces objects of beauty. A programmer who subconsciously views himself as an artist will enjoy what he does and will do it better. Therefore we can be glad that people who lecture at computer conferences speak about the state of the Art.

It requires some science and mathematics fundamentals to program and become a craftsmen and do a constructive activity to produce some useful solution for a problem. if you are good at Maths then there
are high chances you can be a good programmer as well because that makes you understand the data structure and algorithm faster than anyone else and have more analytical approach to any problem. Like many of us who tries to solve everything in this world with mathematics. There is an old saying that everything has some sense of maths in it and even if you look at history Ancient Greeks have used Pythagores Theorem.

However views of lot of people reading this post will conflict who thinks a programmer learns a set of practical techniques and then uses that to solve a business problem or any other problem. I don't want to argue on this point though. Everyone is free to think in their own way.

Having said so far, I have no means to scare you and instead I want to give the right path to learn Python and join this band wagon. Don't fall trap to 2 weeks and 1 month course. Here is how
you need to kick start your python journey.

**Basic Python**

Learn this from one of the best course available on internet and that too from Google.

[https://developers.google.com/edu/python/](https://developers.google.com/edu/python/)

It covers all the basics that you need to head start and then you can go with your own pace and understand the examples given in this tutorial. Take your time and don't rush to finish off everything in a Week. It's
a fantastic course and doing it slowly build your fundamentals strong for the language.

Practice - Project Euler

Practice is key to success and same implies here, Leverage your learning to solve some real time problems and what else can be best to start than Project Euler. It's not a cake walk to solve [project euler problems](https://projecteuler.net/problem=22) and it really takes lot of effort to understand and then code a solution. So to have a good start you can initially start with old Archive problems which are pretty decent and as you go ahead the problem complexity level increase. You can try top 10-15 problems and that will help you to implement some
of your learning from Step 1

An Example how to implement dictionary

![](/images/2019/01/image-29.png)

```
import string
from typing import List, Dict

def get_text_from_file(file_path: str) -> List:
    '''
    Parses a text file containing names in this format:
    "FOO","BAR","BAZ"
    and returns a list of strings, e.g.,
    ["FOO","BAR","BAZ"]
    '''
    with open(file_path) as file_:
        return file_.read().replace('"', "").split(",")


def get_sorted_indexed_dict(items: List, is_0_indexed: bool = False) -> Dict:
    '''
    Takes a list of strings and returns an alphabetically sorted dict
    with the item's 1-indexed (by default) position as key, e.g.,
    `list` arg: [ "FOO", "BAR", "BAZ" ]
    returns: { 1 : "BAR", 2 : "BAZ", 3 : "FOO" }
    The return dict can be 0-indexed by adding a 2nd argument as True.
    '''
    ix_start = 1
    if is_0_indexed:
        ix_start = 0
    items = sorted(items)
    numbered_dict = {}
    for item in items:
        numbered_dict[items.index(item) + ix_start] = item
    return numbered_dict


def get_ascii_values() -> Dict:
    '''
    Assigns ASCII chars A-Z, inclusive, to a
    number value 1-26, respectively.
    '''
    alpha_values = {}
    for index, letter in enumerate(string.ascii_uppercase, 1):
        alpha_values[letter] = index
    return alpha_values


def get_sum_word_alpha(word: str, alpha_values: Dict) -> int:
    #use of the get() method of the dicts
    return sum([alpha_values.get(letter, 0) for letter in word])


def get_rank_value(rank: int, word: str, alpha_values: Dict) -> int:
    '''
    Calculates the ranked value according to problem 22, i.e.,
    the value of each word's letters multiplied by its
    alphabetical rank.
    '''
    return rank * get_sum_word_alpha(word, alpha_values)


def get_final_answer(file_path: str) -> int:
    '''
    Returns the solution to Project Euler 22 based on the file path provided.
    '''
    alpha_values = get_ascii_values()
    # sanity check some alpha values
    assert alpha_values['A'] == 1
    assert alpha_values['Z'] == 26
    assert alpha_values['S'] == 19

    names = get_text_from_file(file_path)
    names = get_sorted_indexed_dict(names)
    # sanity checks name order based on PE 22 problem description
    assert names[1] == "AARON"
    assert names[938] == "COLIN"
    assert names[5163] == "ZULMA"

    name_values = [
        get_rank_value(k, v, alpha_values)
        for (k, v) in names.items()
    ]
    return sum(name_values)


if __name__ == "__main__":
    FILE = "C:/Personal/Kanoki/Python is not a Joke/names.txt"
    print("The answer to PE 22 is: {}".format(get_pe22_solution(FILE)))
```

source: [https://codereview.stackexchange.com/questions/115489/project-euler-22](https://codereview.stackexchange.com/questions/115489/project-euler-22)

**Intermediate Python Topics:**

- Classes and Functions
- Using the Python Shell
- itertools
- Collections
- Generators and Decorators
- Connect with Databases
- Parsing Excel/CSV/TXT Files
- File Management
- List & Dictionary comprehension
- Basic Memory Management

**Pandas**

An excellent tool for playing with data and ofcourse if you are aspiring data scientist then you need to learn this tool without any iota of doubt. There is an awesome book written by the author himself
which will be a very good point to start.

![](/images/2019/01/image-30.png)

Download any dataset from kaggle and just practice what you learnt in the book.

There are visualization libraries Matplotlib, Bokeh, Seaborne and Plotly. To start with Matplotlib is good and easy to learn and later on based on your use case you can choose a library which your team prefers or you have good a command.

I have a very Brief Tutorial "[Pandas in a Nutshell](https://kanoki.org/2017/07/16/pandas-in-a-nutshell/)"

**Numpy and it's Arrays**

Once you are done with pandas then it's good to start with [numpy](http://www.numpy.org/) and learn some of it's feature on how to write code for faster data processing which is one of the challenge every data scientist faces in his journey and it's good to tame it.

**SQL**

Not related to Python however if you are an aspiring data scientist then
This is the most critical skills that every datascientist know and should master it. Believe me you would be using SQL everywhere whether it's pulling the data from HIVE/MYSQL or for any kind of data pre-processing or cleanup. if you know SQL then **half the battle is won**. Have a good grip on [joins](https://en.wikipedia.org/wiki/Join_(SQL)) and [regular expressions](https://en.wikipedia.org/wiki/Regular_expression).

**SQL skills tops the list.**

**Advance Topics**

This I would suggest not to start immediately and when need be then start learning it else you would be mid-way confused.

- Meta-programming
- Descriptors
- Decorators (class and method based)
- Buffering Protocol
- Comprehensions
- GIL and multiprocessing and multithreading
- WSGI protocol
- Context Managers
- Design Patterns

You can master python and become an expert but there is sufficient time that everyone needs to spend to learn a new skill. Be patient and learn everyday that is the key for a Pythonistic Future. Good Luck!!
