---
layout: post
title:  "Stacking machine learning models: Light GBM and a neural network"
categories: top_post
---

Using Python to stack a Light GBM model and a simple neural network.

I've been exploring Kaggle recently. XGBoost is a model which has been used for many winning entries on Kaggle. It was taking a while to train the models on my laptop so I tried Light GBM which was faster. Stacking can be used to combine and improve the results from two or more individual models.

```python
# Imports
import numpy as np
import pandas as pd
import sys
import time
import lightgbm as lgb
import json
from random import uniform, randint
from sklearn.preprocessing import Imputer
from sklearn.model_selection import train_test_split
from sklearn.utils import shuffle
from sklearn.metrics import log_loss
from sklearn.model_selection import cross_val_score, cross_val_predict
from sklearn.model_selection import StratifiedKFold
from sklearn.neural_network import MLPClassifier
from sklearn.preprocessing import StandardScaler  
```


```python
# Read in data and separate out target data into y

print "Reading in data...",
train = pd.read_csv('../train.csv')
test = pd.read_csv('../test.csv')
sample = pd.read_csv('../sample_submission.csv')
y = train.target
train.drop(['target'], axis=1, inplace=True)
print("done")
```

    Reading in data... done



```python
# Prepare for stacking: create additional columns in dataframes

train_meta = train.copy()
test_meta = test.copy()
train_meta['M1'] = 0
train_meta['M2'] = 0
test_meta['M1'] = 0
test_meta['M2'] = 0
```


```python
# Stacking
print "Start stacking:",
start_time=time.time()
kf=StratifiedKFold(n_splits=5, shuffle=True, random_state=2017)

gbm_cv_score = []
nn_cv_score = []
mean_cv_score = []

for i, (train_index, val_index) in enumerate(kf.split(train, y)):
    print "Fold",i,", ",
    x_train_kf = train.loc[train_index, :]
    x_val_kf = train.loc[val_index, :]
    y_train_kf, y_val_kf = y[train_index], y[val_index]

    # LightGBM
    lgb_train = lgb.Dataset(x_train_kf, y_train_kf)
    lgb_eval = lgb.Dataset(x_val_kf, y_val_kf, reference=lgb_train)

    params = {
        'task': 'train',
        'metric': {'l2','auc'},    
    }
    gbm = lgb.train(params,
                    lgb_train,
                    valid_sets=lgb_eval,
                    verbose_eval=False
                   )

    gbm_val_pred = gbm.predict(x_val_kf, num_iteration=gbm.best_iteration)
    
    train_meta.iloc[val_index, train_meta.columns.get_loc('M1')] = gbm_val_pred
    
    kf_gbm_log_loss = log_loss(y_val_kf.values, gbm_val_pred, labels=[0,1])
    gbm_cv_score.append(kf_gbm_log_loss)
    
    # Neural net
    clf = MLPClassifier(solver='lbfgs', alpha=1e-5,
                        hidden_layer_sizes=(4, 2), random_state=1)
    scaler = StandardScaler()  
    scaler.fit(x_train_kf)
    x_train_scaled = scaler.transform(x_train_kf)  
    x_val_scaled = scaler.transform(x_val_kf)  

    clf.fit(x_train_scaled, y_train_kf)

    nn_val_pred = clf.predict_proba(x_val_scaled)

    train_meta.iloc[val_index, train_meta.columns.get_loc('M2')] = nn_val_pred[:,1]
    
    nn_log_loss = log_loss(y_val_kf, nn_val_pred)
    nn_cv_score.append(nn_log_loss)
    
    # Mean
    kf_mean_output = np.mean([nn_val_pred[:,1], gbm_val_pred], axis=0)
    mean_log_loss = log_loss(y_val_kf, kf_mean_output)
    mean_cv_score.append(mean_log_loss)

end_time = time.time()
print("Cross validation took %.1f seconds" % (end_time - start_time))
print('Mean cross-validation GBM log_loss score with {} folds is {:.6f}'.format(i+1, np.mean(gbm_cv_score)))
print('Mean cross-validation NN log_loss score with {} folds is {:.6f}'.format(i+1, np.mean(nn_cv_score)))
print('Mean cross-validation mean(GBM,NN) log_loss score with {} folds is {:.6f}'.format(i+1, np.mean(mean_cv_score)))
```

    Start stacking: Fold 0 ,  Fold 1 ,  Fold 2 ,  Fold 3 ,  Fold 4 ,  Cross validation took 228.8 seconds
    Mean cross-validation GBM log_loss score with 5 folds is 0.152113
    Mean cross-validation NN log_loss score with 5 folds is 0.152923
    Mean cross-validation mean(GBM,NN) log_loss score with 5 folds is 0.152160



```python
# Make predictions on the test data 
gbm_test_pred = gbm.predict(test, num_iteration=gbm.best_iteration)
test_meta.iloc[:, test_meta.columns.get_loc('M1')] = gbm_test_pred

test_scaled = scaler.transform(test)  
nn_test_pred = clf.predict_proba(test_scaled)
test_meta.iloc[:, test_meta.columns.get_loc('M2')] = nn_test_pred[:,1]
```


```python
# Fit the stacking model to train_meta, using M1 and M2 as features.

train_meta_2 = train_meta[["M1","M2"]]

x_train, x_val, y_train, y_val = train_test_split(
    train_meta_2, y, test_size=0.2, random_state=2017)

lgb_train = lgb.Dataset(x_train, y_train)
lgb_eval = lgb.Dataset(x_val, y_val, reference=lgb_train)

params = {
    'task': 'train',
    'metric': {'l2','auc'},    
}
gbm = lgb.train(params,
                lgb_train,
                valid_sets=lgb_eval,
                verbose_eval=False
               )

gbm_test_pred = gbm.predict(test_meta, num_iteration=gbm.best_iteration)
```


```python
# Write out results to file
submission=sample.copy()
submission.target=gbm_test_pred
submission.to_csv('6.csv.gz', compression='gzip', index=False)
```
