\#\# Agenda

-   Basics of optimization
-   Gradient descent
-   Newton's method
-   Curve-fitting
-   R: `optim`, `nls`

Examples of Optimization Problems
---------------------------------

-   Minimize mean-squared error of regression surface
-   Maximize likelihood of distribution
-   Maximize output of tanks from given supplies and factories;
-   Maximize return of portfolio for given volatility
-   Minimize cost of airline flight schedule

Optimization Problems
---------------------

Given an **objective function** *f* : 𝒟 ↦ *R*, find

*θ*<sup>\*</sup> = argmin<sub>*θ*</sub>*f*(*θ*)

Basics: maximizing *f* is minimizing −*f*:

$$
\\argmax\_{\\theta}{f(\\theta)}= \\argmin\_{\\theta}{-f(\\theta)}
$$

If *h* is strictly increasing (e.g., log), then

$$
\\argmin\_{\\theta}{f(\\theta)} = \\argmin\_{\\theta}{h(f(\\theta))}
$$

Considerations
--------------

-   Approximation: How close can we get to *θ*<sup>\*</sup>, and/or
    *f*(*θ*<sup>\*</sup>)?

-   Time complexity: How many computer steps does that take?  
    <small>Varies with precision of approximation, niceness of *f*, size
    of 𝒟, size of data, method...</small>

-   Most optimization algorithms use **successive approximation**, so
    distinguish number of iterations from cost of each iteration

You remember calculus, right?
-----------------------------

Suppose *x* is one dimensional and *f* is smooth. If *x*<sup>\*</sup> is
an **interior** minimum / maximum / extremum point

$$
{\\left. \\frac{df}{dx} \\right|}\_{x=x^\*} = 0
$$

If *x*<sup>\*</sup> a minimum,
$$
{\\left. \\frac{d^2f}{dx^2}\\right|}\_{x=x^\*} &gt; 0
$$

------------------------------------------------------------------------

This all carries over to multiple dimensions:

At an **interior extremum**,
∇*f*(*θ*<sup>\*</sup>)=0

At an **interior minimum**,
∇<sup>2</sup>*f*(*θ*<sup>\*</sup>)≥0
 meaning for any vector *v*,
*v*<sup>*T*</sup>∇<sup>2</sup>*f*(*θ*<sup>\*</sup>)*v* ≥ 0
 ∇<sup>2</sup>*f*= the **Hessian**, **H**

*θ* might just be a **local** minimum

Gradients and Changes to f
--------------------------

$$
f^{\\prime}(x\_0)  =  {\\left. \\frac{df}{dx}\\right|}\_{x=x\_0} = \\lim\_{x\\rightarrow x\_0}{\\frac{f(x)-f(x\_0)}{x-x\_0}} $$

*f*(*x*)≈*f*(*x*<sub>0</sub>)+(*x* − *x*<sub>0</sub>)*f*<sup>′</sup>(*x*<sub>0</sub>)

Locally, the function looks linear; to minimize a linear function, move
down the slope

Multivariate version:
*f*(*θ*)≈*f*(*θ*<sub>0</sub>)+(*θ* − *θ*<sub>0</sub>)⋅∇*f*(*θ*<sub>0</sub>)

∇*f*(*θ*<sub>0</sub>) points in the direction of fastest ascent at
*θ*<sub>0</sub>

Gradient Descent
----------------

1.  Start with initial guess for *θ*, step-size *η*
2.  While ((not too tired) and (making adequate progress))

-   Find gradient ∇*f*(*θ*)
-   Set *θ* ← *θ* − *η*∇*f*(*θ*)

1.  Return final *θ* as approximate *θ*<sup>\*</sup>

Variations: adaptively adjust *η* to make sure of improvement or search
along the gradient direction for minimum

<!--

## Pros and Cons of Gradient Descent


Pro:

- Moves in direction of greatest immediate improvement
- If $\eta$ is small enough, gets to a local minimum eventually, and then stops

Cons:

- "small enough" $\eta$ can be really, really small
- Slowness or zig-zagging if components of $\nabla f$ are very different sizes

How much work do we need?


## Scaling


Big-$O$ notation:

\[
h(x) = O(g(x))
\]

means

\[
\lim_{x\rightarrow\infty}{\frac{h(x)}{g(x)}} = c
\]

for some $c \neq 0$  

<small>e.g., $x^2 - 5000x + 123456778 = O(x^2)$</small>

