---
title: "Calculus Recap"
date: 2023-08-30
draft: false
math: true
---

## Intermediate Value Theorem (IVT)

If a function $f(x)$ is continuous on a closed interval $[a, b]$ and $k$ is any number
between $f(a)$ and $f(b)$, then there is at least one number $c$ in the interval
$[a, b]$ such that $f( c) = k$.

The intermediate value theorem has two conditions:

- Function $f$ is continuous over the interval $[a, b]$.
- The value $d$ lies between $f(a)$ and $f(b)$

We _must_ establish these two conditions to conclude that there is a value $c$ in
the interval $[a, b]$ for which $g(x) = d$ (another way to phrase this conclusion is
that the equation $g(x) = d$ has a solution where $a <= x <= b$).

## Property of limits of composite functions

If $u$ are functions such that $\displaystyle\lim_{x \to c} u(x) = L$ and
$\displaystyle\lim_{x \to L} u(x) = u(L)$ (which means both limits exist and $u$ is
continuous at $x = L$), then

$$\displaystyle\lim_{x \to c} u(u(x)) = u(\lim_{x \to c} u(x)) = u(L)$$

### Dealing with composite function's limits

When dealing with **composite exponential functions**, we can let $y$ equals to that
function, then take natural log on both sides:

| Steps | Process                                                                                              |
| ----- | ---------------------------------------------------------------------------------------------------- |
| 1     | $y = (2x - 1)^\frac{1}{x - 1}$                                                                       |
| 2     | $\ln(y) = \ln((2x - 1)^\frac{1}{x - 1}) = \frac{1}{x - 1} \cdot \ln(2x-1) = \frac{\ln(2x-1)}{x - 1}$ |

By doing this, we can bring the complex exponent in front.

## Squeeze Theorem

The _squeeze theorem_ tells us that if we have three function $f$, $g$, and $h$ such
that...

- $g(x) <= f(x) <= h(x)$ for all $x$-values other than $x = c$ in an interval round
  $x = c$, and
- $\displaystyle\lim_{x \to c} g(x) = \lim_{x \to c} h(x) = L$

Then we can conclude that $\displaystyle\lim_{x \to c} f(x) = L$.

## Derivative of Function

The derivative of function $f$ at the point where $x = a$ is defined by:

$$f\prime(a) = \lim_{{h \to 0}} \frac{{f(a + h) - f(a)}}{h}$$

The alternative form is:

$$f\prime(a) = \lim_{{x \to a}} \frac{{f(x) - f(a)}}{x - a}$$

---

The whole underlying idea of the formal definition of limits is to find the slope of the
secant line between these two points, and then take the limit as $h$ approaches $0$. And
the secant line is going to become a better and better approximation of the tangent line
at $x$.

## Under what conditions can a function be considered continuous?

A function $f(x)$ is considered continuous at a point $x = a$ if the following
conditions are all met:

- Defined: The function $f(x)$ is defined at $x = a$. Meaning you can substitute a into
  the function and get a real number.

- Limit Exists: The limit of the function as $x$ approaches $a$ exists. This means
  that as $x$ gets closer and closer to a from either direction, $f(x)$ approaches some
  specific value $L$.

- Function Equals Limit: The value of the function at $a$ is equal to the limit of the
  function as $x$ approaches $a$. In other words, $f(a) = L$.

The function $f(x)$ is said to be continuous on an interval if it is continuous at
every point in that interval. It is said to be continuous everywhere or simply
continuous if it is continuous at every point over its entire domain.

So, Mathematically, A function $f(x)$ is continuous at $x = a$ if 
$\lim_{{x \to a}} f(x) = f(a)$

Or more explicitly, it is continuous at $x = a$ if 
$\lim_{{x \to a^-}} f(x) = f(a) = \lim_{{x \to a^+}} f(x)$

Where $\lim_{{x \to a^-}} f(x)$ is the limit as $x$ approaches $a$ from the left, $f(a)$ 
is the function value at $a$, and $\lim_{{x \to a^+}} f(x)$ is the limit as $x$ 
approaches $a$ from the right.

If all three of these quantities are the same, then $f(x)$ is continuous at $x = a$.

## Differentiability Implies **Continuity**

There are three cases where a function is _not differentiable_:

1. Wherever the function isn't continuous
2. Wherever the graph of the function has a _vertical_ tangent line
3. Wherever the graph of the function has a sharp turn

---

- In order to be differentiable, you need to be continuous. You can't have
  differentiable but not continuous.
- In order to be continuous, $\displaystyle f(a) = \lim_{{x \to a}} f(x)$. Two
  one-sided limits exist, and are equal to $f(a)$.
