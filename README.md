# Python Bayesian Tests

[![Build](https://github.com/tcassou/babtest/workflows/Test%20and%20Release/badge.svg)](https://github.com/tcassou/babtest/actions)

Bayesian Tests are a Bayesian alternative to classical hypothesis testing, you can read more about it [here](https://en.wikipedia.org/wiki/Bayes_factor).

## Setup
Simply install from `pip`:
```
pip install babtest
```

## Distributions currently available
* [Bernoulli distribution](https://en.wikipedia.org/wiki/Bernoulli_distribution)
* [Exponential distribution](https://en.wikipedia.org/wiki/Exponential_distribution)
* [Lognormal distribution](https://en.wikipedia.org/wiki/Log-normal_distribution)
* [Normal distribution](https://en.wikipedia.org/wiki/Normal_distribution)
* [Poisson distribution](https://en.wikipedia.org/wiki/Poisson_distribution)
* [Student's t-distribution](https://en.wikipedia.org/wiki/Student's_t-distribution)

## Example
Suppose you run an AB test between two populations, the outcome being one set of measures for each group (could be conversion, revenue, latency, etc).
We'll call these two data sets `control` (A group) and `variant` (B group):
```
>>> control
array([ 0.00892857,  0.0699088 , -0.101626  , ...,  0.323944  ,
        0.5       ,  0.236842  ])
>>> variant
array([ 0.0986842, -0.0176   ,  0.0571429, ...,  0.452941 , -0.304348 ,
        0.289474 ])
```

You simply need to create a `BABTest` object, specifying the data sets and the model you want to use. All available models can be found in `babtest.py`.
Here we'll assume the Student's t-distribution is a good description of our metric.
```
bt = BABTest(control, variant, model='student')
bt.run()
bt.plot()
```

This will typically produce the following plots:

### Data and model

![Data vs Model](https://github.com/tcassou/babtest/blob/master/example/data_vs_pred.png)
The metric distribution is shown for both `control` and `variant` sets (red bars), as well as a few plots of distributions estimated in the Monte Carlo sampling (blue lines).

In our example, one can eyeball that the model is a good fit for the data.

### Posterior knowledge of distribution parameters

![Posterior Parameters](https://github.com/tcassou/babtest/blob/master/example/posterior_distrib.png)
For each of the sets (`control`, `variant`), distribution parameters are estimated. This "knowledge" of parameters is described as a likelihood, or in other words denstity of credibility, which is represented in the above graphs.
In our example, it can be seen that the parameters of the two sets are extremely likely to be different, as their distributions are shifted and
* barely overlap in case of the mean,
* do not overlap in case of the other two parameters.

One could conclude that A and B groups are different here.

## Troubleshooting
### MacOS
#### Installing requirements
In order to install the `pymc` requirement, you'll need a fortran compiler. On MacOS simply install with `brew install gcc`
#### Matplotlib runtime error
You may run into this `matplotlib` issue:
> RuntimeError: Python is not installed as a framework. The Mac OS X backend will not be able to function correctly if Python is not installed as a framework.

Simply create a `~/.matplotlib/matplotlibrc` file with the following option: `backend: TkAgg`, which will change the `matplotlib` backend and solve the issue.

## Requirements
* `numpy>=1.11.3`
* `scipy>=0.18.1`
* `pymc>=2.3.6`
* `matplotlib>=1.5.3`
