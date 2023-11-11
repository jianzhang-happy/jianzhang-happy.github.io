# optuna

[TOC]

## optuna超参数搜索流程

1. 定义目标函数
2. 创建study对象（可以将study对象保存为数据库文件，其中保存了每次实验的参数及目标值变化情况；后续可以load此对象用于画图）
3. 用study对象调用optimize方法优化目标函数
4. 画图展示结果或者将结果保存为表格

## optuna优化方式

- 参数调优方式：optuna支持多种超参数搜索方式，通过Smpler参数调整搜索方式，默认采用TPESampler，optuna实现以下Sampler：

  - Grid Search implemented in [`GridSampler`](https://optuna.readthedocs.io/en/stable/reference/samplers/generated/optuna.samplers.GridSampler.html#optuna.samplers.GridSampler)
  - Random Search implemented in [`RandomSampler`](https://optuna.readthedocs.io/en/stable/reference/samplers/generated/optuna.samplers.RandomSampler.html#optuna.samplers.RandomSampler)
  - Tree-structured Parzen Estimator algorithm implemented in [`TPESampler`](https://optuna.readthedocs.io/en/stable/reference/samplers/generated/optuna.samplers.TPESampler.html#optuna.samplers.TPESampler)
  - CMA-ES based algorithm implemented in [`CmaEsSampler`](https://optuna.readthedocs.io/en/stable/reference/samplers/generated/optuna.samplers.CmaEsSampler.html#optuna.samplers.CmaEsSampler)
  - Algorithm to enable partial fixed parameters implemented in [`PartialFixedSampler`](https://optuna.readthedocs.io/en/stable/reference/samplers/generated/optuna.samplers.PartialFixedSampler.html#optuna.samplers.PartialFixedSampler)
  - Nondominated Sorting Genetic Algorithm II implemented in [`NSGAIISampler`](https://optuna.readthedocs.io/en/stable/reference/samplers/generated/optuna.samplers.NSGAIISampler.html#optuna.samplers.NSGAIISampler)
  - A Quasi Monte Carlo sampling algorithm implemented in [`QMCSampler`](https://optuna.readthedocs.io/en/stable/reference/samplers/generated/optuna.samplers.QMCSampler.html#optuna.samplers.QMCSampler)

- 剪枝（pruning）算法：剪枝算法在表现不好的参数实验时早停，optuna默认采用MedianPruner。optuna默认情况下不会剪枝，因为剪枝要求记录中间值，所以只有在循环遍历epoch的过程中加入中间值记录以及是否剪枝语句才能执行剪枝，对于大多数机器学习/深度学习框架，optuna实现了继承的剪枝callback以便更容易实现剪枝。optuna实现的剪枝算法：

  - Median pruning algorithm implemented in [`MedianPruner`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.pruners.MedianPruner.html#optuna.pruners.MedianPruner)

  - Non-pruning algorithm implemented in [`NopPruner`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.pruners.NopPruner.html#optuna.pruners.NopPruner)

  - Algorithm to operate pruner with tolerance implemented in [`PatientPruner`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.pruners.PatientPruner.html#optuna.pruners.PatientPruner)

  - Algorithm to prune specified percentile of trials implemented in [`PercentilePruner`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.pruners.PercentilePruner.html#optuna.pruners.PercentilePruner)

  - Asynchronous Successive Halving algorithm implemented in [`SuccessiveHalvingPruner`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.pruners.SuccessiveHalvingPruner.html#optuna.pruners.SuccessiveHalvingPruner)

  - Hyperband algorithm implemented in [`HyperbandPruner`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.pruners.HyperbandPruner.html#optuna.pruners.HyperbandPruner)

  - Threshold pruning algorithm implemented in [`ThresholdPruner`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.pruners.ThresholdPruner.html#optuna.pruners.ThresholdPruner)

    

## optuna创建目标函数

- optuna通过优化目标函数来执行超参数搜索，目标函数模板如下：

  ```python
  def objective(trial):
      
      clf = MLPClassifier(
          hidden_layer_sizes=tuple([trial.suggest_int('n_units_l{}'.format(i), 32, 64) for i in range(3)]),
          learning_rate_init=trial.suggest_float('lr_init', 1e-5, 1e-1, log=True),
      )
  
      for step in range(100):
          clf.partial_fit(x_train, y_train, classes=classes)
          value = clf.score(x_valid, y_valid)  
          
          # Report intermediate objective value.
          trial.report(value, step)
  
          # Handle pruning based on the intermediate value.
          if trial.should_prune():
              raise optuna.TrialPruned()  
  
      return value
  ```

- 需要注意的是这里的中间值记录需要在目标函数中写出epoch的循环过程，如果采用`keras`直接拟合则无法得到中间值，如果用`keras`需要展示中间值需要用`train_on_epoch`等方法。
- 参数搜索空间：



## study对象

### study对象保存与查看

- 在定义study对象时可以指定study_name和storage参数，storage参数指定study对象保存的位置，多个study对象可以保存在一个数据库中，因此最好指定study_name方便后续查看结果；
- storage参数的地址往往指定为相对路径，形式为：`sqlite:///+`相对路径，文件名需要以sqlite3为后缀；
- 保存的sqlite3文件可以通过opyuna-dashboard的[浏览器接口](https://optuna.github.io/optuna-dashboard/)打开，目前此接口仅支持部分图像。