- In order to be differentiable,
  $\displaystyle \lim_{{x \to a}} \frac{{f(x) - f(a)}}{x - a}$ needs to exist.

If a function is differentiable then it's also continuous. This property is very useful
when working with functions, because if we know that a function is differentiable, we
immediately know that it's also continuous.

## Differentiation Rules

| Rules                  | Expression                                                                                                                      |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Constant rule          | $\frac{d}{dx}(k) = 0$                                                                                                           |
| Constant multiple rule | $\frac{d}{dx}[k \cdot f(x)] = k \cdot f\prime(x)$                                                                               |
| Sum rule               | $\frac{d}{dx}[f(x) + g(x)] = f\prime(x) + g\prime(x)$                                                                           |
| Difference rule        | $\frac{d}{dx}[f(x) - g(x)] = f\prime(x) - g\prime(x)$                                                                           |
| Power Rule             | $\frac{d}{dx} (x^n) = n x^{n-1}$                                                                                                |
| Product Rule           | $\frac{d}{dx} [f(x) \cdot g(x)] = \frac{d}{dx}[f(x)] \cdot g(x) + f(x) \cdot \frac{d}{dx}[g(x)]$                                |
| Quotient Rule          | $\frac{d}{dx} \left[\frac{f(x)}{g(x)}\right] = \frac{\frac{d}{dx} [f(x)] \cdot g(x) -f(x) \cdot \frac{d}{dx}[g(x)]} {[g(x)]^2}$ |

## Chain Rule

If $g$ is differentiable at $x$ and $f$ is differentiable at $g(x)$, then the composite
function $F = f \circ g$ defined by $F(x) = f(g(x))$ is differentiable at $x$ and
$F\prime$ is given by the product

$$
F\prime[f(g(x))] = f\prime(g(x)) \cdot g\prime(x)
$$

if $y = f(u)$ and $u = g(x)$ are both differentiable functions, then

$$
\frac{dy}{dx} = \frac{dv}{du} \cdot \frac{du}{dx}
$$

## Derivatives of the Trigonometric Functions

| Trigonometric | Derivative                                                                                                         |
| ------------- | ------------------------------------------------------------------------------------------------------------------ |
| sin           | $\frac{d}{dx} [\sin (x)] = \cos(x)$                                                                                |
| cos           | $\frac{d}{dx} [\cos (x)] = -\sin(x)$                                                                               |
| tan           | $\frac{d}{dx} [\tan (x)] = \frac{d}{dx} \left [\frac{\sin(x)}{\cos(x)} \right] = \frac{1}{\cos^2(x)} = \sec^2(x)$  |
| cot           | $\frac{d}{dx} [\cot (x)] = \frac{d}{dx} \left[\frac{\cos(x)}{\sin(x)}\right] = -\frac{1} {\sin^2(x)} = -\csc^2(x)$ |
| csc           | $\frac{d}{dx} [\csc (x)] = -\frac{\cos(x)}{\sin^2(x)} = -\csc(x) \cdot \cot(x)$                                    |
| sec           | $\frac{d}{dx} [\sec (x)] = \frac{\sin(x)}{\cos^2(x)} = \sec(x) \cdot \tan(x)$                                      |

| Two Special Trigonometric Limits                         |
| -------------------------------------------------------- |
| $\lim_{\theta \to 0} \frac{\sin \theta}{\theta} = 1$     |
| $\lim_{\theta \to 0} \frac{\cos \theta - 1}{\theta} = 0$ |

## Derivative of $e$

$$ \frac{d}{dx} e^x = e^x $$

## Derivative of $\ln(x)$

$$ \frac{d}{dx} [\ln(x)] = \frac{1}{x} $$

## Composite and combined functions

A composite function is where we make the output from one function, such as $u$, the
input for another function, such as $w$.

We can also combine functions using arithmetic operations, but such a combination is not
considered a composite function.

Our two functions appear to be $x-8$ and $e^x$, but neither of them _takes the other
as its input_.

## Derivatives of Exponential Functions

If $b$ is a constant, then

$$ \frac{d}{dx} (b^x) = \ln(b) \cdot b^x $$

## Derivatives of Logarithmic Functions

If $a$ is a constant, $a \neq 1$, then

$$ \frac{d}{dx} \left[\log_a(x)\right] = \frac{1}{\ln(a) \cdot x} $$

$$ \frac{d}{dx} \left[\ln(u(x)) \right] = \frac{u\prime(x)}{u(x)} $$

## Derivative of Inverse Function

Let's say $f(x)$ and $g(x)$ are inverse function (and both are differentiable), so by
definition, we have this equation:

$$ f(g(x)) = x $$

We take derivative of both sides:

