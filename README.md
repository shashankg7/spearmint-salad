# Spearmint Salad
This python software is an implementation of the Sequential Model-Based Ensemble Optimization (ESMBO) algorithm [[1](http://arxiv.org/abs/1402.0796)].  It combines [Spearmint](https://github.com/JasperSnoek/spearmint) [[2](https://nips.cc/Conferences/2012/Program/event.php?ID=3571)] for fast hyperparameter optimization with the Agnostic Bayes theory [[3](http://jmlr.org/proceedings/papers/v32/lacoste14.html)] to generate an ensemble of learning algorithms over the hyperparameter space for increasing the generalization performances.

## Features
* **Fast hyperparameter optimization** via [Spearmint](https://github.com/JasperSnoek/spearmint), based on a gaussian process modelization of the hyperparameter search space.
* **State of the art ensemble of learning algorithms** with no extra computational cost at learning time. 
* **Compatible with any programming language** via a configurable command line interface (*coming soon*).
* **3D vizualization** of the hyperparameter space based on [mayavi](http://code.enthought.com/projects/mayavi/) (Optional dependency).
* **Easy parallelization** via python's multiprocessing or an implementation of an mpi queue for running on a computer grid (*alpha*).
* **Crash recovery** (*coming soon*). 
* **Anytime algorithm.** You can obtain the best predictor so far and visualize the behavior at anytime.
* **MongoDB compatible**. All information gathered during the optimization process is stored in a MongoDB-like structure. 

## Dependencies
* numpy, scipy
* matplotlib (Optional, for visualization)
* mayavi, traits (Optional, for 3D visualization)
* mpi4py, openmpi (Optional, for parallelization over computational grid)
* scikit-learn (Optional, for playing with the provided examples)

## Usage

First, you have to define a hyperparameter search space. To do so, we provide an elegant pythonic tool that mimics the way you would normally instantiate your learning algorithm. Example using SVR from scikit learn:
```python
from spearmint_salad import hp
from sklearn.svm import SVR

# Encapsulate the class into a hp.Obj to be able to instantiate using variable parameters 
# or constant parameters if necessary.
hp_space = hp.Obj(SVR)(
    C = hp.Float( min_val=0.01, max_val=1000, hp.log_scale ), # variable
    kernel = 'rbf', # constant
    gamma = hp.Float( min_val=10**-5, max_val=1000, hp.log_scale ), # variable
    epsilon = hp.Float(min_val=0.01, max_val=1, hp.log_scale), # variable
)
```
Next, it suffices to start the optimization for a given metric on a particular dataset

```python
from spearmint_salad import metric

metric = metric.SquareDiffLoss()
make_salad( hp_space, metric, dataset_path)
```
During the optimization, you can launch the visualization tool
```bash
$ viz.py
```

![](https://raw.github.com/wiki/recursix/spearmint-salad/viz.png)

## References
[1] Lacoste, Alexandre, Hugo Larochelle, François Laviolette, and Mario Marchand. "Sequential Model-Based Ensemble Optimization." *arXiv* preprint arXiv:1402.0796 (2014).

[2] Snoek, Jasper, Hugo Larochelle, and Ryan P. Adams. "Practical Bayesian Optimization of Machine Learning Algorithms." In *NIPS*, pp. 2960-2968. 2012.

[3] Lacoste, Alexandre, Mario Marchand, François Laviolette, and Hugo Larochelle. "Agnostic Bayesian Learning of Ensembles." In *Proceedings of The 31st International Conference on Machine Learning*, pp. 611-619. 2014.

