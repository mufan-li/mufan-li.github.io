---
layout: post
title: Conformal Invariance of Brownian Motion and Estimates on Hitting Distributions
comments: true
---

Beyond characteristic functions, there are few connections between probability theory and complex analysis. However, recent developments in Schramm-Loewner evolutions (SLEs) have opened up many unexpected links between stochastic processes and other areas of mathematics. A key result at the centre of all SLE theory is the property of conformal invariance, which simplifies many technicalities using complex analysis that would be otherwise very challenging. Without going into too many technical details, this blog post will attempt to provide an intuitive overview of results demonstrating the important consequences of conformal invariance. We will then discuss an application in estimating the hitting distributions of Brownian motion. 

## Conformal Maps

Before studying stochastic processes, we will review a bit of complex analysis. The most important intuition to take away is that almost any reasonable set can be *stretched* via a conformal map to a *nice* set, like the unit disk or half plane. This important result is called the **Riemann Mapping Theorem**, but before we get there, we will need to recall several definitions. 

Let $$U,V \subset \mathbb{C}$$ be domains, 
and we say that $$f:U\to V$$ is **holomorphic** 
if it is complex differentiable, i.e. for all $$z\in U$$, 
the limit 

$$ f'(z) := \lim_{w \to z} \frac{f(w) - f(z)}{w-z} 
    \quad \text{exists}.
$$

The map $$f$$ is called **conformal** if it is a holomorphic bijection.
A domain $$U\subset \mathbb{C}$$ is called **simply connected** 
if $$\mathbb{C} \setminus U$$ is connected.
We also define two important simply connected sets, 
first is the unit disk $$\mathbb{D}:= \{z \in \mathbb{C} : |z| < 1\}$$
and the other is the upper half plane 
$$\mathbb{H}:=\{z \in \mathbb{C} : \text{Im}(z) > 0\}$$.

We will next state the key result.

**Theorem (Riemann Mapping)**
Suppose that $$U$$ is a simply connected domain with 
$$U \neq \mathbb{C}$$ and $$x \in U$$. 
Then there exists a unique conformal map $$f:\mathbb{D}\to U$$ 
with $$f(0) = x$$ and $$f'(0) > 0$$.

`Bill: consider adding a picture here to explain the intuition`

`Bill: explain `











