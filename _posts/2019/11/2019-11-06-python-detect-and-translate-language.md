---
title: "Python Detect and Translate language"
date: "2019-11-06"
categories: [ Data Science, Python, Python, Data Science ]
tags: [ DataScience, Python, Translation ]
---

The internet is flooded with articles and posts for translating the language using Machine Learning or Deep Learning LSTM models and building a deep neural network for developing your own Translation model. However, if you are not interested in coding then we have google as one of the prominent leader in providing the translation service from any known language in world to another. But yes that comes with a cost if you are using it for commercial purpose or huge data to translate

In this post we are going to see how `[Textblob](https://textblob.readthedocs.io/en/dev/#)` - A python library for text processing and mainly used for natural language processing tasks such as part-of-speech tagging, noun phrase extraction, sentiment analysis, classification, translation, and more that can be used to detect and translate a language. Later on, We will also explore the python `google translate` package for translating the text

## **Install Textblob**

```
pip install -U textblob
```

## **Detect Language**

### **Hindi Text Detection**

The language we are detecting here is Hindi, which is the 4th largest spoken language in the world. The output is the abbreviation for Hindi i.e. hi

```
from textblob import TextBlob
hi_blob = TextBlob(u'तुम्हारा नाम क्या है')
hi_blob.detect_language()
```

**Output**: 'hi'

### **German Text Detection**

```
de_blob = TextBlob(u"Maschinelles Lernen ist ein interessantes Thema zum Lernen")
de_blob.detect_language()
```

**Output**: 'de'

## **Translate Language**

### **Translate Hindi text to English**

This is the same hindi text we detected above

```
hi_blob.translate(to='en')
```

**Output**: TextBlob("what is your name")

### **Translate German to English**

```
de_blob.translate(to='en')
```

**Output**: TextBlob("Machine learning is an interesting topic for learning")

### **Translate German to French**

```
de_blob.translate(to='fr')
```

**Output**: TextBlob("L'apprentissage automatique est un sujet d'apprentissage intéressant")

### **Translate German to Arabic**

```
de_blob.translate(to='ar')
```

**Output**: TextBlob("التعلم الآلي هو موضوع مثير للاهتمام للتعلم")

### **Translate German to Hindi**

```
de_blob.translate(to='hi')
```

**Output**: TextBlob("मशीन लर्निंग, सीखने का एक दिलचस्प विषय है")

**Note:** The TextBlob output can be treated as a Python String

## **Google Translate**

It's a free and unlimited to use python library that implements Google Translate API's. It has two powerful methods detect and translate which uses [Google Translate AJAX API](https://translate.google.com/)

### **Installation**

```
pip install googletrans
```

### **Translate Hindi to English**

```
from googletrans import Translator
translator = Translator()
print(translator.translate(u'तुम्हारा नाम क्या है'))
```

**Output:** Translated(src=hi, dest=en, text=what is your name, pronunciation=None, extra_data="{'translat...")

### **Translate Arabic to English**

The `dest` parameter is used to specify the language to translate, which is English(en) here

```
print(translator.translate(u'التعلم الآلي هو موضوع مثير للاهتمام للتعلم', dest='en'))
```

**Output:** Translated(src=ar, dest=en, text=Automated learning is interesting to learn the subject, pronunciation=None, extra_data="{'translat...")

Even you can just specify the `src` parameter - Text language and it will translate that to English by default

```
print(translator.translate(u'التعلم الآلي هو موضوع مثير للاهتمام للتعلم', src='ar'))
```

**Output:** Translated(src=ar, dest=en, text=Automated learning is interesting to learn the subject, pronunciation=None, extra_data="{'translat...")

## **Detect Language**

```
translator = Translator()
print(translator.detect("Les livres sont les meilleurs amis de l'homme"))
```

**Output**: Detected(lang=fr, confidence=1.0)

Language detected is French and the confidence score is 1 i.e. 100%

## **Translate List of Sentences**

### **English to Hindi**

```
translations = translator.translate(['what is your Name', 'how old are you', 'Boston weather is good'], dest='hi')

for translation in translations:
    print(translation.origin, ' -> ', translation.text)
```

**Output:**

what is your Name -> तुम्हारा नाम क्या हे
how old are you -> आपकी उम्र क्या है
Boston weather is good -> बोस्टन मौसम अच्छा है
