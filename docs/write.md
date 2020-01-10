---
id: write
title: Data Provider Write
sidebar_label: Data Writting 
---


## Storage Data 

we use mongodb storage for stocks data its called <a href="https://github.com/man-group/arctic">Arctic</a>,High performance datastore for time series and tick data and you can check its docs from <a href="https://arctic.readthedocs.io/en/latest/">Here</a> , and also we need to use data provider api called <a href="https://www.alphavantage.co/">alpha Vantage  </a> , Alpha Vantage offers free APIs in JSON and CSV formats for realtime and historical stock and forex data, digital/crypto currency data and over 50 technical indicators. Supports intraday, daily, weekly, and monthly quotes and technical analysis with chart-ready time series. 100% free with unlimited API calls.





## import Libraries 

```python

ALPHA_KEY = '3CRSNQLTCX8TX53G'
from arctic import Arctic
from alpha_vantage.timeseries import TimeSeries
from alpha_vantage.techindicators import TechIndicators

```



## inialize libraries and Arctic Storage Database 

```python

store = Arctic('localhost')
store.initialize_library('HISTORICAL_DATA')
store.initialize_library('SMA')

lib = store['HISTORICAL_DATA']
lib_sma = store['SMA']

```


## Select stock and get data and store it inside database 
* you can change symbol and outputsize and interval
* you can select one or more that one Tech Indicators from alpha_vantage Library .

```python

ts = TimeSeries(ALPHA_KEY,output_format='pandas')
ti = TechIndicators(ALPHA_KEY,output_format='pandas')

aapl_data_ts,meta_data_ts = ts.get_daily(symbol='AAPL',outputsize='full')
aapl_data_ti,meta_data_ti = ti.get_sma(symbol='AAPL',interval='daily')

meta_data_aapl={'Source':'Alpha Vantage'}


```


## Write data 

```python

lib.write('AAPL',aapl_data_ts,metadata=meta_data_aapl)
lib_sma.write('AAPL',aapl_data_ti,metadata=meta_data_aapl)

```

## Check data 

``` python 

items = lib.read('AAPL')
aapl = items.data
meta_data = items.metadata

appl


            1. open	2. high	3. low	  4. close	5. volume    		
date			
2019-12-27	291.12	293.97	290.6600  293.54	2839816.0
2019-12-26	284.82	289.98	284.7000  289.91	22410950.0
2019-12-24	284.69	284.89	282.9197  284.27	12119714.0
2019-12-23	280.53	284.25	280.3735  284.00	24677883.0
2019-12-20	282.23	282.65	278.5600  279.44	69032743.0
...         ...     ...    ...        ...        ...    	
1999-12-31	100.94	102.87	99.5000	  102.81	1462600.0
1999-12-30	102.19	104.12	99.6200	  100.31	1849500.0
1999-12-29	96.81	102.19	95.5000	  100.69	2540200.0
1999-12-28	99.12	99.62	95.0000	  98.19	    2210500.0
1999-12-27	104.37	104.44	99.2500	  99.31	    1503500.0


```


This is a link to [Read Stock Data](read.md)  
This is a link to an [Alpha Vanitage .](https://www.alphavantage.co/)

