---
title: Connected by Poincar&eacute; Inequality
---

While studying two seemingly irrelevant subjects, 
probability theory and partial differential equations (PDEs),
I ran into a somewhat surprising overlap:
the [Poincar&eacute; inequality](https://en.wikipedia.org/wiki/Poincar%C3%A9_inequality).
On one hand, it is not out of the ordinary 
for analysis based subjects to share inequalities 
such as [Cauchy-Schwarz](https://en.wikipedia.org/wiki/Cauchy%E2%80%93Schwarz_inequality) 
and [H&ouml;lder](https://en.wikipedia.org/wiki/H%C3%B6lder%27s_inequality);
on the other hand, the two forms of
Poincar&eacute; inequality have quite different applications.

In this blog post, I hope to put together some excellent content
I studied recently, specifically from:
 - *Concentration Inequalities* by Boucheron, Lugosi, and Massart (2013) 
 - *Partial Differential Equations* by Evans (2010)
<!-- - *Functional Analysis, Sobolev Spaces, and Partial Differential Equations*
by Haim Brezis (2011) -->

<!-- Both books are great references, I would strongly recommend both 
for an intuitive yet rigorous read in their respective topics. -->

## A Simple Inequality 

We first state a very simple version of the inequality:

---

**Theorem (A Simple Poincar&eacute; Inequality)**
Let $$\Omega \subset \mathbb{R}^n$$ be open and bounded, 
and let $$f \in C^1_c(\Omega)$$ (differentiable
with compact support).
Then there exists a constant $$C$$ that depends only 
on $$\Omega$$ such that:

\\[ \left\lVert f \right\rVert_{L^2(\Omega)} 
    \leq C \lVert \nabla f \rVert_{L^2(\Omega)}
\\]

<!-- where $$\overline{f}$$ is the average of the function 
$$f$$ in domain $$\Omega$$. -->

---

Quick aside: we say a function $$f$$ has **compact support**
if the set $$ S = \{ x \in \Omega : f(x) \neq 0 \} $$
has compact closure. 
This implies $$f(x) = 0$$ near the boundary.

Observe that the inequality simply bounds 
the $$L^2$$-norm of a function in terms of the $$L^2$$-norm
of its gradient instead.
Note the compact support here is an important assumption 
when we are integrating with respect to the Lebesgue measure.
Consider for example a constant function, 
then this inequality would fail as the gradient is zero.
The reader may be comforted that a general form will require 
much fewer assumptions, and can be generalized to 
all $$L^p$$ norms.

The reason we start with this inequality 
is because the proof is quite straightforward:

*proof (of the Simple Poincar&eacute; Inequality):*

Without loss of generality, 
we let $$\Omega \subset [0,M]^n$$
for some large $$M > 0$$, 
and by the Cauchy-Schwarz inequality we have

\\[ \vert f(x) \vert^2 \leq \left\vert \int_0^{x_1} 
    \frac{\partial }{\partial x_i} f(y_1, x_2, \ldots) dy_1 \right\vert^2
    \leq \left[ \int_0^M 1^2 dy_1 \right]
        \left[ \int_0^M \left\vert \frac{\partial f}{\partial x_1} \right\vert^2 dy_1 \right]
 \\]

 Summing over all $$n$$ possible derivatives,
 and integrating over $$\Omega$$ we have 

 \\[ n \int_\Omega \vert f(x) \vert^2 
    \leq \int_\Omega \sum_{i=1}^n M \int_0^M \left\vert 
        \frac{\partial f}{\partial x_i} \right\vert^2 dy_i
    = \sum_{i=1}^n M \left\lVert \frac{\partial f}{\partial x_i}
    \right\rVert^2_{L^2(\Omega)}
 \\]

 where in the last step we exchanged the order of integration,
 and used the fact that the $$L^2$$ norm is a constant. 
 Rewriting the above we get the desired result

 \\[ \lVert f \rVert_{L^2(\Omega)}
    \leq \frac{M}{\sqrt{n}} \lVert \nabla f \rVert_{L^2(\Omega)}
 \\]

$$\tag*{$\Box$}$$

## As a Concentration Inequality

We now state the inequality in a form 
most useful for probability theory, see
Theorem 3.20 from Boucheron, Lugosi, Massart (2013):

---

**Theorem (Gaussian-Poincar&eacute; Inequality)**
Let $$X = (X_1, \ldots, X_n)$$ be a vector of i.i.d. 
standard Gaussian random variables. 
Let $$\, f : \mathbb{R}^n \to \mathbb{R} $$
be any continuously differentiable function. 
Then

\\[ \text{Var}[f(X)] \leq 
    \mathbb{E}\left[ \| \nabla f(X)\|^2 \right] \\]

---

Observe that the inequality is slightly different.
Firstly this time the norm is centered, 
although centering in this case is not an issue 
since $$Var[X] \leq \mathbb{E}X^2$$.
Secondly due to the measure being a probability measure,
we have a much smaller constant on the inequality $$C=1$$. 
In combination, we were also able to drop 
the compact support assumption.

An immediate consequence is to consider 
$$f$$ Lipschitz with coefficient $$1$$, 
i.e. $$ | f(x) - f(y) | \leq \|x - y\|$$,
then we have 

\\[ \text{Var}[f(X)] \leq 1 \\]

In other words, we just found a constant bound
on the variance for a huge class of random functions!
In general, we can consider $$f$$ to be a smooth estimator 
based on a dataset with noise $$X$$. 
The Poincar&eacute; inequality will provide 
a very useful bound on estimation error.
<!-- If this doesn't excite you, I don't know what does :) -->

To prove this inequality, we will use a famous 
result from 1981 (Theorem 3.1 in 
Boucheron, Lugosi, Massart (2013)):

---

**Theorem (Efron-Stein Inequality)**
Let $$X = (X_1, \ldots, X_n)$$ be a vector of i.i.d. 
random variables and let $$Z = f(X)$$ be a square-integrable 
function of $$X$$. 
Then

\\[ \text{Var}(Z) \leq \sum_{i=1}^n \mathbb{E} 
    \left[ \left( Z - \mathbb{E}^{(i)}Z \right)^2 \right] \\]

where $$\mathbb{E}^{(i)}Z = \int 
    f(X_1, \ldots, X_{i-1}, x_i, X_{i+1},\ldots) d\mu_i(x_i) $$,
i.e. the expectation over $$X_i$$ only.

---

The Efron-Stein inequality can be proved by decomposing 
the variance as a sum of telescoping differences of 
conditional expectations, 
and applying Jensen's inequality to the individual terms.
While we omit the proof here, 
we should remark that the simple Efron-Stein inequality 
has wide ranging applications;
we will only look at one such use for the proof 
of the Poincar&eacute; inequality, 
taken from Theorem 3.20 in Boucheron, Lugosi, Massart (2013):

*proof (of Gaussian-Poincar&eacute; Inequality):*

First we observe that a direct application of 
the Efron-Stein inequality can reduce the problem down 
to $$n=1$$, i.e. it is sufficient to show 

\\[ \mathbb{E}^{(i)} \left[ \left( Z - \mathbb{E}^{(i)}Z \right)^2 \right]
    \leq \mathbb{E}^{(i)} \frac{\partial f}{\partial x_i}(X)^2 \\]

From here we assume without loss of generality $$n=1$$.
Then we notice that it is sufficient to prove 
this inequality for compactly supported, twice differentiable
functions, i.e. $$f \in C_c^2(\mathbb{R})$$, 
since otherwise we can just take a limit to the original function.

Here we let $$\epsilon_1,\ldots,\epsilon_n$$ be i.i.d. 
Rademacher random variables, i.e. 
$$\mathbb{P}[\epsilon_j = 1] = \mathbb{P}[\epsilon_j = -1] = 
    \frac{1}{2} \,\forall j \in \{ 1,2,\ldots,n \} $$,
and we define

\\[ S_n = n^{-1/2} \sum_{j=1}^n \epsilon_j \\]

Observe that for every $$i$$ we have 

\\[ \text{Var}^{(i)}[f(S_n)] = 
    \frac{1}{4} \left[ f\left( S_n + \frac{1-\epsilon_i}{\sqrt{n}} \right)
     - f\left( S_n + \frac{1+\epsilon_i}{\sqrt{n}} \right) \right]^2 \\]

Applying the Efron-Stein inequality, we get 

\\[ \text{Var}[f(S_n)] \leq \frac{1}{4} \sum_{i=1}^n 
    \mathbb{E} \left[ \left(
        f\left( S_n + \frac{1-\epsilon_i}{\sqrt{n}} \right)
        - f\left( S_n + \frac{1+\epsilon_i}{\sqrt{n}} \right)
    \right)^2 \right] \\]

Let $$ K = \sup_x \vert f''(x) \vert $$, then we have that 

\\[ \left|f\left( S_n + \frac{1-\epsilon_i}{\sqrt{n}} \right)
        - f\left( S_n + \frac{1+\epsilon_i}{\sqrt{n}} \right)\right|
    \leq \frac{2}{\sqrt{n}} |f'(S_n)| + \frac{2K}{n}
\\]

which implies 

\\[ \frac{n}{4} \left(
        f\left( S_n + \frac{1-\epsilon_i}{\sqrt{n}} \right)
        - f\left( S_n + \frac{1+\epsilon_i}{\sqrt{n}} \right)
    \right)^2
    \leq f'(S_n)^2 + \frac{2K}{\sqrt{n}} | f'(S_n) |
        + \frac{K^2}{n}
\\]

Finally the central limit theorem then imply the desired result

\\[ \limsup_{n\to\infty} \frac{1}{4} \sum_{i=1}^n
    \mathbb{E}
    \left[ \left(
        f\left( S_n + \frac{1-\epsilon_i}{\sqrt{n}} \right)
        - f\left( S_n + \frac{1+\epsilon_i}{\sqrt{n}} \right)
    \right)^2 \right]
    = \mathbb{E} \left[ f'(X)^2 \right]
\\]

$$\tag*{$\Box$}$$

**Remark** 
There are also Poincar&eacute; type inequalities for 
non-Gaussian random variables, 
for example if $$X\sim$$Poisson$$(\mu)$$:

\\[ \text{Var}[f(X)] \leq \mu \mathbb{E}\left[
    (f(X+1) - f(X))^2 \right]
\\]

Or if $$X$$ is double exponential 
i.e. with density $$\frac{1}{2}e^{-\vert x \vert}$$,
then we have:

\\[ \text{Var}[f(X)] \leq 4 \mathbb{E}\left[
    (f'(X))^2 \right]
\\]

## An Application to PDEs

To do proper justice for the theory of PDEs, 
we will need a significant background in functional analysis. 
In this section, we will try to side-step the technical details 
and focus on one single application,
that is showing the existence and uniqueness of 
a weak solution for **Poisson's equation**:

\\[ -\Delta u = f \; \text{ in } \Omega \\]
\\[ u = g   \; \text{ in } \partial \Omega \\]

where $$\Omega \subset \mathbb{R}^n$$ is open bounded with 
smooth boundaries $$\partial \Omega$$.
By **weak solution**, we meant there exists a $$u \in C^1(\Omega)$$
such that $$\forall v \in C^1_c(\Omega)$$ we have

\\[ B[u,v] := \int_\Omega \nabla u \cdot \nabla v = \int_\Omega f v
\\]

Note if $$u$$ is a solution to the (original) Poisson's equation, 
then we have the above weak equation by 
[Green's identity](https://en.wikipedia.org/wiki/Green%27s_identities). 
The main tool we will use to prove existence and uniqueness is 
the following result:

---

**Theorem (Lax-Milgram)**
Let $$H$$ be a Hilbert space, $$B: H^2 \to \mathbb{R}$$
be a continuous, coersive, bilinear form. 
Then $$\forall \varphi \in H^*$$, 
there exists a unique $$u\in H$$ such that

\\[ B(u,v) = <\varphi, v>
    \quad \forall v \in H
\\]

where $$<\varphi,v>$$ is the linear functional
$$\varphi$$ applied to $$v$$.

---

**Remark** Before going into the definitions and technical details, 
we observe that Lax-Milgram Theorem gives us exactly 
what we want - the existence and uniqueness!
Now we just have to fill in the blanks:
 - define a [Hilbert space](https://en.wikipedia.org/wiki/Hilbert_space) 
 $$H$$ where the solution lives in 
 - show that $$B(u,v)$$ is continuous and coersive (we will define later)
 - let $$<\varphi, v> = \int_\Omega f v$$

**Step 1**
To define our Hilbert space,
we will consider the following 
[inner product](https://en.wikipedia.org/wiki/Inner_product_space):

\\[ (u,v) := \int_\Omega [uv + \nabla u \cdot \nabla v] \\]

which corresponds to the following **Sobolev norm**:

\\[ \lVert u \rVert_{H^{1}(\Omega)}
    := (u,u)^{1/2}
    = \left[ \lVert u \rVert_{L^2(\Omega)}^2 + 
        \lVert \nabla u \rVert_{L^2(\Omega)}^2 \right]^{1/2}
\\]

By equipping the space $$C^1_c(\Omega)$$
with the above inner product, 
we almost have a Hilbert space!
Here we will simply take the completion of $$C^1_c(\Omega)$$
with respect to the Sobolev norm, 
i.e. add all the limit points to the space.
We call this (completed) Hilbert space $$H_0^1(\Omega)$$.

<!-- At the same time, we should note the form on the right hand side 
is just a geometric mean of two quantities related 
by the Poincar&eacute; inequality! 
This observation will be key to our proof.  -->

**Step 2**
We now turn our attention to 
$$B(u,v)$$, the bilinear form 
(fancy term for separately linear in both inputs).
Then we say $$B$$ is **continuous** if 

\\[ \exists C_1 > 0 : \forall u,v \in H, 
    \vert B(u,v) \vert 
        \leq C_1 \lVert u \rVert_{H^1(\Omega)} 
            \lVert v \rVert_{H^1(\Omega)}
\\]

Note this is an immediate consequence of Cauchy-Schwarz inequality 

\\[ \vert B(u,v) \vert \leq \lVert \nabla u \rVert_{L^2(\Omega)}
    \lVert \nabla v \rVert_{L^2(\Omega)}
    \leq \lVert u \rVert_{H^1(\Omega)} \lVert v \rVert_{H^1(\Omega)}
\\]

We say $$B$$ is **coersive** if 

\\[ \exists C_2 > 0 : \forall u \in H, 
    B(u,u) \geq C_2 \lVert u \rVert_{H^1(\Omega)}^2
\\]

We notice this is the only non-trivial condition left to check, 
and to prove this we will finally use *Poincar&eacute; inequality*! 
Start by rewriting 

\\[ B(u,u) = \int_\Omega \nabla u \cdot \nabla u
    = \lVert \nabla u \rVert_{L^2(\Omega)}^2 \\]

Applying the Poincar&eacute; inequality on half of the norm 
we have

\\[ \frac{1}{2} \lVert \nabla u \rVert_{L^2(\Omega)}^2 
    \geq \frac{1}{2C} \lVert u \rVert_{L^2(\Omega)}^2 
\\]

Therefore 

\\[ B(u,u) \geq \frac{1}{2} \lVert \nabla u \rVert_{L^2(\Omega)}^2 
    + \frac{1}{2C} \lVert u \rVert_{L^2(\Omega)}^2 
    \geq \min\left(\frac{1}{2}, \frac{1}{2C}\right) 
        \lVert u \rVert_{H^1(\Omega)}^2 
\\]

And voil&agrave;, we have existence and uniqueness!
A rigorous and careful reader may notice that $$u$$
does not necessarily have compact support -
this is correct. 
However every $$u \in H_0^1(\Omega)$$ 
is a limit of compactly supported functions, 
therefore we just need to take a limit to get our result!

**Remark** In fact, we can use similar Lax-Milgram 
based methods to show existence and uniqueness 
for a large subset of 
[elliptical PDEs](https://en.wikipedia.org/wiki/Elliptic_partial_differential_equation).
We should note that 
the fact we can "convert" between $$\|u\|$$ and $$\|\nabla u\|$$
is highly useful for studying Sobolev norms.
We refer curious readers to Evans (2010)
for an excellent chapter on 
[Sobolev spaces](https://en.wikipedia.org/wiki/Sobolev_space)
and related inequalities.

## Final Words

I have a weak spot for connections between different fields, 
probably because it's always surprising, 
and surprises are intriguing in math! 
I hope to have to presented a readable introduction 
to the inequality and its applications in both topics, 
without drowning readers in technical details. 
On this note, I should remark that to study Sobolev spaces rigorously,
the reader will need to go through all the details carefully!

As this is my first blog post, any constructive feedback or suggestions on 
future topics will be appreciated!

## References 

 - Boucheron, S., Lugosi, G., & Massart, P. (2013). Concentration inequalities: A nonasymptotic theory of independence. Oxford university press.
 - Evans, L. C. (2010). Partial differential equations. Providence, R.I.: American Mathematical Society.


<!-- This nice inequality is named after the brilliant
[Henri Poincar&eacute;](https://en.wikipedia.org/wiki/Henri_Poincar%C3%A9),
who contributed much more to mathematics than a blog post can contain.  -->


<!-- Observe that these inequalities now have 
*different constant coefficients*!
As we will see soon, the more general PDE version of Poincar&eacute; 
inequality does in fact have a constant coefficient. -->

<!-- ## Some Background in Sobolev Spaces

I will introduce a few technical definitions 
so I can state the inequality rigorously.
Note these technicalities are only necessary for 
applications in PDEs.

Let $$\Omega \subset \mathbb{R}^n$$ be bounded
with a smooth ($$C^1$$) boundary, 
and let $$u,v \in L^1_{\text{loc}}(\Omega)$$ 
(i.e. integrable on compact subsets). 
We say $$u$$ has a **weak derivative** $$v$$ 
with respect to $$x_i$$ if $$\forall \varphi \in C^\infty_c(\Omega)$$
(compact support) we have

\\[ \int_\Omega 
    u \frac{\partial \varphi}{\partial x_i}
    = \int_\Omega v \varphi \\]

Then if $$u$$ has all weak derivatives of degree 1, 
we can define a $$L^p$$ like norm as follows

\\[ \lVert u \rVert_{W^{1,p}(\Omega)} := \left[ 
    \lVert u \rVert_{L^p(\Omega)}^p 
    + \sum_{i=1}^n 
      \left\lVert \frac{\partial u}{\partial x_i} 
        \right\rVert_{L^p(\Omega)}^p \right]^{1/p}
\\]

The above **Sobolev norm** can be interpreted as a "sum" of 
all the weak derivatives' $$L^p$$ norms.
We then define the normed-space of such functions 
a **Sobolev space**,
denoted $$W^{1,p}(\Omega)$$.
Note we can extend the definition up to the $$k^\text{th}$$
degree weak derivative, which is denoted $$W^{k,p}$$ instead.

An important space we will look at is the $$W^{1,p}$$-closure 
of $$C^\infty_c(\Omega)$$, denoted $$W^{1,p}_0(\Omega)$$.
In other words, all the limit points of $$C^\infty_c(\Omega)$$ 
in the norm we defined above.

## In the Context of Sobolev Spaces

Finally we can state the second form of 
Poincar&eacute; inequality, 
(quoting Corollary 9.19 from Brezis (2011)):

---

**Theorem (Poincar&eacute; Inequality)**
Suppose $$1 \leq p < \infty$$, $$\Omega$$ bounded open.
Then there exists a constant $$C$$ depending on 
$$\Omega, p$$ such that:

\\[ \lVert u \rVert_{L^p(\Omega)}
    \leq C \lVert \nabla u \rVert_{L^p(\Omega)}
    \quad \forall u \in W^{1,p}_0(\Omega)
\\]

---

**Remark** To connect with the previous inequality, 
we need to consider squaring both sides, 
taking $$p=2$$, and change the Lebesgue measure 
to a probability measure 
(note the Radon-Nikodym derivative is just the density, 
which is bounded for Gaussian).
Lastly, there is in fact a version of this inequality 
with the left hand side centered - 
i.e. $$\|u - \overline{u}\|_{L^p(\Omega)}$$
where $$\overline{u}$$ is the average. We will discuss this at the end.

The following proof is based on Proposition 9.18 from Brezis (2011):

*proof (of Poincar&eacute;'s Inequality)*

Firstly since $$u \in W^{1,p}_0(\Omega)$$,
there exists a sequence 
$$ u_k \in C^\infty_c(\Omega) : u_k \to u$$ 
in $$W^{1,p}(\Omega)$$. \\
Using Green's identity from calculus,
and the fact that $$u_k$$ have compact support we have

\\[ \left\vert \int_\Omega u_k 
    \frac{\partial \varphi}{\partial x_i} \right\vert 
    = \left\vert \int_\Omega  
        \frac{\partial u_k}{\partial x_i} \varphi \right\vert 
    \leq \left\lVert \frac{\partial u_k}{\partial x_i} 
        \right\rVert_{L^p(\Omega)} \lVert \varphi \rVert_{L^q{\Omega}}
    \quad \forall \varphi \in C^\infty_c(\Omega)
\\]

where we used H&ouml;lder's inequality in the last step. -->

















