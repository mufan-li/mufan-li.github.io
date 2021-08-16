---
title: An Unusally Clean Proof&#58; Dyson Brownian Motion via Conditioning on Non-intersection
---

Dyson Brownian motion [Dy62] is best known to characterize the eigenvalues of special random matrices [Ta12]. Most interestingly, it is also equal in distribution to $$n$$ independent Brownian motions conditioned to not intersect [Gr99]. In a topics course by [B&aacute;lint Vir&aacute;g](http://www.math.toronto.edu/balint/), I came across a proof of this result that is *just too clean* for this type of calculations. After picking up my jaw from the ground months later, I finally decided to write up this surprisingly elegant proof. 

## Background: Doob's h-Transform

To compute the conditional dynamics of Markov processes, we will use the h-transform by [Joseph Doob](https://en.wikipedia.org/wiki/Joseph_L._Doob) [Bl10]. Let us consider a time-homogeneous Markov process $$\{ X(t) \}_{t \geq 0}$$ to be conditioned on a shift invariant event $$A$$, i.e. 

$$ \mathbb{P}( \{ X(t) \}_{t\geq 0} \in A | X(0) = x ) 
	= \mathbb{P}( \{ X(t+s) \}_{t\geq 0} \in A | X(s) = x ) \,. 
$$

An important example of a shift invariant event is the *gambler's ruin* example, where $$X(t) \in (0,c)$$ is a martingale, and the event can be defined as 

$$ A := \left\{ X(t) \text{ hits } c \text{ before } 0 \right\} \,. 
$$

Here $$X(t)$$ is intended to model a gambler's wealth process in a fair betting game, and it's well known that $$\mathbb{P}(A) = \frac{X(0)}{c}$$ (a direct consequence of optional stopping theorem). 

We will provide a simple sketch of the h-transform result (see [Bl10] for a rigorous proof). Here we introduce the following notations: 

$$ 
\begin{split}
    \mathbb{P}_x( \cdot ) &:= \mathbb{P}( \cdot | X(0) = x ) \,, 
	\\
    h(x) &:= \mathbb{P}_x(A) \,, 
	\\
	P^t(x, dy) &:= \mathbb{P}_x( X(t) \in dy ) \,, 
	\\
	\tilde{P}^t(x, dy) &:= \mathbb{P}_x( X(t) \in dy | A) \,, 
\end{split}
$$

where $$h(x)$$ is the key transform function, and $$P^t(x,dy)$$ is the transition kernel, which completely characterizes the dynamics of a Markov process. Therefore our goal is to compute $$\tilde{P}^t(x,dy)$$, which we can just use Bayes' rule 

$$
\begin{aligned}
	\tilde{P}^t(x, dy) &= \mathbb{P}_x( X(t) \in dy | A) 
	\\
	&= \frac{ \mathbb{P}_x(A | X(t) \in dy) \mathbb{P}_x( X(t) \in dy ) }{ \mathbb{P}_x(A) } 
	\\
	&= \frac{h(y)}{h(x)} P^t(x, dy) \,, 
\end{aligned}
$$

where we used the shift invariance of $$A$$ to write $$\mathbb{P}_x(A \vert X(t) \in dy) = h(y)$$. In other words, the Radon--Nikodym derivative for the transition kernel is simply the ratio $$h(y)/h(x)$$! 

To make calculations even simpler, we will also compute the effect on the infinitesimal generator, namely the operator $$L$$ defined as follows: 

$$ L[f](x) := \lim_{t \to 0} \frac{ \mathbb{E}_x f(X(t)) - f(x) }{t} \,, 
	\quad \text{ if it exists, }
$$

where we define $$\mathbb{E}_x(\cdot) := \mathbb{E}( \cdot \vert X(0) = x)$$. 

We want to then compute 

$$ 
\begin{aligned}
	\tilde{L}f 
	&:= 
		\lim_{t \to 0} \frac{ \mathbb{E}_x( f(X(t)) | A ) - f(x) }{t} 
	\\
	&= 
		\lim_{t \to 0} \frac{1}{t} \left( 
			\int f(y) \tilde{P}^t(x,dy) - f(x) 
			\right) 
		\\ 
	&= 
		\lim_{t \to 0} \frac{1}{t} \left( 
			\int f(y) \frac{h(y)}{h(x)} P^t(x,dy) - f(x) 
			\right) 
		\\
	&= 
		\frac{1}{h(x)} \lim_{t \to 0} \frac{1}{t} \left( 
			\int (f(y) h(y) - f(x) h(x)) P^t(x,dy) 
			\right) 
		\\
	&= 
		\frac{1}{h(x)} L[fh](x) \,, 
\end{aligned}
$$

where we used the h-transform result above and the definition of the generator. 

At this point, we will recall a well known that $$h$$ is harmonic, i.e. $$Lh = 0$$, to save some calculations (I suspect the letter "h" in h-transform stands for "harmonic"). We also recall that for a diffusion process $$dX(t) = \mu(X(t))\,dt + dB(t)$$, the generator follows from It&ocirc;'s Lemma 

$$ L[f](x) := \langle \mu(x), \nabla f(x) \rangle + \frac{1}{2} \Delta f(x) \,. 
$$

Using the harmonic property, we have the following clean formula for the transformed generator 

$$ \tilde{L} [f](x) = \frac{ L[fh](x) }{h(x)} = \langle \mu(x) + \nabla \log h(x), \nabla f(x) \rangle + \frac{1}{2} \Delta f(x) \,, 
$$

which corresponds to the diffusion process 

$$ d\tilde{X}(t) = ( \mu(\tilde{X}(t)) + \nabla \log h(\tilde{X}(t)) ) \, dt  + dB(t) \,. 
$$

To summarize, *h-transform simply adds a* **drift term** *to the original process*! Here we remark that although the above formula looks simple in terms of $$h(x)$$, the function $$h(x)$$ itself is often quite complicated, making this calculation at least convoluted if not *completely intractable*. This is why the proof coming out so clean in the next section is a huge surprise. 




## Dyson Brown Motion via Conditioning on Non-intersection 

Here we will first state the main result. 

---

**Theorem** Let $$\{ \lambda_i(t) \}_{t\geq 0, i \in [n]}$$ be the Dyson Brownian motions, i.e. $$\lambda(t)$$ satisfy the following stochastic differential equation (SDE) 

$$ d\lambda_i(t) = dB_i(t) + \sum_{j \neq i} \frac{dt}{ \lambda_i(t) - \lambda_j(t) } \,, 
$$

where the initial conditions satisfy $$\lambda_1(0) > \lambda_2(0) > \cdots > \lambda_n(0)$$, and \{$$B_i(t)$$\} are independent standard Brownian motions. Then we have the following equality in distribution 

$$ \{ \lambda_i(t) \}_{t \geq 0, i \in [n]} \overset{d}{=} \{ B_i(t) \}_{t \geq 0, i \in [n]} \vert A \,, 
$$

where we define $$ A := \{ \{B_i(t)\} \text{ do not intersect } \} $$. 

---

Before we start, we note the event of $$n$$ Brownian motions to not intersect for all time has zero probability. Then what does it even mean to condition on an event of zero probability? Well we would consider a collection of events $$\{A_c\}_{c>0}$$ converging to the null event $$A$$, such that $$\mathbb{P}(A_c) > 0$$ for all $$c>0$$, and we can compute dynamics of these Brownian motions in the limit as $$c\to \infty$$. 

To define these events $$A_c$$, we will define the *Vandermonde determinant*: 

$$ \Delta_n( \lambda ) = \prod_{1 \leq i < j \leq n} ( \lambda_i - \lambda_j ) \,. 
$$

Here we observe that since $$\lambda$$ is sorted in decreasing order, we have that 

$$ \Delta_n( \lambda ) > 0 \iff \lambda_i \neq \lambda_j \quad \forall i \neq j \,. 
$$ 

Therefore, we can define the events $$A_c := \{ \Delta_n( B(t) ) \text{ hits } c \text{ before } 0 \}$$. Observe that we can indeed recover the non-intersection event $$A$$ in the limit 

$$ A = \lim_{c \to \infty} A_c \,. 
$$

Recalling the gambler's ruin example, if $$\Delta_n( B(t) )$$ is a martingale, we have a very simple formula for the h-transform 

$$ h_c( x ) := \mathbb{P}_{x}( A_c ) = \frac{ \Delta_n(x) }{ c } \,. 
$$

Indeed we will first prove this result. 

--- 

**Lemma** $$\Delta_n(B(t))$$ is a martingale. 

---

*proof (of Lemma):* We will directly compute the SDE of $$\Delta_n(B(t))$$ using It&ocirc;'s Lemma 

$$ d \Delta_n(B(t)) 
	= \frac{1}{2} \Delta \Delta_n(B(t)) \, dt + \cdots dB(t) \,, 
$$

where hide the diffusion term since the It&ocirc; integral with respect to a martingale is also a martingale. Therefore it's sufficient to show the drift term is zero. 

Using the identity 

$$ \frac{1}{(a-b)(a-c)} + \frac{1}{(b-a)(b-c)} + \frac{1}{(c-a)(c-b)} = 0 \,, 
$$

it's a simple calculation to show that $$\Delta \Delta_n(x) = 0$$ via this symmetry, and hence the desired result follows. 

$$\tag*{$\Box$}$$


*proof (of Theorem):* It remains to compute the h-transform dynamics, in particular the drift term 

$$
\begin{aligned}
	\partial_i \log h_c(x) 
	&= 
		\partial_i ( \log \Delta_n(x) - \log c ) 
	\\
	&= 
		\frac{1}{\Delta_n(x)} \sum_{j \neq i} \frac{\Delta_n(x)}{ x_i - x_j } \,, 
\end{aligned}
$$

which implies that conditioned on the event $$A_c$$, the h-transformed process satisfy the SDE 

$$
\begin{aligned}
	d \lambda_i(t) 
	&= \partial_i \log h_c( \lambda(t) ) \, dt + dB_i(t) 
	\\
	&= \sum_{j \neq i} \frac{ dt }{ \lambda_i(t) - \lambda_j(t) } + dB_i(t) \,. 
\end{aligned}
$$

Finally, to complete the proof, we observe the dynamics of $$\lambda(t)$$ is invariant to changes in $$c>0$$, therefore taking $$c \to \infty$$ recovers the unbounded dynamics of Dyson Brownian motion. 

$$\tag*{$\Box$}$$



## Final Words 

That's it! That's the proof! Having played around with h-transforms before, and getting only ridiculously ugly expressions, it's quite remarkable to me that this proof was able to avoid messy calculations all together. 

What helped simplified this proof? To quote B&aacute;lint Vir&aacute;g: "[t]his is because the Vandermonde [determinant] is harmonic, so this fits into the h-transform language." Indeed, we saw above that $$\Delta \Delta_n(x) = 0$$ played an important role in the calculations. So let this be a rule of thumb for future problems: try to define events using harmonic functions in h-transforms! 

For those interested in random matrix theory, Terrence Tao wrote some very nice [lecture notes](https://terrytao.wordpress.com/2010/01/18/254a-notes-3b-brownian-motion-and-dyson-brownian-motion/) on the applications of Dyson Brownian motion, which can also be found in his book [Ta12]. In particular, we can use Dyson Brownian motion to derive the eigenvalue density of a Gaussian unitary ensemble (GUE) matrix, common known as the *Ginibre formula*: 

$$ \rho(\lambda) = \frac{1}{(2\pi)^{n/2} 1! \cdots (n-1)!} e^{ -|\lambda|^2/2 } |\Delta_n(\lambda)|^2 \,, 
$$ 

which can then be used to derive the famous [Wigner's semicircle law](https://mathworld.wolfram.com/WignersSemicircleLaw.html) in the limit $$n \to \infty$$. 







## References 

<!-- - \[Bak08\] Y. Bakhtin, "Exit asymptotics for small diffusion about an unstable equilibrium." Stochastic processes and their applications 118.5 (2008): 839-851.  -->
- [Bl10] Bloemendal, Alex. "Doob’s h-transform: theory and examples." Lecture notes (2010).
- [Dy62] Dyson, Freeman J. "A Brownian‐motion model for the eigenvalues of a random matrix." Journal of Mathematical Physics 3.6 (1962): 1191-1198.
- [Gr99] Grabiner, David J. "Brownian motion in a Weyl chamber, non-colliding particles, and random matrices." Annales de l'IHP Probabilit&eacute;s et statistiques. Vol. 35. No. 2. 1999.
- [Ta12] Tao, Terence. Topics in random matrix theory. Vol. 132. American Mathematical Soc., 2012.
