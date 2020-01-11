---
id: model_train
title: Model Training
sidebar_label: Model Training
---


## Set Model Parameter  

* set the Min Max Scaler with feature range
* set validate percent
* set time step (Sequence length)
* set after_day (output size) 
* set batch size
* set epochs number
* set empty array for outputs
  

```python

    scaler = MinMaxScaler(feature_range=(0, 1))

    validate_percent = 0.8
    time_step = 20
    after_day = 5
    batch_size = 64
    epochs = 100
    output = []

```

# Scaling data


```python
        # normalize data
        data = normalize_data(data, scaler, feature_len)

        # test data
        x_test = data[-time_step:]
        x_test = np.reshape(x_test, (1, x_test.shape[0], x_test.shape[1]))

```


# Scaling data


```python
        # normalize data
        data = normalize_data(data, scaler, feature_len)

        # test data
        x_test = data[-time_step:]
        x_test = np.reshape(x_test, (1, x_test.shape[0], x_test.shape[1]))

```