<small>e.g., $e^{x}/(1+e^{x}) = O(1)$</small>

Useful to look at over-all scaling, hiding details

Also done when the limit is $x\rightarrow 0$



## How Much Work is Gradient Descent?



Pro:

- For nice $f$, $f(\theta) \leq f(\theta^*)+\epsilon$ in $O(\epsilon^{-2})$ iterations
+ <small>For _very_ nice $f$, only $O(\log{\epsilon^{-1}})$
iterations</small>
- To get $\nabla f(\theta)$, take $p$ derivatives, $\therefore$ each iteration costs $O(p)$

Con:

- Taking derivatives can slow down as data grows --- each iteration might really be $O(np)$

-->
Taylor Series
-------------

What if we do a quadratic approximation to *f*?

$$
f(x) \\approx f(x\_0) + (x-x\_0)f^{\\prime}(x\_0) + \\frac{1}{2}(x-x\_0)^2
f^{\\prime\\prime}(x\_0)
$$

<small>Special cases of general idea of Taylor approximation</small>

Simplifies if *x*<sub>0</sub> is a minimum since then
*f*<sup>′</sup>(*x*<sub>0</sub>)=0:

$$
f(x) \\approx f(x\_0) + \\frac{1}{2}(x-x\_0)^2 f^{\\prime\\prime}(x\_0)
$$

Near a minimum, smooth functions look like parabolas

Carries over to the multivariate case:
$$
f(\\theta) \\approx f(\\theta\_0) + (\\theta-\\theta\_0) \\cdot \\nabla f(\\theta\_0) +
\\frac{1}{2}(\\theta-\\theta\_0)^T \\mathbf{H}(\\theta\_0) (\\theta-\\theta\_0)
$$

Minimizing a Quadratic
----------------------

If we know
*f*(*x*)=*a**x*<sup>2</sup> + *b**x* + *c*

we minimize exactly:

$$
\\begin{eqnarray\*}
2ax^\* + b & = & 0\\\\
x^\* & = & \\frac{-b}{2a}
\\end{eqnarray\*}
$$

If
$$
f(x) = \\frac{1}{2}a (x-x\_0)^2 + b(x-x\_0) + c
$$

then
*x*<sup>\*</sup> = *x*<sub>0</sub> − *a*<sup>−1</sup>*b*

Newton's Method
---------------

Taylor-expand for the value *at the minimum* *θ*<sup>\*</sup>
$$
    f(\\theta^\*) \\approx f(\\theta) + (\\theta^\*-\\theta) \\nabla f(\\theta) +
      \\frac{1}{2}(\\theta^\*-\\theta)^T \\mathbf{H}(\\theta) (\\theta^\*-\\theta)
    $$

Take gradient, set to zero, solve for *θ*<sup>\*</sup>:
$$
    \\begin{eqnarray\*}
    0 & = & \\nabla f(\\theta) + \\mathbf{H}(\\theta) (\\theta^\*-\\theta) \\\\
    \\theta^\* & = & \\theta - {\\left(\\mathbf{H}(\\theta)\\right)}^{-1} \\nabla f(\\theta)
    \\end{eqnarray\*}
    $$

Works *exactly* if *f* is quadratic  
<small>and **H**<sup>−1</sup> exists, etc.</small>

If *f* isn't quadratic, keep pretending it is until we get close to
*θ*<sup>\*</sup>, when it will be nearly true

Newton's Method: The Algorithm
------------------------------

1.  Start with guess for *θ*
2.  While ((not too tired) and (making adequate progress))

-   Find gradient ∇*f*(*θ*) and Hessian **H**(*θ*)
-   Set *θ* ← *θ* − **H**(*θ*)<sup>−1</sup>∇*f*(*θ*)

1.  Return final *θ* as approximation to *θ*<sup>\*</sup>

Like gradient descent, but with inverse Hessian giving the step-size

<small>"This is about how far you can go with that gradient"</small>

<!--
  
  ## Advantages and Disadvantages of Newton's Method
  
  Pros:
  
  - Step-sizes chosen adaptively through 2nd derivatives, much harder to get zig-zagging, over-shooting, etc.
- Also guaranteed to need $O(\epsilon^{-2})$ steps to get within $\epsilon$ of optimum
- Only $O(\log\log{\epsilon^{-1}})$ for very nice functions
- Typically many fewer iterations than gradient descent



## Advantages and Disadvantages of Newton's Method