$$ \frac{d}{dx} f(g(x)) = \frac{d}{dx} (x)$$

Use chain rule to the left-hand side, right-hand side equals to 1. Finally, we get:

$$ f\prime(x) = \frac{1}{g\prime(f(x))} $$

## Derivative of Inverse Sine, Cosine, Tangent

$$\frac{d}{dx} \left[\sin^{-1}(x)\right] = \frac{1}{\sqrt{1 - x^2}} $$

$$\frac{d}{dx} \left[\cos^{-1}(x)\right] = -\frac{1}{\sqrt{1 - x^2}} $$

$$\frac{d}{dx} \left[\tan^{-1}(x)\right] = \frac{1}{1 + x^2} $$

## Interpret motion graph

The direction of movement is indicated by the velocity's sign:

- If the velocity is positive, the object is moving forward.
- If the velocity is negative, the object is moving backward.
- If the velocity is zero, the object is not moving.

The direction of the position graph indicates the direction of movement:

- If the graph is increasing, the object is moving forward.
- If the graph is decreasing, the object is moving backward.
- If the graph is nether increasing not decreasing, the object is not moving. This
  happens at extremum points of the graph.

Speed is the magnitude of velocity. When analyzing linear motion,

$$ \text{speed} = | \text{velocity} | $$

So the object is speeding up whenever the absolute value of the velocity is increasing,
and slowing down whenever the absolute value of the velocity is decreasing.

When the velocity is zero, the object isn't speeding up or slowing down.

---

Velocity as a function of time is the derivative of position:

$$ v(t) = x\prime(t) $$

Acceleration as a function of time is the derivative of velocity, which is the second
derivative of position:

$$ a(t) = v\prime(t) = x\prime\prime(t) $$

### Velocity and Acceleration

- Velocity: The rate of change of _displacement_ (位移) with respect to time.
- Acceleration: The instantaneous rate of change of _velocity_ with respect to time.

| Velocity | Acceleration | Speed      |
| -------- | ------------ | ---------- |
| Positive | Positive     | Increasing |
| Negative | Negative     | Increasing |
| Positive | Negative     | Decreasing |
| Negative | Positive     | Decreasing |

If the acceleration is positive, and velocity is positive, that means the magnitude
of velocity is increasing, so that means the speed is increasing.

If the acceleration is negative, and the velocity is also negative, that also means the
speed is increasing.

But the velocity and acceleration have different signs, that mean the speed is
decreasing.

## Related rates

This is the core of our solution: by relating the quantities (i.e., $A$ and $r$) we were
able to relate their rates (i.e., $A\prime$ and $r\prime$) through differentiation. This
is why these problems are called "related rates".

For example, what we know is $\frac{dy}{dt}$ ($y\prime(t)$), what we are seeking for is
$\frac{dx}{dt}$ ($x\prime(t)$). So, if we can find the relationship between $y$ and $x$,
then take derivative on them, we can find derivative of $x$ by solve for
$\frac{dx}{dt}$.

## Linear approximation

But what they're suggesting that we do is use this tangent line. If we know the
equation of this tangent line here, we can say what the tangent line equals when x
equals 1.9? When x equals 1.9, it equals that point over there. And then we could use
that as our approximation.

The tangent line gives us a "best" linear approximation to the function $h$ near the
point $x = 0$ (or $x = a$). If we were to graph the function $h$ and the line $y= 4x -
5$ together, the two would be very close together near $x = 0$ (or $x = a$), although
they may diverge as we move away or zoom out from this point.

## L'Hôpital's rule

L'Hôpital's rule states that for functions $f$ and $g$ which are differentiable on an
open interval $I$ except possibly at a point $c$ contained in $I$, if $\lim_{x \to c}
f(x) = \lim_{x \to c} g(x) = 0$ or $±\infty$, and $g\prime(x) \neq 0$ for all $x$ in $I$
with $x \neq c$, and $\lim_{x \to c} \frac{f\prime(x)}{g\prime(x)}$ exists, then

$$
\displaystyle \lim _{x\to c}{\frac {f(x)}{g(x)}}=\lim _{x\to c}{\frac {f\prime(x)}
{g\prime(x)}}
$$

The differentiation of the numerator and denominator often simplifies the quotient or
converts it to a limit that can be evaluated directly.

## Mean value theorem (MVT)

The mean value theorem connects the average rate of change of a function to its
derivative. It says that for any differentiable function $f$ and an interval $[a, b]$
(within the domain of $f$), there exists a number $c$ within $(a, b)$ such that
$f\prime(c)$ is equal to the function's average rate of change over $[a, b]$.

