# Sep 19

## Anomaly detection filter

### Question about previous code

1. How to set the hyper-parameters of the dSPOT algorithm.

   Using the parameters in the code will cause nearly all series anomalies. (`"d": 10, "q": 1e-4, "n_init": 100, "level": 0.95`)

   Currently, I use the alarm numbers as the anomaly score and take the top n positions after sorting.

2. What is the `index` used in the original code? the label?

   ```python
   index = index_data[metric]
   ```

   > Make sure that all metrics use the same pods.

3. At the beginning of the code, it does `l1` normalization. But in the next part, it still doing max normalization. And `l1` normalization only be applied to the metric, but `max` normalization is applied to all data.

   ```python
   # first part of code
   X_metric = preprocessing.normalize(X_metric, axis=0, norm = 'l1')
   
   # second part of code
   X_max = np.max(X[:, :-1], axis = 0)
   X_max[X_max == 0] = 1
   X_norm = X[:, :-1]/X_max
   ```

4. The `idx_std` generated from the first constant detection is merely only used in metric removing. Does it have no use if we only use `cpu_usage` metrics?

   And why do we need to do twice constant detection?

   ```python
   # remove constant pods
   X_max = np.max(X[:, :-1], axis = 0)
   X_max[X_max == 0] = 1
   X_norm = X[:, :-1]/X_max
   std = np.std(X_norm, axis=0)
   idx_std = [i for i, x in enumerate(std > 1e-3) if x]
   
   if len(idx_std) == 0:
       metric_weight[metric_id] = 0
       metric_id = metric_id + 1
       print(metric,' all pods are all constant or quasi-constant')
       continue
   ```

5. What is the definition of `columns_common`?

6. Why does the normalization to the `ind_causal_score`? 

   ```python
   ind_causal_score = np.sum(ind_causal_score, axis=0)
           
   normalized_ind_causal_score = preprocessing.normalize([ind_causal_score], norm='l1').ravel()
   ```

   > anomaly score <- anomaly detection for each point
   >
   > `ind_causal_score`: matrix (length of time series, pod number)
   >
   > 100 pod, 1000, 
   >
   > pod-> 1000 score.
   >
   > 100 pod —> socre
   >
   > ranking score> 20%

### New code

> following the framework of previous code, this is the anomaly filter I used.

```python
from sklearn import preprocessing
from sklearn.feature_selection import VarianceThreshold
import ads_evt as spot
import numpy as np
import matplotlib.pyplot as plt

def anomaly_detection(data, depth, proba, n_init,plot=False):
    """
    Using dSPOT algorithm to detect the anomaly score
    """
    model = spot.dSPOT(q=proba, depth=depth)
    model.fit(init_data=n_init, data=data)
    model.initialize()
    results = model.run()
    score = len(results["alarms"])

    if plot and score >5000:
        figs = model.plot(results)
        plt.show()
    return score


def get_anomalous_cols(X, idx,depth=10,proba=1e-4,n_init=100, n_top=10,plot=False):
    """
    Perform anomaly detection on each column and return the top n_top columns with highest scores
    """
    cols = []
    for i in idx:
        score = anomaly_detection(data=X[:, i], depth=depth, proba=proba, n_init=n_init,plot=plot)
        cols.append((i, score))
    sorted_cols = sorted(cols, key=lambda x: x[1], reverse=True)[:n_top]
    anomalous_cols = [i[0] for i in sorted_cols]
    return anomalous_cols

patch = 10
downsample_data = np.sum(original_data[:original_data.shape[0] - original_data.shape[0] % patch].reshape(-1, patch, original_data.shape[1]), axis=1)
normalized_data = preprocessing.normalize(downsample_data, axis=0, norm='l1')
X, KPI = normalized_data[:, :-1], normalized_data[:, -1]

selector = VarianceThreshold(threshold=0)
X_var = selector.fit_transform(X)
support_idx = selector.get_support(indices=True) # support_idx is the index of the features that are not constant

anomalous_cols = get_anomalous_cols(X, support_idx,n_top=20,plot=True)
data = X[:, anomalous_cols]
labels = list(np.array(original_labels)[anomalous_cols])

print(data.shape)
print(len(labels))
print(labels)
```

## Datasets

### Nezha

**System classification**

* Online Boutique: 2022-08-22 and 2022-08-23
* Train ticket: 2023-01-29 and 2023-01-30

**Construct**

* [construct_data](https://github.com/IntelligentDDS/Nezha/blob/main/construct_data) is the data of the fault-free phase
* [rca_data](https://github.com/IntelligentDDS/Nezha/blob/main/rca_data) is the data of the fault-suffering phase

1. What is the difference between fault-free and fault-suffering data? How to use the fault-free dataset? if we need to use anomaly detection to filter entities.
2. What is trace and dependency?
3. what is source_50?
4. How to format the log data without a log template
5. If we use this dataset, should we compare with this method?

### GAIA



## Other Question

1. If we use thresholds to filter nodes, will it cause some causal intermediate nodes with unclear anomalies to be filtered and finally influence the RCA progress?

2. Should we add the constraint for the output from domain knowledge? Such as:

   ```text
   Your response should include:
   1. Introduction and analysis of the information and roles of {} and {}. # change
   2. The analysis of the causal relationship from {} to {}. This relationship should generated from your domain knowledge, the result from the causal discovery algorithm, and the summary from log data.
   ```

3. The method's prompt and code can't be used in various datasets. How to optimized this?



加入消融实验（没有LLMs的，没有Log的，）

加入uncertainty estimation的测试（画graph）





three public datasets: Auto mpg (Quinlan 1993), SACHS (Sachs et al. 2005), and GAIA (Generic AIOps Atlas) (AIOps community 2022). Auto mpg and SACHS are commonly used benchmarks in causal discovery, while GAIA is an anomaly detection dataset from a microservice application. We used the ground truth causal graphs from Takayama et al.(2024) for Auto mpg, and the provided ground truth graphs for SACHS and GAIA. The target variables were mpg, akt, and webservice for the three datasets, respectively. Detailed dataset descriptions and preprocessing methods are provided in the Supplementary Material.
