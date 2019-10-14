[![PyPI version](https://badge.fury.io/py/mltb.svg)](https://badge.fury.io/py/mltb)
[![License](https://img.shields.io/github/license/philipmay/mltb.svg)](https://github.com/PhilipMay/mltb/blob/master/LICENSE)


# Machine Learning Tool Box
This is the machine learning tool box. A collection of userful machine learning tools intended for reuse and extension.
The toolbox contains the following modules:
* hyperopt - Hyperopt tool to save and restart evaluations
* keras - Keras callback for various metrics and various other Keras tools
* lightgbm - metric tool functions for LightGBM
* metrics - several metric implementations
* plot - plot and visualisation tools
* tools - various (i.a. statistical) tools

## Module: hyperopt
This module contains a tool function to save and restart Hyperopt evaluations.
This is done by saving and loading the ``hyperopt.Trials`` objects.
The usage looks like this:
```
from mltb.hyperopt import fmin
from hyperopt import tpe, hp, STATUS_OK


def objective(x):
    return {
        'loss': x ** 2,
        'status': STATUS_OK,
        'other_stuff': {'type': None, 'value': [0, 1, 2]},
        }


best, trials = fmin(objective,
    space=hp.uniform('x', -10, 10),
    algo=tpe.suggest,
    max_evals=100,
    filename='trials_file')

print('best:', best)
print('number of trials:', len(trials.trials))
```

Output of first run:
```
No trials file "trials_file" found. Created new trials object.
100%|██████████| 100/100 [00:00<00:00, 338.61it/s, best loss: 0.0007185087453453681]
best: {'x': 0.026805013436769026}
number of trials: 100
```

Output of second run:
```
100 evals loaded from trials file "trials_file".
100%|██████████| 100/100 [00:00<00:00, 219.65it/s, best loss: 0.00012259809712488858]
best: {'x': 0.011072402500130158}
number of trials: 200
```

## Module: lightgbm
This module implements metric functions that are not included in LightGBM.
At the moment this is the F1- and accuracy-score for binary and multi class problems.
The usage looks like this:
```
bst = lgb.train(param,
                train_data,
                valid_sets=[validation_data]
                early_stopping_rounds=10,
                evals_result=evals_result,
                feval=mltb.lightgbm.multi_class_f1_score_factory(num_classes, 'macro'),
               )
```

## Module: keras
Has been removed. Use [metrics from Tensorflow Addons](https://github.com/tensorflow/addons/tree/master/tensorflow_addons/metrics) instead.