Graphically, the theorem says that for any arc (弧线) between two endpoints, there's a
point at which the tangent the arc is parallel to the secant through its endpoints.

> **Formal definition**
>
> Let $f$ be a function that satisfies the following hypotheses:
>
> 1. $f$ is continuous on the closed interval $[a, b]$.
> 2. $f$ is differentiable on the open interval $(a, b)$.
>
> Then there is a number $c$ in $(a, b)$ such that
>
> $$ f\prime(c) = \frac{f(b) - f(a)}{b - a} $$
>
> Or, equivalently,
>
> $$ f(b) - f(a) = f\prime(c)(b - a) $$

The mean value theorem has two conditions:

1. Function $g$ is **differentiable** over the open interval $(a, b)$ and **continuous**
   over the closed interval $[a, b]$.
2. The average rate of change is equal to the slope of that point on the function.
   $f\prime( c) = \frac{f(b) - f(a)}{b - a}$

## Critical number

A critical number of a function $f$ is a number $c$ in the domain of $f$ such that
either $f\prime(c) = 0$ or $f\prime(c)$ does not exist.

## Intervals on which a function is increasing or decreasing

| $f\prime$'s status                    |       | which means $f$   |
| ------------------------------------- | :---: | ----------------- |
| $f\prime > 0$ ($f\prime$ is positive) |  <=>  | $f$ is increasing |
| $f\prime < 0$ ($f\prime$ is negative) |  <=>  | $f$ is decreasing |

- The intervals where the derivative $f\prime$ is positive (i.e., above the $x$-axis)
  are the intervals where the function $f$ is increasing.
- The intervals where the derivative $f\prime$ is negative (i.e., below the $x$-axis)
  are the intervals where the function $f$ is decreasing.

A function can only change its direction from increasing to decreasing and vice versa
between its critical points and the points where the function itself is undefined.

## Relative extrema

- Local minimum is the point where $f\prime = 0$ and $f\prime\prime > 0$ (means
  $f\prime$ is increasing).
- Local maximum is the point where $f\prime = 0$ and $f\prime\prime < 0$ (means
  $f\prime$ is decreasing).

1. We must not assume that any critical point is an extremum. Instead, we should check
   out critical points to see if the function is defined at those points and the
   **derivative changes signs** at those points.
2. When we analyze increasing and decreasing intervals, we must look for all points
   where the derivative is equal to zero **and** all points where the function or its
   derivative is undefined. If you miss any of these points, you will probably end up
   with a wrong sign chart.

### Second Derivative Test

Suppose $f(x)$ is a function of $x$ that is twice differentiable at a *stationary point*
(minimum point, maximum point and inflection point) $x_0$, $f\prime(x_0) = 0$, then:

1. If $f\prime\prime(x_0) > 0$ then $f$ has a local minimum at $x_0$.
2. If $f\prime\prime(x_0) < 0$ then $f$ has a local maximum at $x_0$.

## Absolute extrema

An **absolute maximum** point is a point where the function obtains its greatest
possible value. Similarly, an **absolute minimum** point is a point where the function
obtains its least possible value.

These extreme values are obtained, either on a relative extremum point within the
interval, or on the endpoints of the interval.

## Extreme value theorem

If a function is continuous on a closed interval $[a,b]$, then the function must have a
maximum and a minimum on the interval.

> **Formal definition**
>
> If $f$ is continuous on a closed interval $[a, b]$, then $f$ attains an absolute
> maximum value $f(c)$ and an absolute minimum value $f(d)$ at some numbers $c$ and
> $d$ in $[a, b]$.

## Concavity

Concave up if the $f\prime$ increases, concave down if $f\prime$ decreases.

| **Concave up**             | equivalent to |                        |
| -------------------------- | :-----------: | ---------------------- |
| $g\prime(x)$ is increasing |      =>       | $g\prime\prime(x) > 0$ |

| **Concave down**           | equivalent to |                        |
| -------------------------- | :-----------: | ---------------------- |
| $g\prime(x)$ is decreasing |      =>       | $g\prime\prime(x) < 0$ |

## Inflection point (拐点)

Inflection points are points where the function changes concavity, i.e., from being
"concave up" (the slope of the function - $f\prime$ is increasing) to "concave down"
(the slope of the function - $f\prime$ is decreasing) or vice versa. They can be found
by considering where **the second derivative changes signs**.

Inflection points will occur when the second derivative is either zero or undefined.
Even if critical points are found, they can only be considered as candidates before
actually testing them.

Finding inflection points is similar to finding extreme points. Instead of looking for
points where the derivative changes its sign, we are looking for points where the
*second derivative* changes its sign.

$f\prime$'s (relative) minimum or maximum point is possibly inflection point.