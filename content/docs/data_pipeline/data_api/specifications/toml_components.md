---
title: TOML Components
weight: 1
---

# TOML Components

There are only three types of TOML file:

* point-estimate
* distribution
* samples

These are all different ways of representing the estimate for a value. The value can be anything - the mean of something, the standard deviation, etc. The only way of describing that at the moment is the name - so you could have a data product called `latent-period`, and it could just have a single point estimate:

``` toml
[latent-period]
type = "point-estimate"
value = xxxx
```

The component is taken as the last part of the name (in the above example, period)... or it could have several components, for instance

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

As far as units are concerned, the idea is that the will be a unit-respecting system in there if we get the data pipeline grant, so every data product component will have to say what units it is in, and the pipeline api will do conversions and error if the units aren't compatible. However, it's not there yet. The only thing I can suggest at the moment is to decide to use the component names to specify the units, so add an additional component to the toml file such as:

``` toml
[hours]
type = "point-estimate"
value = 24.0
```

In the future... if you ask for a unit that isn't provided, you'll just get an error...
