# Generalised Linear models

A linear model is defined on a basic level as:
```
Y=mx+c
```

This is typically rewritten for stats as:
```
y=𝛽0+𝛽1x1
```
Where:
- β are parameters
- x are features
- y is the target
 
This is a single single-variable linear regression. It is possible to produce multi-variable linear regressions as: 
```
f(x)=𝛽0+𝛽1x1+𝛽2x2+..+𝛽nxn
```
Ultimately the observations [y] will be normally distributed around your model target [f(x)]. This can shortened into the form of a 'General Linear Model':
```
y=N(f(x))
```

A 'Generalised Linear Model' is a more general more of this. It allows an distribution of results are a transformed linear regression:
```
y=O(g(x)f(x))
```