Cons:
  
  - Hopeless if $\mathbf{H}$ doesn't exist or isn't invertible
- Need to take $O(p^2)$ second derivatives *plus* $p$ first derivatives
- Need to solve $\mathbf{H} \theta_{\mathrm{new}} = \mathbf{H} \theta_{\mathrm{old}} - \nabla f(\theta_{\mathrm{old}})$ for $\theta_{\mathrm{new}}$
  + <small>inverting $\mathbf{H}$ is $O(p^3)$, but cleverness gives
$O(p^2)$ for solving for $\theta_{\mathrm{new}}$</small>
  
  
  ## Getting Around the Hessian
  
  
  Want to use the Hessian to improve convergence

Don't want to have to keep computing the Hessian at each step

Approaches:

- Use knowledge of the system to get some approximation to the Hessian, use that instead of taking derivatives ("Fisher scoring")
- Use only diagonal entries ($p$ unmixed 2nd derivatives)
- Use $\mathbf{H}(\theta)$ at initial guess, hope $\mathbf{H}$ changes
*very* slowly with $\theta$
- Re-compute $\mathbf{H}(\theta)$ every $k$ steps, $k > 1$
- Fast, approximate updates to the Hessian at each step (BFGS)


## Other Methods


- Lots!
- See bonus slides at end for for "Nedler-Mead", a.k.a. "the simplex method", which doesn't need any derivatives
- See bonus slides for the meta-method "coordinate descent"

-->

\#\# Curve-Fitting by Optimizing

We have data
(*x*<sub>1</sub>, *y*<sub>1</sub>),(*x*<sub>2</sub>, *y*<sub>2</sub>),…(*x*<sub>*n*</sub>, *y*<sub>*n*</sub>)

We also have possible curves, *r*(*x*; *θ*)

e.g., *r*(*x*)=*x* ⋅ *θ*

e.g., *r*(*x*)=*θ*<sub>1</sub>*x*<sup>*θ*<sub>2</sub></sup>

<!--
  e.g., $r(x) = \sum_{j=1}^{q}{\theta_j b_j(x)}$ for fixed "basis" functions
$b_j$
  -->

\#\# Curve-Fitting by Optimizing

Least-squares curve fitting:

$$
    \\hat{\\theta} = \\argmin\_{\\theta}{\\frac{1}{n}\\sum\_{i=1}^n{(y\_i - r(x\_i;\\theta))^2}}
    $$

"Robust" curve fitting:
$$
    \\hat{\\theta} = \\argmin\_{\\theta}{\\frac{1}{n}\\sum\_{i=1}^{n}{\\psi(y\_i - r(x\_i;\\theta))}}
    $$

Derivatives in R
----------------

    f <- expression(5*x^2+3*x+1) ; deriv(f,"x")

    ## expression({
    ##     .value <- 5 * x^2 + 3 * x + 1
    ##     .grad <- array(0, c(length(.value), 1L), list(NULL, c("x")))
    ##     .grad[, "x"] <- 5 * (2 * x) + 3
    ##     attr(.value, "gradient") <- .grad
    ##     .value
    ## })

    x <- 2:4 ; eval(deriv(f,"x"))

    ## [1] 27 55 93
    ## attr(,"gradient")
    ##       x
    ## [1,] 23
    ## [2,] 33
    ## [3,] 43

Integrate in R
--------------

    g <- function(x) eval(f)
    integrate(g,lower = 0,upper = 2)

    ## 21.33333 with absolute error < 2.4e-13

    h <- function(x) exp(cos(x)^2)+exp(sin(x))
    integrate(h,1,5)

    ## 11.99573 with absolute error < 0.00046

Optimization in R: optim()
--------------------------

    optim(par, fn, gr, method, control, hessian)

