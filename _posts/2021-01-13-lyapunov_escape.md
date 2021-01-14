---
title: On Escape Time, Lyapunov Function, Poincar&eacute; Inequality, and the KLS Conjecture Beyond Convexity 
---

Nobody has time to read an [80 page paper](https://arxiv.org/abs/2010.11176) 
\[LE20\]. 
Therefore I doubt most readers realized the manifold Langevin algorithm paper 
actually contains a novel technique for establishing functional inequalities. 
And I really doubt anyone had time to interpret the intuitive consequences 
of such results on perturbed gradient descent, 
and definitely not extending the Kannan-Lov&#225;sz-Simonovits (KLS) 
conjecture \[LV18\] - 
which brings me to write this blog post. 






## Background 

Let us start with a potential function $$F : \mathbb{R}^d \to \mathbb{R}$$, 
an inverse temperature parameter $$\beta > 0$$, 
and we define the **Gibbs density** as 

$$ \nu(x) := \frac{1}{Z} e^{ -\beta F(x) } \,, 
$$

where $$Z = \int e^{-\beta F(x)} dx$$ is the normalizing constant. 

We say $$\nu$$ satisfies the **Poincar&eacute; inequality** 
with constant $$\kappa > 0$$, 
denoted $$\text{PI}(\kappa)$$, if 

$$ \int f^2 \, d\nu - \left( \int f \, d\nu \right)^2 
    \leq 
        \frac{1}{\kappa \, \beta} 
        \int | \nabla f |^2 \, d\nu \,, 
$$

for all $$f \in C^1(\mathbb{R}^d) \cap L^2(\nu)$$. 
Note we adopt the convention of \[BGL13\] which adjusts 
the right hand side by a factor of $$\beta$$, 
and the two conventions agree when $$\beta = 1$$. 

$$\text{PI}(\kappa)$$ is well known to be equivalent to 
exponential convergence of Langevin diffusion 
\[BGL13, Theorem 4.2.5\], 
quadratic-linear cost transport inequality \[Vil08, Theorem 22.25\], 
and Cheeger's isoperimetric inequality \[LV18, Theorem 11\]. 
Furthermore, $$\text{PI}(\kappa)$$ also 
implies dimension free exponential concentration \[Vil08, Theorem 22.32\], 
and serves as a key tool for deriving existence, uniqueness, and smoothness 
results in partial differential equations \[Eva10\]. 
Therefore a tight lower bound for the Poincar&eacute; constant 
is widely desired for a large range of applications. 




### Interpreting the Poincar&eacute; Constant

Firstly, we will recall the (overdamped) Langevin diffusion 
is defined by the following stochastic differential equation (SDE) 

$$ dX_t = \underbrace{ - \nabla F(X_t) \, dt }_{ \text{gradient flow}_{} } 
    + \underbrace{ \sqrt{ 2/\beta } \, dW_t 
        }_{ \text{perturbation}_{} }\,, 
$$

where $$\{W_t\}_{ t_{} \geq 0 }$$ is a standard $$d$$-dimensional 
Brownian motion. 
Observe that when $$\beta$$ becomes large, 
the Brownian motion term becomes very small. 
Therefore Langevin diffusion can be interpreted as 
a perturbed gradient flow. 

Since the Gibbs density $$\nu$$ finds the global minimum 
of $$F$$ as $$\beta \to \infty$$, 
a dimension and temperature free Poincar&eacute; constant 
implies a fast convergence of Langevin diffusion to the global minimum 
when $$\beta$$ is large. 
Therefore it is no surprise that 
1. a strongly convex $$F$$ implies $$\nu$$ has 
a dimension and temperature free Poincar&eacute; constant, 
more famously known as the Bakry-&Eacute;mery criterion 
\[BGL13, Proposition 4.8.1\]; 
2. a non-convex $$F$$ with multiple isolated local minima leads to 
a Poincar&eacute; with exponentially poor dependence on $$\beta$$, 
more famously known as the Eyring-Kramers formula \[Ber11\]. 

In other words, strongly convex functions are easy to optimize, 
general non-convex functions are hard, what else is new? 
**What is new** are the cases in between: 
non-strongly convex functions with a unique minimum. 
<!--  -->
However, even when weakening to $$F$$ being only convex, 
this problem remains open - 
this is an equivalent formulation of the KLS conjecture \[LV18\]. 

---

**Conjecture (Kannan-Lov&#225;sz-Simonovits, Poincar&eacute; version)** 
There exists a universal constant $$\kappa > 0$$, 
such that for all positive integer $$d$$, 
and all convex function $$F: \mathbb{R}^d \to \mathbb{R}$$ 
such that the Gibbs density $$\nu(x) = \frac{1}{Z} e^{-F(x)}$$ 
(note $$\beta = 1$$ here) 
has zero mean and identity covariance matrix, 
we have that $$\nu$$ satisfies $$\text{PI}(\kappa)$$. 

---

I should briefly mention that a recent arXiv preprint \[Che20\]
proposed a result equivalent to an almost constant lower bound 
on the Poincar&eacute; constant of order $$O( d^{-o(1)} )$$, 
which in the limit of $$d\to\infty$$ converges to $$0$$ 
slower than $$d^{-r}$$ for all $$r>0$$. 
For the sake of staying on topic, 
we will leave this extremely interesting subject for a future post. 
<!-- As of the writing of this post, 
\[Che20\] still awaits confirmation by the community. 
An earlier draft of \[LV16\] that claimed a similar result, 
and it was eventually corrected to a weaker 
(but still the best known) lower bound. 
Regardless, we will not dive deeper in this direction. 
 -->

Furthermore, it is already known that 
an adaptive perturbation of gradient descent 
escapes saddle points at a dimension free rate \[JGN+17\]. 
This hints at the possibility of establishing 
a dimension and temperature free Poincar&eacute; inequality 
for even non-convex potential functions! 
Indeed, we will discuss this next. 










## Non-Convex Poincar&eacute; 

The main result of this blog post is actually 
an intermediate result of \[LE20, Proposition 9.11\]. 
While the original proposition is proved for 
a product manifold of spheres, 
it can be easily adapted to $$\mathbb{R}^d$$ 
with a containment type condition, 
see for example \[Vil06, Theorem A.1\] 
and \[MS14, Assumption 1.4\]. 
Since as of writing this post, 
there is no complete proof of this adaptation, 
we will state it as a claim. 

---

**Claim (Adapting [LE20, Proposition 9.11])** 
Suppose $$F:\mathbb{R}^d \to \mathbb{R}$$ 
have a unique local (and therefore global) minimum, 
and all saddle points are strict, 
i.e. the minimum eigenvalue 
$$\lambda_{\text{min}}( \nabla^2 F ) < - \lambda$$ 
for some constant $$\lambda>0$$ at saddle points. 
Then under appropriate containment conditions, 
and choosing $$\beta$$ sufficiently large, 
we have that $$\nu(x) = \frac{1}{Z} e^{-\beta F(x)}$$ 
satisfies $$\text{PI}(\kappa)$$ 
for a constant $$\kappa>0$$ independent of $$\beta, d$$. 

---

As we discussed earlier, 
perturbation helps gradient descent escape saddle points, 
therefore this result is very intuitive. 
However, deriving a quantitative bound is completely non-trivial. 
We remark that for non-convex potentials, 
most approaches to establishing a Poincar&eacute; inequality 
will yield exponentially poor dependence on both $$\beta$$ and $$d$$. 
To our best knowledge, only \[MS14\] has established a bound 
independent of $$\beta$$, and (by our calculations) exponential in $$d$$. 

<!-- Of course, there is the condition on $$\beta$$ sufficiently large, 
but we have good intuitions (later in this post) to believe 
this is a rather technical constraint. 
More precisely, we speculate that with a sharpened analysis, 
we can show that for all $$\beta \geq 1$$, 
$$\nu(x)$$ satisfies $$\text{PI}(\kappa)$$ 
with $$\kappa$$ independent of $$\beta, d$$!   -->

### Implications 

Let us start by emphasizing this result **does not** 
imply the KLS conjecture. 
Indeed, the conditions of the conjecture does not 
require $$F$$ to have a unique minimum. 
Hence $$F$$ needs not to be strongly convex around the minimum. 
This is a key requirement of the proof technique. 

It does, however, imply that the KLS conjecture 
can be extended beyond convex functions. 
More precisely, if a potential $$F$$ satisfies 
a dimension and temperature free Poincar&eacute; inequality, 
we can add saddle points to $$F$$ without losing this property. 
<!--  -->
Therefore, we can replace convex potentials $$F$$ in the statement 
of the KLS conjecture with modifications of convex potentials $$F$$ 
with strict saddle points. 
In fact, this naturally leads us to further conjecture 
the strictness of saddle points can be relaxed as well, 
since it's the parallel of relaxing strong convexity to convexity 
for saddle points. 

Additionally, notice that we can take $$\beta$$ 
to be as large as we want. 
This implies that the amount of randomness added to gradient flow 
does not affect its ability to escape saddle points. 
In other words, any tiny amount of perturbation will 
help escape saddle points. 
Furthermore, we emphasize this implies 
**a discretization of Langevin diffusion**, 
i.e. a perturbed gradient descent, 
will also escape strict saddle points - 
this was the main result of \[LE20\]. 
This is in sharp contrast with (deterministic) gradient descent 
taking up to exponential time to escape a saddle point \[DJL+17\], 
implying **the addition of noise, even arbitrarily small, 
fundamentally changes the behaviour of gradient descent**. 

<!-- We also note the continuous time result is not new, 
as there have been asymptotic analysis of saddle point escape time 
in the limit of $$\beta \to \infty$$ \[Bak08\], 
this is the first non-asymptotic characterization to our best knowledge. 
 -->





## Proof Sketch - Step 1: Lyapunov Function Away From Saddle Points 

Now to my favourite part of this post, 
where we actually describe the proof techniques. 
We will see that despite the lengthy calculations in \[LE20\], 
the proof idea is quite straight forward to explain. 
We start by stating a Lyapunov criterion for 
the Poincar&eacute; inequality. 

---

**Theorem \[BBCG08, Theorem 1.4 Adapted\]** 
Let $$U \subset \mathbb{R}^d$$ be such that 
$$\nu$$ restricted to $$U$$ satisfies $$\text{PI}(\kappa_U)$$. 
Suppose there exist constants $$\theta > 0, b \geq 0$$ 
and a function $$V \in C^2(\mathbb{R}^d)$$ such that 
$$V \geq 1$$ and 

$$ LV := \langle -\nabla F, \nabla V \rangle 
    + \frac{1}{\beta} \Delta V 
    \leq 
    -\theta \, V + b {1}_{U_{}} \,, 
$$

Then $$\nu$$ satisfies $$\text{PI}(\kappa)$$ with constant 

$$ \kappa = \frac{ \theta }{ 1 + b / \kappa_U} \,. 
$$

---

Intuitively, we can think of the Lyapunov function $$V$$ 
as an energy measure of the Langevin diffusion $$\{X_t\}_{t_{} \geq 0}$$, 
$$LV$$ as the time evolution of $$V$$ via 
[It&ocirc;'s Lemma](https://en.wikipedia.org/wiki/It%C3%B4%27s_lemma), 
and the Lyapunov condition (inequality) 
describes the rate of energy dissipation over time. 
<!--  -->
This energy $$V$$ will decrease as $$X_t$$ gets closer to $$U$$, 
hence $$U$$ behaves like an attractor. 
Once $$X_t$$ reaches $$U$$, the process begins to "mix" 
due to the Poincar&eacute; inequality on $$U$$. 
<!--  -->
In our case, we will choose $$U$$ to be a small neighbourhood 
of the global minimum, 
and use the strong convexity (Bakry-&Eacute;mery criterion) 
to get a Poincar&eacute; constant $$\kappa_U$$. 
<!--  -->
For those that find this description familiar, 
indeed this is the diffusion equivalent of the drift 
and minorization conditions for Markov chain mixing \[MT09\]. 

Similar to other Lyapunov function based methods 
in differential equations, 
constructing such a function $$V$$ is the main difficulty. 
\[MS14\] observed that when $$F$$ has only strict saddle points, 
the choice of $$V = \exp\left( \frac{\beta}{2} F \right)$$ 
works very nicely away from saddle points. 
In fact, we can directly compute the Lyapunov condition 

$$ \frac{LV}{V} = \frac{1}{2} \Delta F - \frac{\beta}{4} |\nabla F|^2 \,, 
$$

and observe that as long as $$|\nabla F|$$ is bounded away from zero, 
we can choose $$\beta$$ to be large, 
hence forcing $$\frac{LV}{V}$$ to be negative. 
In other words, excluding small neighbourhoods around saddle points, 
$$\nu$$ satisfies a Poincar&eacute; inequality. 

The precise version of this result can be found in 
\[LE20, Lemma 9.10\]. 
In particular, we can compute this constant 
to be dimension and temperature free. 













## Proof Sketch - Step 2: An Escape Time Construction 

Now we have narrowed the problem down only constructing 
a Lyapunov function for the neighbourhoods around saddle points, 
we will observe replacing the inequality of the Lyapunov condition 
with equality gives us the Poisson equation. 
Consequently, we have a stochastic representation of 
the solution in terms of the escape time. 

---

**Theorem \[BdH16, Theorem 7.15 Adapted\] \[LE20, Corollary 9.3\]** 
Let $$B \subset \mathbb{R}^d$$, $$\{X_t\}_{t_{}\geq 0}$$ 
be the Langevin diffusion, 
and $$\tau_{B^c}$$ be the first escape time of $$X_t$$ from $$B$$. 
<!--  -->
Suppose there exists a constant $$\theta>0$$ such that 

$$ V(x) 
    := \mathbb{E} [ \, \exp( \theta \, \tau_{B^c} ) \, 
        | \, X_0 = x \, ] 
    < \infty \,, \quad \forall x \in B \,, 
$$

then $$V$$ is the unique solution to the Poisson equation 

$$
\begin{split}
    LV &= - \theta \, V \,, \quad & x \in B \,, \\ 
    V &= 1 \,, \quad & x \in \partial B \,. 
\end{split}
$$

---

Readers with a Markov chain background may recognize 
this escape time based condition to be equivalent to 
drift and minorization \[DMPS18, Theorem 14.1.3\]. 
In fact, this method was inspired by the nice connection 
drawn between diffusions and Markov chains. 

Additionally, readers familiar with concentration inequalities 
may recognize the theorem's condition is known as 
exponential integrability, 
and it's one of the equivalent characterizations 
for sub-exponential random variables. 
Indeed, we will actually use a slightly easier equivalent form 
for calculations. 

---

**Theorem \[Wai19, Theorem 2.13\]**
For a zero mean random variable $$\tau$$, the following are equivalent: 
1. there exists a constant $$\theta > 0$$ such that 
$$\mathbb{E} \, e^{ \theta \tau } < \infty $$, 
2. there exists constants $$c,\theta > 0$$ such that 
$$ \mathbb{P} [ \, |\tau| > t \, ] \leq c e^{ -2\theta t } $$ 
for all $$t \geq 0$$. 

---

At this point, it's then sufficient to establish 
an exponentially decaying tail bound for $$\tau_{B^c}$$. 
To this goal, we will make several observations: 
1. When $$F$$ is sufficiently smooth, 
$$F$$ can be locally approximated by a quadratic function. 
2. We only need to consider an escape via 
a direction corresponding to a negative eigenvalue 
of $$\nabla^2 F$$. 
3. When restricted within this direction only, 
$$F$$ near a saddle point can be viewed as a local maximum. 

To illustrate this point clearly, 
let us consider the quadratic function $$f(x,y) = x^2 - \frac{\lambda}{2} y^2$$ 
with a saddle point at $$(x,y) = (0,0)$$. 
For the Langevin diffusion to escape a neighbourhood of radius $$r>0$$, 
it's sufficient to ensure the $$y$$-component exceeds $$r>0$$. 
Therefore, it's sufficient to restrict $$f$$ to only its $$y$$-component, 
which makes $$y=0$$ a local maximum. 
Therefore it's sufficient to study the Langevin diffusion restricted 
to escaping an one dimension local maximum, i.e. 

$$ dX_t = \lambda X_t \, dt + \sqrt{ 2/\beta } \, dW_t \,, 
$$

where $$-\lambda$$ upper bounds the smallest eigenvalue of 
$$\nabla^2 F$$ at saddle points. 

We observe that this SDE is the "negative" Ornstein-Uhlenbeck process, 
and it has a closed form solution 

$$ X_t = X_0 e^{\lambda t} 
    + \sqrt{2/\beta} \, \int_0^t e^{\lambda(t-s)} dW_s \,, 
$$

which corresponds to 
$$X_t \sim N( X_0 e^{\lambda t} \,, 
\frac{1}{\lambda \beta}(e^{2\lambda t} - 1) )$$. 
Finally, plugging in the Gaussian density 
and a few calculations later, 
we get the desired result of 

$$ \mathbb{P} [ \, |\tau_{B^c}| \geq t \, ] 
    \leq \mathbb{P} [ \, X_t \in B \, ] 
    \leq c e^{ -\lambda t } \,, 
$$

where the constant $$c$$ does not depend on $$t$$. 
I.e. this escape time tail bound implies that 
$$V(x)$$ is a valid Lyapunov function, 
and hence implies a Poincar&eacute; inequality. 
















## Final Thoughts

Quite a few technical details were swept under the rug 
to simplify the proof sketch, as the reader might expect. 
Probably the most significant is the approximation 
of $$F$$ by a quadratic function - 
it is actually not very straight forward to connect 
an approximation bound to an escape time bound. 

At the same time, the requirement of $$\beta$$ to be 
sufficiently large is quite unsatisfying. 
Intuitively, why would adding noise hurt the mixing of 
a Markov process? 
It feels to me that this condition is merely a technical constraint, 
and a more careful analysis could sharpen or remove this condition. 
Hopefully the readers will have more thoughts and ideas than I do. 

Thanks to reading up this point, 
and I wish everyone a happy new year! 











## References 

<!-- - \[Bak08\] Y. Bakhtin, "Exit asymptotics for small diffusion about an unstable equilibrium." Stochastic processes and their applications 118.5 (2008): 839-851.  -->
- \[BBCG08\] Dominique Bakry, Franck Barthe, Patrick Cattiaux, and Arnaud Guillin, A simple proof of the poincar ́e inequality for a large class of probability measures, Electronic Communications in Probability 13 (2008), 60–66.
- \[Ber11\] N. Berglund, Kramers’ Law: Validity, Derivations and Generalisations. arXiv preprint arXiv:1106.5799 (2011).
- \[BGL13\] D. Bakry, I. Gentil, and M. LeDoux, Analysis and Geometry of Markov Diffusion Operators, Springer (2013). 
- \[Che20\] Y. Chen, An Almost Constant Lower Bound of the Isoperimetric Coefficient in the KLS Conjecture, arXiv preprint arXiv:2011.13661 (2020). 
- \[DJL+17\] S. S. Du, C. Jin, J. D. Lee, M. I. Jordan, A. Singh, B. Poczos, Gradient descent can take exponential time to escape saddle points. In Advances in neural information processing systems, pp. 1067-1077 (2017).
- \[DMPS18\] Randal Douc, Eric Moulines, Pierre Priouret, and Philippe Soulier, Markov chains, Springer, 2018.
- \[Eva10\] L.C. Evans, Partial differential equations, Graduate studies in mathematics, American Mathematical Society (2010).
- \[JGN+17\] C. Jin, R. Ge, P. Netrapalli, S. M. Kakade, & M.I. Jordan, How to escape saddle points efficiently, arXiv preprint arXiv:1703.00887 (2017). 
- \[LE20\] M.B. Li, M.A. Erdogdu, Riemannian Langevin Algorithm for Solving Semidefinite Programs, arXiv preprint arXiv:2010.11176 (2020). 
<!-- - \[LV16\] Y.T. Lee, and S.S. Vempala, Eldan's Stochastic Localization and the KLS Conjecture: Isoperimetry, Concentration and Mixing, arXiv preprint arXiv:1612.01507 (2016).  -->
- \[LV18\] Y.T. Lee, and S.S. Vempala, The Kannan-Lov&#225;sz-Simonovits Conjecture, arXiv preprint arXiv:1807.03465 (2018). 
- \[MS14\] Georg Menz and Andr&eacute; Schlichting, Poincar&eacute; and Logarithmic Sobolev Inequalities by Decomposition of the Energy Landscape, The Annals of Probability 42 (2014), no. 5, 1809–1884. 
- \[MT09\] Sean Meyn and Richard L. Tweedie, Markov chains and stochastic stability, 2nd ed., Cambridge University Press, USA, 2009.
- \[Vil06\] C. Villani, Hypocoercivity, arXiv preprint math/0609050 (2006). 
- \[Vil08\] C. Villani, Optimal Transport: Old and New, Grundlehren der Mathematischen Wissenschaften, Springer Berlin Heidelberg (2008).
- \[Wai19\] Martin J Wainwright, High-dimensional statistics: A non-asymptotic viewpoint, vol. 48, Cambridge University Press, 2019.










