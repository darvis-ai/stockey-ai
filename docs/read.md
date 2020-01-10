---
id: read
title: Data Provider Reading
sidebar_label: Data Reading
---


## Storage Data 

we use mongodb storage for stocks data its called <a href="https://github.com/man-group/arctic">Arctic</a>,High performance datastore for time series and tick data and you can check its docs from <a href="https://arctic.readthedocs.io/en/latest/">Here</a> 





## import Libraries 

```python
ALPHA_KEY = '3CRSNQLTCX8TX53G'
from arctic import Arctic


```



## inialize Arctic Storage Database 

```python
store = Arctic('localhost')
lib = store['HISTORICAL_DATA']


```


## Select stock and get data and store it inside database 
* you can change symbol and outputsize and interval

```python

item = lib.read('AAPL')
data = item.data
metadata=item.metadata


```


## Check data 

``` python 

data.head(3)


            1. open	2. high	3. low	  4. close	5. volume    		
date			
2019-12-27	291.12	293.97	290.6600  293.54	2839816.0
2019-12-26	284.82	289.98	284.7000  289.91	22410950.0
2019-12-24	284.69	284.89	282.9197  284.27	12119714.0



```

This is a link to [Read Stock Data](write.md)  
This is a link to an [Alpha Vanitage .](https://www.alphavantage.co/)

