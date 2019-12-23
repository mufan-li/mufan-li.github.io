---
layout: post
title: The Auffinger-Chen Representation
comments: true
---

Equivalent representation results contribute not only a connection between different concepts, but also a new set of proof techniques. Indeed, [stochastic analysis](https://en.wikipedia.org/wiki/Stochastic_calculus) has offered a number of alternative proofs to many problems. Occasionally the proof can simplify drastically. In this post, we will discuss a particularly elegant application by Auffinger and Chen (2015), for an otherwise very difficult problem in spin glass. 




## The Problem Statement

For the sake of writing a self-contained blog post, we will not attempt to provide a description of spin glass models. Instead, we will state the problem in the most mathematically interesting form, without explaining where the quantities came from. 

Let $$\xi:[0,1]\to \mathbb{R}$$ be twice differentiable and strictly increasing and strictly convex (i.e. $$\xi', \xi'' > 0$$), also let $$\zeta:[0,1] \to [0,1]$$ be a cumulative distribution function (CDF). We will consider the Parisi partial differential equation (PDE) defined as follows

$$ 
\begin{cases}
    \partial_t \Phi = \frac{ - \xi''(t) }{2} \left[
        \partial_{xx} \Phi + 
        \zeta(t) \left( \partial_x \Phi \right)^2
    \right], \\
    \Phi(1,x) = \log \cosh(x) \,, 
\end{cases}
$$

where the time derivative is defined by the right limit for when $$\zeta(t)$$ is discontinuous. 

It is well known that we can solve this PDE backwards in time using a [Hopf-Cole transformation](https://en.wikipedia.org/wiki/Burgers%27_equation#Solution_of_viscous_Burgers'_equation); in fact, we will provide a sketch in a <a href="#sketch-of-strict-convexity">a later section</a>. This allows us to state an optimization objective as follows:

$$ \inf_{\zeta} \Phi_\zeta(0,x),
$$

where we are minimizing over the set of all CDFs on $$[0,1]$$ for each $$x \in \mathbb{R}$$. Finally we can state the question as follows: 

**Question** Does there exist a unique minimizer to the optimization problem $$\inf_{\zeta} \Phi_\zeta(0,x)$$ for each $$x\in\mathbb{R}$$?

The main difficulty comes from the unclear dependence on $$\zeta$$, even if we can write down a closed form solution to the Parisi PDE. At the very least, it would be extremely unpleasant and tedious to work with. Additionally, we remark that the problem is already stated in a simplified form, as opposed to the original framing in spin-glass. 

Before we jump into the main results, we observe that existence of a minimizer is straight forward to prove. Since we are restricted to the domain $$[0,1]$$, any sequence of probability measures is [tight](https://en.wikipedia.org/wiki/Tightness_of_measures). It is then sufficient to consider any sequence of probability measures $$\{\zeta_n\}$$ that minimizes $$\Phi_\zeta(0,x)$$, and tightness implies there exist a converging subsequence such that $$\zeta_{n_k} \to \zeta^*$$ weakly, which is a minimizer of $$\Phi(0,x)$$.








## The Auffinger-Chen Representation 

To complete the proof, it is sufficient to show $$\Phi(0,x)$$ is strictly convex in $$\zeta$$. In this section, we will use a stochastic representation to show convexity, which is the main difficulty of the problem. Readers unfamiliar with stochastic analysis can find a brief introduction in a [previous blog post](https://mufan-li.github.io/stone_ito/), in particular we will use [It&ocirc;'s Lemma](https://en.wikipedia.org/wiki/It%C3%B4%27s_lemma) in the upcoming proofs.

We start by defining $$B_t := W_{\xi'(t)}$$, where $$\{W_t\}$$ is a standard Brownian motion. Let $$\{\mathcal{F}_t\}_{t\geq 0}$$ be $$\{W_t\}$$'s canonical filtration, and then we define a collection of processes

$$ \mathcal{D} := \left\{ (u_t)_{0 \leq t \leq 1}
    : u_t \text{ is adapted to } \mathcal{F}_t, 
    |u_t| \leq 1
    \right\}.
$$

For simplicity of notation, we will write $$\sigma(t) = \sqrt{\xi''(t)}$$ for this section. At this point we will state the main result. 

---

**Theorem (Auffinger-Chen Representation)**
For all $$\zeta$$ a probability distribution on $$[0,1]$$, we have the following 

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

In particular, we have the maximizer is unique, and is given by $$u_s = \partial_x \Phi(s, x + X_s)$$, where $$X_s$$ is the strong solution of the following stochastic differential equation (SDE)

$$ dX_s = \sigma^2(s) \, \zeta(s) \, \partial_x \Phi(s, x + X_s) \, ds
    + \sigma(s) \, dW_s,
    \quad X_0 = 0.
$$

---


**Remark** Before we begin the proof, we will observe that $$\Phi(0,x)$$'s convexity **follows directly from this representation**. Firstly both integral terms containing $$\zeta$$ are linear in $$\zeta$$. Since $$\Phi(1,x) = \log \cosh (x)$$ is convex in $$x$$, we have the $$\Phi$$ term is convex in $$\zeta$$. Next the expectation over the sum of two convex functions remain convex. Finally, a maximum (or supremum) over convex functions remain convex, proving the desired convexity result! 


Before we start, we will state several technical (but not difficult to prove) Lemmas. To guarantee a [strong solution of the SDE](https://en.wikipedia.org/wiki/Stochastic_differential_equation#Existence_and_uniqueness_of_solutions), it is sufficient to have $$\partial_x \Phi(s, x)$$ be Lipschitz in $$x$$. We will omit the proof of these results as they are not important to the main goal of this blog post. Instead we will state the following Lemma containing the desired estimates.

---

**Lemma (Derivative Estimates)**
For all $$\zeta$$ probability distributions on $$[0,1]$$, we have that 

$$ |\partial_x \Phi(t, x)| \leq 1, 
    |\partial_{xx} \Phi(t,x)| \leq 1.
$$

---

Another important result we will omit is the continuity of $$\Phi$$ in $$\zeta$$.

---

**Lemma (Lipschitz in $$L^1$$)** 
For any discrete distributions $$\zeta_1, \zeta_2$$, 
and for all $$k \in \mathbb{N}$$, 
we have that 

$$\begin{align}
    \left| \Phi_{\zeta_1} - \Phi_{\zeta_2} \right| 
    &\leq \xi''(1) \int_0^1 |\zeta_1(t) - \zeta_2(t)| dt, \\
    \left| \partial_x^k \Phi_{\zeta_1}(t,x) - 
        \partial_x^k \Phi_{\zeta_2}(t,x)
    \right| 
    &\leq c_k \, \xi''(1) \int_0^1 |\zeta_1(t) - \zeta_2(t)| dt.
\end{align}$$

---

Since we can approximate any distributions in $$L^1$$ by discrete distributions, then we can extend the definition of $$\mathcal{P}(\cdot)$$ and $$\Phi(t,x)$$ to all distributions by continuity. Therefore it is sufficient to prove the result for only finitely supported distributions. 

*proof (of the Auffinger-Chen representation):*
The proof will be a straight forward application of It&ocirc;'s Lemma, 
and the results follow almost directly from invoking the Parisi PDE. 

We start with discrete $$\zeta$$, i.e $$\zeta$$ is a piecewise constant function. Let $$u \in \mathcal{D}$$, and define 

$$ dX_s := \sigma^2(s) \, \zeta(s) \, u_s \, ds
    + \sigma(s) \, dW_s, 
    \quad X_0 = 0,
$$

and let $$Y_s := \Phi(s, x + X_s)$$. Then we observe that

$$ X_1 = \int_0^1 \sigma^2(s) \, \zeta(s) \, u_s \, ds 
    + \int_0^1 \sigma(s) \, dW_s
$$

appears exactly inside the first $$\Phi$$ term of the Auffinger-Chen representation.

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










## Sketch of Strict Convexity

At this point, the author believes the goal of the blog post is already achieved: we have demonstrated the key technique with only very basic manipulations. That being said, to complete the story, we will provide a short sketch on how to prove strict convexity - hence proving there is a unique minimizer of $$\Phi_\zeta(0,x)$$.

We once again start with a key technical lemma.

---

**Lemma (Strict Convexity in $$x$$)**
For all $$\zeta$$ a probability distribution on $$[0,1]$$, 
and for all $$s \in [0,1]$$, we have 

$$ \partial_{xx} \Phi(s,x) > 0.
$$

---

Here we remind the reader that strict convexity in $$x$$ does not directly imply strict convexity in $$\zeta$$. We could just take this result for granted, but there is a nice proof using the Hopf-Cole transform and another stochastic representation, so why not?

*sketch (of Lemma):* Since $$\Phi(t,x)$$ is continuous in $$\zeta$$, we will only consider a discrete $$\zeta$$. Then using an appropriate time change and time reversal, we can get a new PDE 

$$ \partial_t \Phi = \frac{1}{2 \widehat \zeta(t)} \partial_{xx} \Phi
    + \frac{1}{2} (\partial_x \Phi)^2, 
$$

with **initial conditions** (as opposed to terminal conditions) 
$$\Phi(0,x)=\log \cosh(x)$$, and $$\widehat \zeta(t) = \zeta(1 - t)$$ changed due to time reversal. 
To simplify the PDE, we use the Hopf-Cole transformation to substitute 
$$\phi = \exp\left( \frac{\Phi}{\widehat\zeta(t)} \right) $$, which leads to the simplified linear PDE 

$$ \partial_t \phi = \frac{1}{2 \widehat \zeta(t)} \partial_{xx} \phi, 
$$

with initial conditions $$\phi(0,x) = \frac{1}{\widehat\zeta(t)} \log \exp\left( \frac{\log \cosh(x)}{\widehat \zeta(t)} \right) = \cosh(x)^{1/\widehat \zeta(t)}$$. Using another time change, we can also remove the $$\widehat \zeta(t)$$ above. 

Here we can use any of the reader's favourite method: the [Feynman-Kac Representation](https://en.wikipedia.org/wiki/Feynman%E2%80%93Kac_formula), the [Kolmogorov backward equation](https://en.wikipedia.org/wiki/Kolmogorov_backward_equations_(diffusion)), or the [heat kernel](https://en.wikipedia.org/wiki/Heat_kernel) to write 

$$ \phi(t,x) = \mathbb{E} \cosh( x + W_t )^{1/\widehat \zeta(t)}, 
$$

where $$W_t$$ is a standard Brownian motion, and $$\widehat \zeta$$ is constant in $$[0,t)$$. At this point it is sufficient to show strict convexity for this $$t$$, since we can piece together the constant intervals later. To this end, we will write 

$$ \Phi(t,x) = \frac{1}{\widehat \zeta(t)} \log \mathbb{E} \cosh(x + W_t)^{1/\widehat \zeta(t)},
$$

and define 

$$ \langle f(W_t) \rangle := \frac{
    \mathbb{E} f(W_t) \cosh(x + W_t)^{1/\widehat \zeta(t)}
}{
    \mathbb{E} \cosh(x + W_t)^{1/\widehat \zeta(t)}
} \, ,
$$

where we observe since $$\cosh(x) > 0$$ and $$\mathbb{E}\cosh(x + W_t) < \infty$$, we have that $$\langle \cdot \rangle$$ defines a new probability measure. In particular, we have Jensen's inequality.

With this we can take the second derivative of $$\Phi$$ to get

$$ 
\begin{align}
    \partial_{xx} \Phi(t,x) &= 
    \frac{-1}{\widehat \zeta(t)} \left\langle 
        \frac{1}{\widehat \zeta(t)} \tanh(x + W_t)
        \right\rangle^2 \\
        & \quad+ 
        \frac{1}{\widehat \zeta(t)}
        \left\langle \left( \frac{1}{\widehat \zeta(t)} \tanh(x + W_t) \right)^2
        + \frac{1}{\widehat \zeta(t)} \left( 1 - \tanh(x + W_t)^2 \right)
        \right\rangle  \\
        &\geq \frac{1}{\widehat \zeta(t)}
        \left\langle \frac{1}{\widehat \zeta(t)} \left( 1 - \tanh(x + W_t)^2 \right)
        \right\rangle \\
        &> 0 \, ,
\end{align}
$$

where we used Jensen's inequality and the fact that $$\tanh(x)^2 < 1$$.

<!-- 
Observe that since $$\widehat \zeta(t) \in [0,1]$$, the second term in the square brackets $$[\cdots]$$ are always greater than 1. Then we can use Jensen's inequality to write 

$$ \partial_{xx} \Phi(t,x) \geq 
    \frac{1}{\widehat \zeta(t)} \mathbb{E} \frac{1}{\widehat \zeta(t)} \log \cosh (x + W_t).
$$

Since $$\log \cosh(x) > 0$$ unless $$x = 0$$, we have the right hand side must be strictly positive, hence implying $$\partial_{xx}\Phi(t,x) > 0$$.
 -->












---

Finally we return to strict convexity in $$\zeta$$.

*sketch (of Strict Convexity in $$\zeta$$):*
We will start by introducing quantities related to convexity.
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

Recall the original problem of $$\inf_\zeta \Phi_\zeta(0,x)$$. 
We have shown that while the dependence structure is unclear, 
we are able to prove its convexity with it easily 
using a stochastic representation. 
The author would like to point out that 
most techniques used here are quite basic, 
which is surprising for an originally very difficult problem. 

The author would also like to point to a more general 
variational stochastic representation by 
[Bou&eacute; and Dupuis (1998)](https://projecteuclid.org/euclid.aop/1022855876), 
perhaps more useful for other applications. 

Finally the post would not be possible without attending 
an excellent graduate course on spin glass taught by Dmitry Panchenko, 
where he has done a much better job explaining this topic. 
In particular, Dmitry has written an excellent book (Panchenko, 2013) with a bonus chapter covering this topic that can be found [online](https://drive.google.com/file/d/0B6JeBUquZ5BwRFpLVjdVd3IwV1E/view?usp=drive_open). 
I would also highly recommends Dmitry's 
[notes on probability theory](https://sites.google.com/site/panchenkomath/lecture-notes), 
which has been in general very helpful to the author's 
studies and research. 




## References

 - Auffinger, A., & Chen, W. K. (2015). The Parisi formula has a unique minimizer. Communications in Mathematical Physics, 335(3), 1429-1444.
 - Bou&eacute;, M., & Dupuis, P. (1998). A variational representation for certain functionals of Brownian motion. The Annals of Probability, 26(4), 1641-1659.
 - Panchenko, D. (2013). The Sherrington-Kirkpatrick model. Springer Science & Business Media.
























