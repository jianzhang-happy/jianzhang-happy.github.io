# lightgbm

[TOC]

## :o: 要点

- optuna为lightgbm设计了两种参数寻优方式：
  - 第一种是基本的参数寻优方式，即定义一个optuna的objective函数进行参数寻优，在这种方式中==可以自定义需要调优的超参数==。
    - 这种方式调参的优点有：
      - 可以定制参数
      - ==可以画各种optuna支持的可视化调参过程图像==
    - :exclamation: 结果可复制的方式：==sampler中指定seed==，lightgbm的params中deterministic参数指定为True；另外random.seed,np.random.seed()最好也指定
  - 第二种是利用optuna.integration.lightgbm.LightGBMTuner,这种方式可以对lightgbm的==7个超参数==（`lambda_l1`, `lambda_l2`, `num_leaves`, `feature_fraction`, `bagging_fraction`, `bagging_freq`,`min_child_samples`）进行参数寻优。
    - 这种方式调参优点主要就是方便
    - :exclamation: 结果可复制的方式：lightgbm的params中deterministic参数指定为True，==指定optuna_seed==；另外random.seed,np.random.seed()最好也指定

## :one:自定义调参baseline

### 自定义调参介绍

- 通过定义objective对象并建立study即可利用optuna进行参数寻优
- optuna为lightgbm定义了一个`optuna.integration.LightGBMPruningCallback`对象用于剪枝，==并记录中间值用于调优过程展示==

### baseline

```python
import random
import lightgbm as lgb
import numpy as np
import sklearn.datasets
import sklearn.metrics
from sklearn.model_selection import train_test_split

import optuna

SEED = 42

np.random.seed(SEED)
random.seed(SEED)

def objective(trial):
    data, target = sklearn.datasets.load_breast_cancer(return_X_y=True)
    train_x, valid_x, train_y, valid_y = train_test_split(data, target, test_size=0.25)
    dtrain = lgb.Dataset(train_x, label=train_y)
    dvalid = lgb.Dataset(valid_x, label=valid_y)

    param = {
        "objective": "binary",
        "metric": "auc",
        "verbosity": -1,
        "boosting_type": "gbdt",
        "bagging_fraction": trial.suggest_float("bagging_fraction", 0.4, 1.0),
        "bagging_freq": trial.suggest_int("bagging_freq", 1, 7),
        "min_child_samples": trial.suggest_int("min_child_samples", 5, 100),
        "deterministic": True,
    }

    # Add a callback for pruning.
    pruning_callback = optuna.integration.LightGBMPruningCallback(trial, "auc")
    gbm = lgb.train(param, dtrain, valid_sets=[dvalid], callbacks=[pruning_callback])

    preds = gbm.predict(valid_x)
    pred_labels = np.rint(preds)
    accuracy = sklearn.metrics.accuracy_score(valid_y, pred_labels)
    return accuracy


study = optuna.create_study(
    direction="maximize",
    sampler=optuna.samplers.TPESampler(seed=SEED),
    pruner=optuna.pruners.MedianPruner(n_warmup_steps=10),
)
study.optimize(objective, n_trials=5, timeout=600)

# You can use Matplotlib instead of Plotly for visualization by simply replacing `optuna.visualization` with
# `optuna.visualization.matplotlib` in the following examples.
optuna.visualization.plot_optimization_history(study)
optuna.visualization.plot_intermediate_values(study)
optuna.visualization.plot_parallel_coordinate(study)
optuna.visualization.plot_parallel_coordinate(study, params=["bagging_freq", "bagging_fraction"])
optuna.visualization.plot_contour(study)
optuna.visualization.plot_contour(study, params=["bagging_freq", "bagging_fraction"])
optuna.visualization.plot_slice(study)
optuna.visualization.plot_slice(study, params=["bagging_freq", "bagging_fraction"])
optuna.visualization.plot_param_importances(study)
optuna.visualization.plot_param_importances(
    study, target=lambda t: t.duration.total_seconds(), target_name="duration"
)
optuna.visualization.plot_edf(study)
optuna.visualization.plot_rank(study)
optuna.visualization.plot_timeline(study)
```



## :two:tuner调参

### tuner调用介绍

- 利用optuna.integration.lightgbm.LightGBMTuner（以下简称LightGBMTuner）进行参数寻优可以直接按照按照lightgbm的python API直接调用train进行调参即可。

- 具体地，只需要将`import lightgbm as lgb`改为`import optuna.integration.lightgbm as lgb`即可，之后就可以调用`lgb.train`进行参数寻优。

- 这里调用的实际上是`optuna.integration.lightgbm.train`（以下简称train），train是LightGBMTuner的封装，既支持lightgbm的参数输入，也支持一些optuna自定义参数输入。

- LightGBMTuner的输入参数如下：

  ```python
  optuna.integration.lightgbm.LightGBMTuner(params, train_set, num_boost_round=1000, valid_sets=None, valid_names=None, feval=None, feature_name='auto', categorical_feature='auto', keep_training_booster=False, callbacks=None, time_budget=None, sample_size=None, study=None, optuna_callbacks=None, model_dir=None, verbosity=None, show_progress_bar=True, *, optuna_seed=None)
  ```

  - 其中params与lightgbm的参数一致，有几个需要注意的参数：
    - num_boost_round：num_boost_round在lightgbm中默认为100，optuna中改成了默认1000
    - optuna_seed：控制结果可复制性的参数，==需要注意的是：这个参数需要配合lightgbm的deterministic参数一起使用，因为lightgbm默认deterministic=False==。

- 最终得到的模型即为表现最好的超参数对应的模型，调用model.params即可返回最好的超参数，调用model.best_iteration返回最佳迭代次数。

### baseline

```python
"""
Optuna example that optimizes a classifier configuration for cancer dataset using LightGBM tuner.
In this example, we optimize the validation log loss of cancer detection.
"""
import random
import numpy as np
import optuna.integration.lightgbm as lgb

from lightgbm import early_stopping
from lightgbm import log_evaluation
import sklearn.datasets
from sklearn.metrics import accuracy_score
from sklearn.model_selection import train_test_split

random.seed(42)
np.random.seed(42)

# 二类分类问题,数据集规模为569*30
data, target = sklearn.datasets.load_breast_cancer(return_X_y=True)
train_x, val_x, train_y, val_y = train_test_split(data, target, test_size=0.25)
dtrain = lgb.Dataset(train_x, label=train_y)
dval = lgb.Dataset(val_x, label=val_y)

params = {
    "objective": "binary",
    "metric": "binary_logloss",
    "verbosity": 0,
    "boosting_type": "gbdt",
    "device": 'gpu',
    'deterministic': True,
}

model = lgb.train(
    params,
    dtrain,
    valid_sets=[dtrain, dval],
    callbacks=[early_stopping(100), log_evaluation(100)],
    optuna_seed=30,
)

# 二类分类,所以可以用np.rint将概率转换为类别
prediction = np.rint(model.predict(val_x, num_iteration=model.best_iteration))
accuracy = accuracy_score(val_y, prediction)

best_params = model.params
print("Best params:", best_params)
print("  Accuracy = {}".format(accuracy))
print("  Params: ")
for key, value in best_params.items():
    print("    {}: {}".format(key, value))
```