-   `fn`: function to be minimized; mandatory
-   `par`: initial parameter guess; mandatory
-   `gr`: gradient function; only needed for some methods
-   `method`: defaults to a gradient-free method (\`\`Nedler-Mead''),
    could be BFGS (Newton-ish)
-   `control`: optional list of control settings
-   <small>(maximum iterations, scaling, tolerance for convergence,
    etc.)</small>
-   `hessian`: should the final Hessian be returned? default FALSE

Return contains the location (`$par`) and the value (`$val`) of the
optimum, diagnostics, possibly `$hessian`

Optimization in R: optim()
--------------------------

    gmp <- read.table("../data/gmp.dat")
    gmp$pop <- gmp$gmp/gmp$pcgmp
    library(numDeriv)
    mse <- function(theta) { mean((gmp$pcgmp - theta[1]*gmp$pop^theta[2])^2) }
    grad.mse <- function(theta) { grad(func=mse,x=theta) }
    theta0=c(5000,0.15)
    fit1 <- optim(theta0,mse,grad.mse,method="BFGS",hessian=TRUE)

fit1: Newton-ish BFGS method
----------------------------

    fit1[1:3]

    ## $par
    ## [1] 6493.2563738    0.1276921
    ## 
    ## $value
    ## [1] 61853983
    ## 
    ## $counts
    ## function gradient 
    ##       63       11

fit1: Newton-ish BFGS method
----------------------------

    fit1[4:6]

    ## $convergence
    ## [1] 0
    ## 
    ## $message
    ## NULL
    ## 
    ## $hessian
    ##             [,1]         [,2]
    ## [1,] 5.25021e+01      4422070
    ## [2,] 4.42207e+06 375729087977

nls
---

`optim` is a general-purpose optimizer

So is `nlm` --- try them both if one doesn't work

`nls` is for nonlinear least squares

nls
---

    nls(formula, data, start, control, [[many other options]])

-   `formula`: Mathematical expression with response variable, predictor
    variable(s), and unknown parameter(s)
-   `data`: Data frame with variable names matching `formula`
-   `start`: Guess at parameters (optional)
-   `control`: Like with `optim` (optional)

Returns an `nls` object, with fitted values, prediction methods, etc.

The default optimization is a version of Newton's method

fit2: Fitting the Same Model with nls()
---------------------------------------

    fit2 <- nls(pcgmp~y0*pop^a,data=gmp,start=list(y0=5000,a=0.1))
    summary(fit2)

    ## 
    ## Formula: pcgmp ~ y0 * pop^a
    ## 
    ## Parameters:
    ##     Estimate Std. Error t value Pr(>|t|)    
    ## y0 6.494e+03  8.565e+02   7.582 2.87e-13 ***
    ## a  1.277e-01  1.012e-02  12.612  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 7886 on 364 degrees of freedom
    ## 
    ## Number of iterations to convergence: 5 
    ## Achieved convergence tolerance: 1.751e-07

fit2: Fitting the Same Model with nls()
---------------------------------------

    plot(pcgmp~pop,data=gmp)
    pop.order <- order(gmp$pop)
    lines(gmp$pop[pop.order],fitted(fit2)[pop.order])
    curve(fit1$par[1]*x^fit1$par[2],add=TRUE,lty="dashed",col="blue")

![](Section10_files/figure-markdown_strict/unnamed-chunk-7-1.png)

Maximize log-likelihood
-----------------------

Imagine that we have a sample that was drawn from a normal distribution
with unknown mean, *μ*, and variance, *σ*<sup>2</sup>. The objective is
to estimate these parameters. The normal log-likelihood function is
given by

$$ l = −.5 n ln (2\\pi)−.5 n ln(\\sigma^2)− \\frac{1}{2\\sigma^2} \\sum\_{i} (y\_i -\\mu)^2 $$
 We can program this function in the following way:

`r   normal.lik<-function(theta,y) {    mu<-theta[1]   sigma2<-theta[2]   n <- length(y)   logl <- -.5*n*log(2*pi) -.5*n*log(sigma2) - (1/(2*sigma2))*sum((y-mu)**2)    return (-logl)   }`

------------------------------------------------------------------------

Once the log-likelihood function has been declared, then the optim
command can be invoked. The minimal specification of this command is

`optim(starting values, log-likelihood, data)`

Given a vector of data, y, the parameters of the normal distrib- ution
can be estimated using

    y <- rnorm(1000,1,5);
    optim(c(0,1),normal.lik,y=y,method="BFGS")

    ## $par
    ## [1]  0.8896141 23.7552151
    ## 
    ## $value
    ## [1] 3002.84
    ## 
    ## $counts
    ## function gradient 
    ##       57       29 
    ## 
    ## $convergence
    ## [1] 0
    ## 
    ## $message
    ## NULL

Summary
-------

1.  Trade-offs: complexity of iteration vs. number of iterations vs.
    precision of approximation

-   Gradient descent: less complex iterations, more guarantees, less
    adaptive
-   Newton: more complex iterations, but few of them for good functions,
    more adaptive, less robust

1.  Start with pre-built code like `optim` or `nls`, implement your own
    as needed
