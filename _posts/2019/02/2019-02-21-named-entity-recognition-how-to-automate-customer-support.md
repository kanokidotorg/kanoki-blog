---
title: "Named Entity Recognition: How to Automate Customer Support"
date: "2019-02-21"
categories: [ Data Science, Pandas, Python ]
tags: [ DataScience, Machine Learning, Python ]
---

Customer support is one of the complex and most important part of any business. This area of business stands to benefit from the machine learning as it is helping to automate and improve the entire customer service process and reduce the overall turn around time to resolve a ticket and eventually ensures a quicker response time. In this article I am going to give a real time Customer Service issue in a Company and how they have leveraged a Machine Learning approach to expedite the customer service resolution

**Major Areas which can be Automated?**

**Quicker Technical Solution based on the same kind of issues encountered in the past**

All the data from past tickets and resolution is collected and then we did some feature engineering by collecting all the imp features like product, problem, type of problem software or hardware, who resolved it, what was the fix, important keywords in ticket etc. Using this data we created a recommendation engine when a new ticket pops up then we can suggest the best available solution for that based on the past experience. For example: ticket with installation problem in windows 10 can have multiple fixes from the past which will suggest to the user who is searching for such fix. A set of questionnaire is prepared and as user answers those questions it narrows down the option available and suggests a right answer for the problem. If user selects windows 10 installation, what type of error? It's visual studio version. What's the Visual studio version? 3.3 , u need 3.5 and above for this installation

![](/images/2019/02/recommendation_engine.png)

**Determine Customer Intent and Recommending Solution**

The company were most of the time unaware that what kind of help their customer need and what's the intention they are contacting using social media or direct channels.Some of the customer just leave a random remarks on social media about your product and will not revert after that. So to tackle this problem we have consolidated all the remarks, feedbacks, comments from all the channels and segreggated them into these three categories Need Attention/No Attention Required/Analyze for resolution. These categories contains additional attributes like customer name, city,zipcode, product, dates if any, frequency, history and many other features which were specific to customer product line. Using this information we developed an ensemble model which predicts what would be the intention of the customer when he/she is contacting you. For example: A customer with product xxx is contacting after 6 months is most probably calling for the warranty extension or a customer with product xyz is calling after 2 months of purchase is want to enquire about the cartridge change.

![](/images/2019/02/ensemble_model.png)

**Routing Ticket to Right Agent/Representative**

Routing the ticket to right person is the most important and time consuming part in the entire customer support operations and this has cost around 20% of total time for a ticket resolution. The challenge lies ahead was to parse all those tickets and understand the problem of each ticket and route it to the right team technical - product - hardware/software - server and there were 125 different such categories of support teams handling these tickets. We used Named Entity Recognition approach to solve this problem and have used spacy module for training the model and eventually tagging and routing to right team. Let's first understand this

![](/images/2019/02/Routing.png)

**What's Named Entity Recognition?**

As per the Wikipedia, Named-entity recognition (NER) (also known as entity identification, entity chunking and entity extraction) is a subtask of information extraction that seeks to locate and classify named entity mentions in unstructured text into pre-defined categories such as the person names, organizations, locations, medical codes, time expressions, quantities, monetary values, percentages, etc.

it refers to a data extraction task that is responsible for finding, storing and sorting textual content into default categories such as the names of persons, organizations, locations, expressions of times, quantities, monetary values and percentages. Duties of NER includes extraction of data directly from plain English text sentences.Named-entity recognition is a state-of-the-art intelligence system that works with nearly the efficiency of a human brain. The system is structured in such a way that it is capable of finding entity elements from raw data and can determine the category in which the element belongs. The system reads the sentence and highlights the important entity elements in the text. NER might be given separate sensitive entities depending on the project. This means that the NER system designed for one project may not be reused for another task. Similarly, NER faces many challenges which include the extraction of correct information for specific but closely related categories.

**SPACY Named Entity Recognition**

[SpaCy’s named entity recognition](https://spacy.io/api/annotation#section-named-entities) Models has been trained on the [OntoNotes 5](https://catalog.ldc.upenn.edu/ldc2013t19) corpus and it supports the following entity types.
There is another Model trained on Wikipedia corpus which uses a less fine-grained NER annotation scheme

![](/images/2019/02/image-19.png)

We have first trained the Entity Recognizer using our custom data. We had tons of annotations that we defined as token tags. We wrote the custom python program to convert this train data into required format. Here is the sample training data

```
train_data = [('Problem with the mobile app BLE-SCANNER ?', [(28, 11, 'Product')]),
              ('Bought the app for $550 and warranty of 6 months', [(20, 4, 'Currency'), (27, 24, 'Warranty Period')])]
```

**Customer Ticket Parsing:**

Here I'm going to showcase a demo ticket which is parsed to extract the required tags and then it's feeded to the routing engine to direct it to the right representative to handle it

```
Customer_Ticket=nlp(u''' I had purchased BLE Scanning Mobile App with 1032 BLE Tags on 21.10.11 from M/s Pai International, Somerville, New Jersey for $9000. It has 2 years warranty and one year
extended warranty. Within a span of 04 months while spinning, the device has started behaving awkwardly . Today morning when I was scanning my BLE tags using this mobile
apps then it has just scanned 29 out of 55 tags in the room, There was a light on the top of the tag which is constantly blinking since then. ''')
```

**NER Parsed Result**

BLE Scanning Mobile App **Product**
$9000 **Currency**
2 Years **Warranty**
Mobile App **Product**
BLE Tag **Product**
Blinking **Issue**

![](/images/2019/02/image-20.png)

This information extracted using the model we trained has lot of information which let out routing engine to decide which Team it has to route. In the example, The ticket is for Mobile App and the Product is BLE Scanning App and the issue is pertaining to Blinking which is more of hardware team.

**Conclusion**

This overall exercise of preparing the Strategy and doing a small one time POC took around 6 months to complete. The result was pretty interesting and it has turned out to be a boon for the company since they were able to cut down their overall ticket resolution, Customer Query turn around time by 30%. Named Entity Recognition is a powerful algorithm which can trained on your data and then can be used to extract the desired information in any new document. This is extensively being used to recommend the news articles by extracting the Person and place in one article and look for other articles matching those tags with some counter applied.
