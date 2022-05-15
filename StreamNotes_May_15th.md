# What we want to do today

* We want to get ArviZ loaded
* PyMC Loaded


## Why
It would be really great if people wanted to learn Probablistic Programming to have a self contained browser env to do so.

Reduces friction to get people started


### Benefits Educators
Educators need to do less work to get people the tools working

### Benefits for learners
* More opportunities to learn
* Can try things out without having to install and environment
  * Current Challenge: To even get started they typically need to setup Conda, or PyENV or something like that

## Cool thing about browser and Pyscript
* Everything has a browser now (pretty much)
* PyScript puts python in the browser
  * JupyterLite

## Summary
Getting tools into the browser makes them much much much more accessible

## What we doing today
We're going to try compiling packages that arent in pyodide, into versions that can be loaded into pyodide

Which ultimately means we can use them in pyscript


## References
### Installing arbitarry pypi packages
We want to try install ArviZ
https://pyodide.org/en/stable/usage/loading-packages.html
```
import micropip
micropip.install(
    'https://files.pythonhosted.org/packages/95/c0/dd9b4bf085d974de1c6d5fffea8e25153e8208ece6b363fa3a9263ed7e51/arviz-0.12.1-py3-none-any.whl'
)
```

### Next Goal
Compile a wheel of ArviZ that doesn't have netcdf4 so we can install it using micropip

#### Notes
`python setup.py bdist_wheel`
https://www.mssqltips.com/sqlservertip/6802/create-wheel-file-python-package-distribute-custom-code/

### Get Pandas running pyscript without explicit import
**Note** Figure out why keep going isn't working like I'd like it to
https://pyodide.org/en/stable/usage/api/micropip-api.html

#### Local Import of compiled wheel
```
micropip.install("./arviz-0.13.0.dev0-py3-none-any.whl")
micropip.install("./aesara-2.6.6+39.ge2e236685-py3-none-any.whl")
micropip.install("aeppl-0.0.28-py3-none-any.whl")
import arviz
```


#### End to End Get Plotting Working with just random samples
Need to build ArviZ wheel first

```python
micropip.install("numpy")
micropip.install("./arviz-0.13.0.dev0-py3-none-any.whl")
samples = np.random.normal(10, 2, size=1000)
import arviz
import numpy
import matplotlib.pyplot as plt

samples = np.random.normal(10, 2, size=1000)

fig, ax = plt.subplots()
arviz.plot_dist(samples, ax=ax)
fig
```


#### PyMC and its dependencies
Build wheels for all the below and load in as well
3. PyMC
2. AePPL
1. Aesara 

Goal: Get a PPL working in the browser, (Which would be incredible)


### PyMC Model
```python
samples = [0, 0, 0, 1, 1]

with pm.Model():
  p = pm.Beta("p",1,1)
  obs = pm.Bernoulli("obs",1,1)
  inf_data = pm.sample(samples=1000, cores=1)
```