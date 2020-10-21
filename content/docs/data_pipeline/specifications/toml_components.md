---
title: TOML components
weight: 1
---

# TOML components

There are only three types of TOML file:

* point-estimate
* distribution
* samples

These are all different ways of representing the estimate for a value, which can be anything - the mean of something, the standard deviation, etc.

## Single component

You could have a data product called `latent-period` with a single point estimate:

``` toml
[latent-period]
type = "point-estimate"
value = 1.0
```

In this case, the component is taken as the last part of the name (in the above example, period).

## Multiple components

Alternatively, the data product could have several components, for instance:

``` toml
[latent-period]
type = "distribution"
distribution = "gamma"
shape = 1.0
scale = 1.0

[mean]
type = "point-estimate"
value = 1.0

[standard-deviation]
type = "point-estimate"
value = 1.0
```

and so on.

As far as units are concerned, there will be a unit-respecting system in that (if we get the data pipeline grant) every data product component will have to say what units it is in, and the pipeline API will do conversions and error if the units aren't compatible (or aren't provided). However, it's not there yet. The only thing we can suggest at the moment is to decide to use the component names to specify the units, so add an additional component to the TOML file such as:

``` toml
[hours]
type = "point-estimate"
value = 24.0
```
