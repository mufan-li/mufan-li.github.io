---
layout: post
title: A Drastically Simplifying Stochastic Representation in a Spin Glass Problem
comments: true
---

Equivalent representation results contribute not only 
a connection between different concepts, but also a new set of 
proof techniques. Indeed, 
[stochastic analysis](https://en.wikipedia.org/wiki/Stochastic_calculus) 
has offered a number of alternative proofs to many problems. 
Occasionally the proof can simplify drastically. 
In this post, we will discuss a particularly elegant application 
by Auffinger and Chen (2015), 
for an otherwise very difficult problem in spin glass. 

## Background and Motivation

For this blog post, we will not attempt to give a complete description for the massive topic of [spin glass](https://en.wikipedia.org/wiki/Spin_glass). For the interested readers, the author recommends an excellent [reference](https://sites.google.com/site/panchenkomath/books) by Dmitry Panchenko, which we will base most of this post on. 

**Remark** It is unnecessary to carefully track of all the notation in this background section to appreciate the later proof technique. In particular, this section is intended to provide only a brief context of the problem.

Consider the following Hamiltonian for a spin system

\\[ H_N(\sigma) := \sum_{p \geq 1} \beta_p H_{N,p}(\sigma)
    = \sum_{p \geq 1} \beta_p
        \frac{1}{N^{(p-1)/2}} \sum_{i_1, \ldots, i_p=1}^N
            g_{i_1,\ldots,i_p} \sigma_{i_1} \cdots \sigma_{i_p},
\\]

where $$N$$ is the number of particles, 
$$\sigma \in \{-1, +1\}^N$$ is the spin configuration, 
$$g_{i_1,\ldots,i_p}$$ are i.i.d. standard Gaussians describing 
the interaction between spins, 
and $$\beta_p \geq 0$$ are weighting of different interactions. 
**The exact definition is not so important** as long as we recognize 
that the spin configurations $$\sigma$$ determines 
the energy of a system based on random interactions
$$g_{i_1,\ldots,i_p}$$. 

The object of interest is the limit of 
a discrete optimization problem 
through a continuous relaxation

\\[ \lim_{N \to \infty} \frac{1}{N} \mathbb{E} \max_\sigma H_N(\sigma)
    = \lim_{N \to \infty} \frac{1}{N\beta} \mathbb{E}
        \log \sum_\sigma \exp\left( \beta H_N(\sigma)
            \right)
        + \mathcal{O}\left( \frac{1}{\beta} \right)
\\]

where $$\beta>0$$ is the inverse temperature. 

One amazing result which we will not do enough justice for is 
the Parisi formula, which gave a variational formula for 
the limit on the right hand side. 
For this, we will need to introduce several definitions first. 
Let us consider an integer $$r \geq 1$$, 
and define 

\\[ 0 < \zeta_0 < \cdots < \zeta_{r-1} < 1, \quad
    0 = q_0 < q_1 < \cdots < q_r = 1.
\\]

Observe that these values can be identified with 
a discrete probability measure, 
in particular a cumulative distribution function 
$$\zeta(t) = \zeta_p$$ for all $$t \in [q_p, q_{p+1})$$.

Let $$(\eta_p)_{0 \leq p \leq r}$$ be i.i.d. standard Gaussians, 
$$\xi(x) := \sum_{p\geq 1} \beta_p^2 x^p$$, 
$$h$$ be any random variable (external field) such that 
$$\mathbb{E} \exp(\lambda h) < \infty \; \forall \lambda \in \mathbb{R} $$,
and define 

\\[ X_r := \log \left[ 2 \cosh \left(
    h + 
    \eta_0 \xi'(0)^{1/2} + \sum_{1 \leq p \leq r}
        \eta_p \sqrt{ \xi'(q_p) - \xi'(q_{p-1}) }
    \right) \right].
\\]

We then recursively define for $$0 \leq l \leq r-1$$

\\[ X_l := 
    \frac{ 1 }{ \zeta_l} \log {\mathbb{E}}\_l
    \exp \left( \zeta_l X_{l+1} \right),
\\]

where $$\mathbb{E}_l$$ denotes the expectation 
conditioned on $$(\eta_p)_{0 \leq p \leq l}$$.

Lastly we define $$\theta(x) = x \xi'(x) - \xi(x)$$, 
and we can now finally define the famous 
**Parisi functional** as

$$\begin{align} \mathcal{P}(\zeta) :&= 
    \mathbb{E} X_0 - \frac{1}{2} \sum_{0 \leq p \leq r-1}
        \zeta_p \left( \theta(q_{p+1}) - \theta(q_p)
        \right) \\
    &= \mathbb{E} X_0
     - \frac{1}{2} \int_0^1 t \xi''(t) \zeta(t) dt.
\end{align}$$

We can now state the famous result (without the temperature term).

---

**Proposition (The Parisi Formula)**

\\[ \lim_{N \to \infty} \frac{1}{N} \mathbb{E}
        \log \sum_\sigma \exp\left( H_N(\sigma)
            \right)
    = \inf_\zeta \mathcal{P}(\zeta),
\\]

where the $$\inf$$ is taken over all discrete distributions defined as above.

---


## The Hard Problem

Given the Parisi formula, there is a natural question that we can ask:
*can we show the existence and uniqueness of a distribution 
that achieves the $$\inf$$ in the formula?*

Indeed, it is possible to show that 
the Parisi functional $$\mathcal{P}(\cdot)$$ 
is strictly convex in the space of all probability distributions 
on the interval $$[0,1]$$. 
However, we can observe that the interaction between $$\zeta$$ 
and $$\mathcal{P}$$ is highly complex. 
Notably the definition of $$X_r$$ depends on the positions 
$$\{q_p\}$$, 
and $$X_0$$ depend on the values $$\{\zeta_l\}$$ 
in a non-linear and recursive way. 
By perturbing $$\zeta$$, it is very difficult to 
understand the changes due to this complex dependence. 

In short, this problem is *hard*!

**Goal** The rest of this blog post will be dedicated to proving 
$$\mathcal{P}(\cdot)$$ is convex.
We will provide a brief discussion on how to show strict convexity.

## The Parisi PDE

Readers with a background in stochastic analysis 
may not be surprised at all to see 
[partial differential equations (PDEs)](https://en.wikipedia.org/wiki/Partial_differential_equation) 
in the section title here, as many results such as the 
[Feynman-Kac representation](https://en.wikipedia.org/wiki/Feynman%E2%80%93Kac_formula) 
have already established a deep connection between 
[It&ocirc; processes](https://en.wikipedia.org/wiki/It%C3%B4_calculus#It%C3%B4_processes)
and [parabolic PDEs](https://en.wikipedia.org/wiki/Parabolic_partial_differential_equation).

However we will not explore these in detail, 
as the proof requires a different representation. 
We will leave the main representation result for the next section, 
instead we first translate the original quantities using a PDE description.

We start this section by defining a function 
$$\Phi(t,x)$$ as follows. 
First of all let $$\Phi(1, x) = \log \cosh (x)$$. 
Then recursively for all $$0 \leq p \leq r-1$$, let 

\\[ \Phi(t,x) = \frac{1}{\zeta_p} \log \mathbb{E}
    \exp\left( \zeta_p \Phi(q_{p+1}, x + B_{q_{p+1}} - B_t)
    \right), 
    \quad \forall t \in [q_p, q_{p+1}),
\\]

where $$B_t = W_{\xi'(t)}$$, 
and $$\{W_t\}$$ a standard Brownian motion. 
Most importantly, we observe that since

\\[ \eta_p \sqrt{ \xi'(q_p) - \xi'(q_{p-1}) }
    \overset{d}{=} B_{q_p} - B_{q_{p-1}},
\\]

we can rewrite the definition of the Parisi functional 
using $$\Phi$$ as 

\\[ \mathcal{P}(\zeta) = \mathbb{E} \Phi(0, h) 
    - \frac{1}{2} \int_0^1 t \xi'\'(t) \zeta(t) dt.
\\]

**Remark** Since the second integral is already linear in $$\zeta$$, 
it is then sufficient to show that $$\Phi(0,x)$$ 
is strictly convex in $$\zeta$$.

A careful reader may have noticed that we have only defined 
everything in terms of discrete distributions only. 
Indeed, this is due to the fact that we have the following result.

---

**Lemma (Lipschitz in $$L^1$$)** 
For any discrete distributions $$\zeta_1, \zeta_2$$, 
and for all $$k \in \mathbb{N}$$, 
we have that 

$$\begin{align}
    \left| \mathcal{P}(\zeta_1) - \mathcal{P}(\zeta_2) \right| 
    &\leq \xi''(1) \int_0^1 |\zeta_1(t) - \zeta_2(t)| dt, \\
    \left| \partial_x^k \Phi_{\zeta_1}(t,x) - 
        \partial_x^k \Phi_{\zeta_2}(t,x)
    \right| 
    &\leq c_k \, \xi''(1) \int_0^1 |\zeta_1(t) - \zeta_2(t)| dt.
\end{align}$$

---

Since we can approximate any distributions in $$L^1$$ 
by discrete distributions, 
then we can extend the definition of 
$$\mathcal{P}(\cdot)$$ and $$\Phi(t,x)$$ 
to all distributions by continuity.

At this point we will begin to prove the first main result. 

---

**Theorem (The Parisi PDE)** For all probability distributions 
on $$[0,1]$$, we have that 

\\[ \partial_t \Phi = \frac{- \xi'\'(t)}{2} \left(
    \partial_{xx} \Phi + \zeta(t) (\partial_x \Phi)^2
    \right),
\\]

where $$\partial_t$$ is understood as the right derivative.

---

Before proving the PDE above, 
we will state a highly useful result, 
particularly for studying a wide range of problems in spin glass.

---

**Proposition (Gaussian Integration by Parts)**
Let $$g \sim N(0, \nu)$$ 
and $$F:\mathbb{R} \to \mathbb{R}$$ be differentiable.
If $$\mathbb{E} |F'(g)| < \infty$$, 
then we have the following formula 

\\[ \mathbb{E} g F(g) = 
    \nu \, \mathbb{E} F'(g).
\\]

---

*proof (of the Parisi PDE):*
We will directly compute both sides, 
and show they are equal using Gaussian integration by parts. 
The exact computation details are not particularly important, 
however the readers are recommended a quick skim 
to verify the proof is indeed simple. 

We begin by taking $$\zeta$$ to be discrete distribution, 
and fixing some $$t \in [q_p, q_{p+1})$$. 
We will also adopt concise notation and write $$\Phi := \Phi(t,x)$$
and $$\hat \Phi := \Phi(q_{p+1}, x + B_{q_{p+1}} - B_t)$$.
Then we can rewrite the original recursive definition as 

\\[ \Phi = \frac{1}{\zeta_p} \log \mathbb{E}
    \exp\left( \zeta_p \hat \Phi
    \right).
\\]

Since $$\eta_p \sqrt{ \xi'(q_p) - \xi'(q_{p-1}) }
    \overset{d}{=} B_{q_p} - B_{q_{p-1}}$$, 
we can compute for all nice functions $$F$$ 
such that we can differentiate under the expectation

$$\begin{align}
    \partial_t \mathbb{E} F( B_{q_{p+1}} - B_t )
    &= \mathbb{E} \partial_t F\left( 
        \eta_{p+1} \sqrt{ \xi'(q_{p+1}) - \xi'(t) } 
        \right) \\
    &= \mathbb{E} F' \left( 
        \eta_{p+1} \sqrt{ \xi'(q_{p+1}) - \xi'(t) } 
        \right) \eta_{p+1} \frac{- \xi''(t)}{2 \sqrt{ 
            \xi'(q_{p+1}) - \xi'(t)
        } } \\
    &= \mathbb{E} F'(B_{q_{p+1}} - B_t)
        \frac{- \xi''(t) (B_{q_{p+1}} - B_t) }{2 (\xi'(q_{p+1}) - \xi'(t))}.
\end{align}$$

Observe the function inside the expectation is [sufficiently nice
such that we can differentiate under the expectation](https://math.stackexchange.com/questions/217702/when-can-we-interchange-the-derivative-with-an-expectation).
As a result, we can compute the time derivative as 

$$\begin{align}
    \partial_t \Phi &= 
    \frac{1}{\zeta_p \mathbb{E} \exp(\zeta_p \hat \Phi) }
    \mathbb{E} \exp(\zeta_p \hat \Phi) \, \zeta_p \, 
        \partial_x \hat \Phi \, 
        \frac{- \xi''(t) (B_{q_{p+1}} - B_t) }{2 (\xi'(q_{p+1}) - \xi'(t))}.
\end{align}$$

At this point, we can apply Gaussian integration by parts 
with $$g = B_{q_{p+1}} - B_t$$, 
which is $$N(0, \xi'(q_{p+1}) - \xi'(t))$$, 
and $$F$$ as the rest of the expression to get 

$$\begin{align}
    & \mathbb{E} \exp(\zeta_p \hat \Phi) \, \zeta_p \, 
        \partial_x \hat \Phi \, 
    \frac{- \xi''(t) (B_{q_{p+1}} - B_t) }{2 (\xi'(q_{p+1}) - \xi'(t))} \\
    =& (\xi'(q_{p+1}) - \xi'(t)) 
        \frac{-\xi''(t)}{2 (\xi'(q_{p+1}) - \xi'(t))}
        \mathbb{E} \partial_x \left[ 
            \exp(\zeta_p \hat \Phi) \, \zeta_p \, \partial_x \hat \Phi
            \right] \\
    =& \frac{-\xi''(t)}{2} \mathbb{E} \exp(\zeta_p \hat \Phi)
        \left[ \zeta_p^2 (\partial_x \hat \Phi)^2 
            + \zeta_p \partial_{xx} \hat \Phi
        \right].
\end{align}$$

Then at this point, to get the Parisi PDE, we want to show 

$$\begin{align}
    \frac{1}{\mathbb{E} \exp(\zeta_p \hat \Phi) }
    \frac{-\xi''(t)}{2}
    \mathbb{E} \exp(\zeta_p \hat \Phi)
        \left[ \zeta_p (\partial_x \hat \Phi)^2 
            + \partial_{xx} \hat \Phi
        \right]
    = \frac{-\xi''(t)}{2} \left[
        \zeta_p (\partial_x \Phi)^2
        + \partial_{xx} \Phi
        \right].
\end{align}$$

To this end, we will first compute

\\[ \partial_x \Phi = \frac{1}{\zeta_p \mathbb{E} \exp(\zeta_p \hat \Phi)}
    \mathbb{E} \exp(\zeta_p \hat \Phi) \, \zeta_p \, 
        \partial_x \hat \Phi.
\\]

This implies 

$$\begin{align}
\partial_{xx} \Phi =& \frac{-1}{\zeta_p 
    \left(\mathbb{E} \exp(\zeta_p \hat \Phi) \right)^2} 
    \left[ \mathbb{E} \exp(\zeta_p \hat \Phi) \, \zeta_p \, 
        \partial_x \hat \Phi \right]^2 \\
    &+ \frac{1}{\zeta_p \mathbb{E} \exp(\zeta_p \hat \Phi)}
    \mathbb{E} \exp(\zeta_p \hat \Phi) \left[ \zeta_p \, 
            \partial_{xx} \hat \Phi
            + \zeta_p^2 (\partial_x \hat \Phi)^2
            \right].
\end{align}$$

Observe the first term above cancels when we compute 

\\[ \zeta_p (\partial_x \Phi)^2 + \partial_{xx} \Phi
    = \frac{1}{\zeta_p \mathbb{E} \exp(\zeta_p \hat \Phi)}
    \mathbb{E} \exp(\zeta_p \hat \Phi) \left[ \zeta_p \, 
            \partial_{xx} \hat \Phi
            + \zeta_p^2 (\partial_x \hat \Phi)^2
            \right],
\\]

hence proving the Parisi PDE for discrete distributions.

To extend to all distributions, 
we will rewrite the Parisi PDE in the integral form 
to remove the time derivative 

\\[ \Phi(t_2, x) - \Phi(t_1, x) = 
    \int_{t_1}^{t_2} \frac{-\xi'\'(t)}{2} \left[
        \partial_{xx} \Phi(t,x)
        + \zeta(t) (\partial_x \Phi(t,x))^2
        \right] dt.
\\]

Then by Lipschitz continuity in $$\zeta$$, 
we can extend the Parisi PDE to all distributions.

$$\tag*{$\Box$}$$






## The Auffinger-Chen Representation

Recall from the last section, it is now sufficient to prove 
that $$\Phi_\zeta(0,x)$$ is convex in $$\zeta$$. 
In this section, we will take advantage of the PDE structure 
and prove the stochastic representation that ultimately simplifies 
the problem. Readers unfamiliar with stochastic analysis 
can find a brief introduction in a 
[previous blog post](https://mufan-li.github.io/stone_ito/), 
in particular we will use 
[It&ocirc;'s Lemma](https://en.wikipedia.org/wiki/It%C3%B4%27s_lemma) 
in the upcoming proofs.

We start by recalling $$B_t := W_{\xi'(t)}$$, 
where $$\{W_t\}$$ is a standard Brownian motion. 
Let $$\{\mathcal{F}_t\}_{t\geq 0}$$ be $$\{W_t\}$$'s canonical filtration, 
and then we define a collection of processes

$$ \mathcal{D} := \left\{ (u_t)_{0 \leq t \leq 1}
    : u_t \text{ is adapted to } \mathcal{F}_t, 
    |u_t| \leq 1
    \right\}.
$$

For simplicity of notation, we will write 
$$\sigma(t) = \sqrt{\xi''(t)}$$ for this section. 
At this point we will state the main result. 

---

**Theorem (Auffinger-Chen Representation)**
For all $$\zeta$$ a probability distribution on $$[0,1]$$, 
we have the following 

$$\begin{align}
    \Phi(0,x) = \max_{u \in \mathcal{D}} \bigg[
        \mathbb{E} & \Phi\left(1, 
        x + \int_0^1 \sigma^2(s) \, \zeta(s) \, u_s \, ds
        + \int_0^1 \sigma(s) \, dW_s
    \right) \\
        &- \frac{1}{2} \int_0^1 \sigma^2(s) \, \zeta(s) \,
        \mathbb{E} u_s^2  \, ds
    \bigg].
\end{align}$$

In particular, we have the maximizer is unique, 
and is given by $$u_s = \partial_x \Phi(s, x + X_s)$$, 
where $$X_s$$ is the strong solution of the following 
stochastic differential equation (SDE)

$$ dX_s = \sigma^2(s) \, \zeta(s) \, \partial_x \Phi(s, x + X_s) \, ds
    + \sigma(s) \, dW_s,
    \quad X_0 = 0.
$$

---


**Remark** Before we begin the proof, 
we will observe that $$\Phi(0,x)$$'s convexity 
**follows directly from this representation**. 
Firstly both integral terms containing $$\zeta$$ are linear in $$\zeta$$. 
Since $$\Phi(1,x) = \log \cosh (x)$$ is convex in $$x$$, 
we have the $$\Phi$$ term is convex in $$\zeta$$. 
Next the expectation over the sum of two convex functions remain convex. 
Finally, a maximum (or supremum) over convex functions remain convex, 
proving the desired convexity result! 

Another quick note, to guarantee a 
[strong solution of the SDE](https://en.wikipedia.org/wiki/Stochastic_differential_equation#Existence_and_uniqueness_of_solutions), 
it is sufficient to have $$\partial_x \Phi(s, x)$$ 
be Lipschitz in $$x$$. 
We will omit the proof of these results 
as they are not important to the main goal of this blog post. 
Instead we will state the following Lemma containing the desired estimates.

---

**Lemma (Derivative Estimates)**
For all $$\zeta$$ probability distributions on $$[0,1]$$, 
we have that 

$$ |\partial_x \Phi(t, x)| \leq 1, 
    |\partial_{xx} \Phi(t,x)| \leq 1.
$$

---

*proof (of the Auffinger-Chen representation):*
The proof will be a straight forward application of It&ocirc;'s Lemma, 
and the results follow almost directly from invoking the Parisi PDE. 

We start with discrete $$\zeta$$. 
Let $$u \in \mathcal{D}$$, and define 

$$ dX_s := \sigma^2(s) \, \zeta(s) \, u_s \, ds
    + \sigma(s) \, dW_s, 
    \quad X_0 = 0,
$$

and let $$Y_s := \Phi(s, x + X_s)$$.
Then we observe that

$$ X_1 = \int_0^1 \sigma^2(s) \, \zeta(s) \, u_s \, ds 
    + \int_0^1 \sigma(s) \, dW_s
$$

appears exactly inside the first $$\Phi$$ term of 
the Auffinger-Chen representation.

At this point we adopt concise notation and write 
$$\Phi := \Phi(s, x + X_s)$$, 
and apply It&ocirc;'s Lemma to $$Y_s$$ to get 

$$ dY_s = \left[ \partial_s \Phi + \sigma^2(s) \, \zeta(s) \, u_s \, 
    \partial_x \Phi + \frac{1}{2} \sigma^2(s) \, \partial_{xx} \Phi
    \right] ds
    + \sigma(s) \partial_x \Phi \, dW_s.
$$

Here we note while the time derivative $$\partial_s \Phi$$ 
does not exist at finitely many points, 
we will eventually only use it in integral form.
Using the Parisi PDE at points of continuity, 
we can make the following substitution

$$ \partial_s \Phi + \frac{1}{2} \sigma^2(s) \partial_{xx} \Phi
    = - \frac{1}{2} \sigma^2(s) \, \zeta(s) \, (\partial_x \Phi)^2.
$$

We will make the substitution and complete the square to get 

$$\begin{align}
    dY_s &= \left[ \sigma^2(s) \, \zeta(s) \, u_s \, \partial_x \Phi 
    - \frac{1}{2} \sigma^2(s) \, \zeta(s) \, (\partial_x \Phi)^2
    \right] ds
    + \sigma(s) \, \partial_x \Phi \, dW_s \\
    &= \left[ \frac{1}{2} \sigma^2(s) \, \zeta(s) \, u_s^2
        - \frac{1}{2} \sigma^2(s) \, \zeta(s) \,
            (u_s - \partial_x \Phi )^2
        \right] ds
        + \sigma(s) \, \partial_x \Phi \, dW_s.
\end{align}$$

Next we write this equation as an integral over $$[0,1]$$,
and taking expectation to remove the martingale term we get

$$\begin{align}
\mathbb{E} \Phi(1, x + X_1) - \Phi(0,x)
    =& \int_0^1 \frac{1}{2} \sigma^2(s) \, \zeta(s) \, 
        \mathbb{E} u_s^2 \, ds \\
    &- \int_0^1 \frac{1}{2} \sigma^2(s) \, \zeta(s) \,
        \mathbb{E} (u_s - \partial_x \Phi)^2 ds.
\end{align}$$

Since $$\Phi, \partial_x \Phi$$ are continuous in $$\zeta$$, 
we can extend this equation to all $$\zeta$$. 
Furthermore, since the second integral is always positive, 
we must have the inequality 

$$ \Phi(0,x) \geq \mathbb{E} \Phi(1, x + X_1) 
    - \int_0^1 \frac{1}{2} \sigma^2(s) \, \zeta(s) \, 
        \mathbb{E} u_s^2 \, ds, 
$$

and the inequality must be strict unless 
$$u_s = \partial_x \Phi$$ almost surely.

Observe this proves the inequality of the representation. 
Since $$|\partial_x \Phi| \leq 1$$, 
we have $$u_s = \partial_x \Phi \in \mathcal{D}$$, 
hence achieving the equality in the representation. 

$$\tag*{$\Box$}$$



## Short Note on Strict Convexity

While at this point, the author believes the goal of 
the blog post is already achieved: 
we have demonstrated the key technique with  
only very basic manipulations. 
That being said, to complete the spin glass story, 
we will provide a short sketch on how to prove strict convexity - 
hence proving there is a unique minimizer of 
the Parisi functional $$\mathcal{P}$$.

We once again start by stating a key technical lemma.

---

**Lemma (Strict Convexity in $$x$$)**
For all $$\zeta$$ a probability distribution on $$[0,1]$$, 
and for all $$s \in [0,1]$$, we have 

$$ \partial_{xx} \Phi(s,x) > 0.
$$

---

Here we remind the reader that strict convexity in $$x$$ 
does not directly imply strict convexity in $$\zeta$$. 
Now there are many ways to prove this result, 
the author speculates the simplest proof should follow directly 
from a Hopf-Cole transform into a linear heat equation, 
and using the fact that the heat equation preserves strict convexity
of the initial condition.

Next we will introduce quantities related to convexity.
Let $$\zeta_1 \neq \zeta_2$$, 
and let $$\zeta = \lambda \zeta_1 + (1-\lambda) \zeta_2$$
for some $$\lambda \in (0,1)$$.
Recalling $$\Phi(1,x) = \log \cosh (x)$$, 
and using the optimal $$u_s = \partial_x \Phi_\zeta(s, x + X_s)$$, 
where $$X_s$$ defined with respect to $$\zeta$$. 
Note this $$u_s$$ is not necessarily optimal for 
$$\zeta_1, \zeta_2$$.

Since $$\log\cosh(x)$$ is convex, we can write 

$$ \Phi_\zeta(0,x) \leq \lambda_1 A_1 + \lambda_2 A_2,
$$

where each $$A_i$$ is defined as

$$\begin{align}
A_i := \mathbb{E}& \, \log \cosh \left(
    x + \int_0^1 \sigma^2(s) \, \zeta_i(s) \, u_s \, ds
    + \int_0^1 \sigma(s) dW_s
    \right) \\
    & - \frac{1}{2} \int_0^1 \sigma^2(s) \, \zeta_i(s) \, 
        \mathbb{E} u_s^2 \, ds.
\end{align}$$

Since $$\log \cosh(x)$$ is strictly convex, 
the inequality is strict unless 

$$ \int_0^1 \sigma^2(s) \, \zeta_1(s) \, u_s \, ds
    = \int_0^1 \sigma^2(s) \, \zeta_2(s) \, u_s \, ds,
$$

almost surely.
Using the Auffinger-Chen representation, we have that 
$$A_i \leq \Phi_{\zeta_i}(0,x)$$. 
Therefore to prove the convexity is strict, 
it is sufficient to prove a gap in the first inequality, 
which is equivalent to saying that 

$$ Z := \int_0^1 \sigma^2(s) \, (\zeta_1(s) - \zeta_2(s)) \, u_s \, ds
$$

has positive variance.
The variance can be computed as  

$$ \text{Var}(Z) = \int_0^1 \int_0^1 \varphi(s) \, \varphi(t)
    \, \text{Cov}(u_s, u_t) \, ds dt,
$$

where $$\varphi(s) = \sigma^2(s) \, (\zeta_1(s) - \zeta_2(s))$$.

While we omit the technical details, 
it's not hard to believe $$u_s = \partial_x \Phi(s, x + X_s)$$
satisfy the following SDE
(from It&ocirc;'s Lemma and differentiating the Parisi PDE)

$$ du_s = \sigma(s) \partial_{xx} \Phi(s, x + X_s) dW_s.
$$

Observing that $$u_s$$ is a martingale with independent increments, 
we can compute $$\text{Cov}(u_s, u_t)$$ as 

$$ \text{Cov}(u_s, u_t) = \text{Var}(u_{s \wedge t})
    = \int_0^{s \wedge t} \sigma^2(v) \mathbb{E} 
        (\partial_{xx} \Phi(v, x + X_v))^2 dv,
$$

where the last step followed from 
[It&ocirc;'s Isometry](https://en.wikipedia.org/wiki/It%C3%B4_isometry).
Defining $$\tau(s) := \text{Var}(u_s)$$, 
we can also write $$\text{Cov}(u_s, u_t) = \tau(s) \wedge \tau(t)$$.
With a bit of algebra we can derive

$$ \text{Var}(Z) = \int_0^1 \left( \int_v^1 \varphi(s) ds \right)^2
    \tau'(v) dv.
$$

Since $$\tau'(v) = \sigma^2(v) \mathbb{E} 
(\partial_{xx} \Phi(v, x + X_v))^2 $$, 
the desired result follows from the fact $$\partial_{xx} \Phi > 0$$.



## Final Comments

Recall the definition of $$\mathcal{P}(\zeta)$$, 
we have shown that while the recursive structure is complicated, 
it led us to reformulate the problem as a PDE, 
and finally solving it using a stochastic representation. 
The author would like to point out that 
most techniques used here are quite straight forward, 
which is surprising for an originally difficult problem. 

The author would also like to point to a more general 
variational stochastic representation by 
[Bou&eacute; and Dupuis (1998)](https://projecteuclid.org/euclid.aop/1022855876), 
perhaps more useful for other applications. 

Finally the post would not be possible without attending 
an excellent graduate course on spin glass taught by Dmitry Panchenko, 
where he has done a much better job explaining this topic. 
The author also highly recommends his 
[notes on probability theory](https://sites.google.com/site/panchenkomath/lecture-notes), 
which has been in general very helpful to the author's 
studies and research. 




## References

 - Auffinger, A., & Chen, W. K. (2015). The Parisi formula has a unique minimizer. Communications in Mathematical Physics, 335(3), 1429-1444.
 - Bou&eacute;, M., & Dupuis, P. (1998). A variational representation for certain functionals of Brownian motion. The Annals of Probability, 26(4), 1641-1659.
 - Panchenko, D. (2013). The Sherrington-Kirkpatrick model. Springer Science & Business Media.





















