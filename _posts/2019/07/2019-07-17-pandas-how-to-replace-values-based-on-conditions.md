 ---
title: "Pandas How to replace values based on Conditions"
date: "2019-07-17"
categories: [ Data Science, Pandas, Python, Python, Data Science ]
tags: [ DataScience, Pandas, Python ]
---

Using these methods either you can replace a single cell or all the values of a row and column in a dataframe based on conditions .

**Dataframe:**

```
import pandas as pd
import numpy as np

df = pd.DataFrame({'Date' : ['11/8/2011', '11/9/2011', '11/10/2011',
                                        '11/11/2011', '11/12/2011'],
                'Event' : ['Dance', 'Painting', 'Dance', 'Dance', 'Painting']})

df
```

![](/images/2019/07/image-16.png)

## **Using loc for Replace**

Replace all the Dance in Column Event with Hip-Hop

```
df.loc[(df.Event == 'Dance'),'Event']='Hip-Hop'
df
```

![](/images/2019/07/image-17.png)

## **Using numpy where**

Replace all Paintings in Column Event with Art

```
df['Event'] = np.where((df.Event == 'Painting'),'Art',df.Event)
df
```

![](/images/2019/07/image-18.png)

## **Using Mask for Replace**

Replace all the Hip-Hop in Column Event with Jazz

```
df['Event'].mask(df['Event'] == 'Hip-Hop', 'Jazz', inplace=True)
```

![](/images/2019/07/image-19.png)

## **Using df where**

Replace all Art in Column Event with Theater

```
m = df.Event == 'Art'
df.where(~m,other='Theater')
```

![](/images/2019/07/image-20.png)

## **Create a new Dataframe**

```
df = pd.DataFrame([[1.4, 8], [1.2, 5], [0.3, 10]],
     index=['China', 'India', 'USA'],
     columns=['Population(B)', 'Economy'])
```

![](/images/2019/07/image-24.png)

## **Set value for an entire row**

```
df.loc['India'] = 10
df
```

![](/images/2019/07/image-21.png)

## **Set value for an entire column**

```
df.loc[:, 'Economy'] = 30
df
```

![](/images/2019/07/image-22.png)

## **Set value for rows matching condition**

```
df.loc[df['Population(B)'] < 1] = 0
df
```

![](/images/2019/07/image-23.png)
