---
id: utils
title: Model Helper 
sidebar_label: Model Helper 
---


## importing 

```python

import os
import csv
import errno
import numpy as np
from keras.utils import plot_model
from sklearn.preprocessing import MinMaxScaler
import matplotlib.pyplot as plt

```


## Load Sequence Data
 This function using for preparing sequences data and its consider the first step at data processing phase .


```python

def load_data(data, time_step=5, after_day=1, validate_percent=0.67):
    seq_length = time_step + after_day
    result = []
    for index in range(len(data) - seq_length + 1):
        result.append(data[index: index + seq_length])
    result = np.array(result)
    print('total data: ', result.shape)

    train_size = int(len(result) * validate_percent)
    train = result[:train_size, :]
    validate = result[train_size:, :]

    x_train = train[:, :time_step]
    y_train = train[:, time_step:]
    x_validate = validate[:, :time_step]
    y_validate = validate[:, time_step:]

    return [x_train, y_train, x_validate, y_validate]
```



## Normalize Data 

 This function using for preparing sequences data via Scaling and inverse scaling it its consider the second and last step at data processing phase .

* Normalize Data 
```python
def normalize_data(data, scaler, feature_len):
    minmaxscaler = scaler.fit(data)
    normalize_data = minmaxscaler.transform(data)
```

* Inverse Normalize data 
```python
def inverse_normalize_data(data, scaler):
    for i in range(len(data)):
        data[i] = scaler.inverse_transform(data[i])

    return data

```

## Save Model 

```python
def save_model(model, model_name):
    file_path = 'model/{}.h5'.format(model_name)
    if not os.path.exists(os.path.dirname(file_path)):
        try:
            os.makedirs(os.path.dirname(file_path))
        except OSError as exc: # Guard against race condition
            if exc.errno != errno.EEXIST:
                raise

    model.save(file_path)

```

## Plot model architecture
  
  
```python
def plot_model_architecture(model, model_name):
    file_path = 'images/model/{}.png'.format(model_name)
    if not os.path.exists(os.path.dirname(file_path)):
        try:
            os.makedirs(os.path.dirname(file_path))
        except OSError as exc: # Guard against race condition
            if exc.errno != errno.EEXIST:
                raise

    plot_model(model, to_file=file_path, show_shapes=True)
```





## Plot Predict 


```python

def plot_predict(data, data_predict, file_name):
    file_path = 'images/result/{}.png'.format(file_name)
    if not os.path.exists(os.path.dirname(file_path)):
        try:
            os.makedirs(os.path.dirname(file_path))
        except OSError as exc: # Guard against race condition
            if exc.errno != errno.EEXIST:
                raise

    fig = plt.figure(figsize=(15, 10))
    ax1 = fig.add_subplot(231)
    ax2 = fig.add_subplot(232)
    ax3 = fig.add_subplot(233)
    ax4 = fig.add_subplot(234)
    ax5 = fig.add_subplot(235)

    ax1.plot(data[:, 0, 3], color='black')
    ax1.plot(data_predict[:, 0, 3], color='red')
    ax1.title.set_text("Day 1")

    ax2.plot(data[:, 1, 3], color='black')
    ax2.plot(data_predict[:, 1, 3], color='red')
    ax2.title.set_text("Day 2")

    ax3.plot(data[:, 2, 3], color='black')
    ax3.plot(data_predict[:, 2, 3], color='red')
    ax3.title.set_text("Day 3")

    ax4.plot(data[:, 3, 3], color='black')
    ax4.plot(data_predict[:, 3, 3], color='red')
    ax4.title.set_text("Day 4")

    ax5.plot(data[:, 4, 3], color='black')
    ax5.plot(data_predict[:, 4, 3], color='red')
    ax5.title.set_text("Day 5")

    plt.savefig(file_path)
    #plt.show()

```




## Plot Loss

```python
def plot_loss(history, file_name):
    file_path = 'images/loss/{}.png'.format(file_name)
    if not os.path.exists(os.path.dirname(file_path)):
        try:
            os.makedirs(os.path.dirname(file_path))
        except OSError as exc: # Guard against race condition
            if exc.errno != errno.EEXIST:
                raise

    plt.figure()

    plt.plot(history.history['loss'])
    plt.plot(history.history['val_loss'])
    plt.title('model train vs validation loss')
    plt.ylabel('loss')
    plt.xlabel('epoch')
    plt.legend(['train', 'validation'], loc='upper right')
    plt.savefig(file_path)
    #plt.show()

```