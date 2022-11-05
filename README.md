# The Fast Fourier Transform

In this repository I will be exploring and implementing the fast fourier transform algorithm. First let us discuss what a `fourier transform` is.

## Fourier Transforms

The idea of the `fourier transform` is that we should be able to decompose a wave into its individual components, it allows us to to find the frequencies and amplitudes that make up a function.

The intuition is that any signal can be made from a combination of circular paths with tracers on the circumference, so we aim to find the kinds of circular paths.

To do this we need to look at `Eulers formula` which is essentially the `polar form` of a `complex number`. Say that $z \in \Complex$ then if $z = x + iy$ we can write $z = re^{i \theta}$ where $r = \sqrt{x^2 + y^2}$ and $\tan{\theta} = \frac{y}{x}$. This form of $z$ allows us to view complex numbers in a circular way.

To describe a circular path we need 3 bits of information:

- Frequency (revolutions per time interval)
- Amplitude (radius of circle)
- Phase angle (starting point)

If we think of the wave traced by a point moving around a circle in time we have a sinusoidal wave, this is how sine and cosine waves are defined.

Fourier had the idea of placing multiple points with different frequencies, amplitudes and phase angles together around the same center.

So how might we go from a function to a sum of sinusoidal waves?

### Fourier Series

A fourier series with a period of $\pi$ and thus a frequency of $f=\frac{1}{\pi}$ on the interval $[-\pi,\pi]$ is given by:

$$
\begin{align}
f(t) = \sum^{\infty}_{m = 0}{a_m \cos{(mt)}} + \sum^{\infty}_{n = 0}{b_n \sin{(nt)}}
\end{align}
$$

where $a_m,b_n$ are the `coefficients` of the fourier series, that determine relative weights for each wave (i.e the amount that each frequency contributes!)

The challenge now becomes: how can we find these co-efficients?

Well, a **very** important fact is that sinusoidal waves multiplied together with non equal frequencies effectively cancel out, one could view this as the 'exactness' of the match.

You may be thinking... Why do I need to know this? To answer that we must dive into the `inner product`. This is an idea from linear algebra that essentially finds the similarity between two things in a `vector space`. You may be familiar with the `dot product` for vectors, well that is an example of an inner prodcut. The space of continuously differentiable functions is also a vector space, thus has an inner product defined on it.

The inner product over the interval $[a,b]$ is defined as follows:

$$
\begin{align*}
\langle f,g\rangle = \int^{b}_{a}{f(x)g(x)dx}
\end{align*}
$$

Two functions are `orthogonal` (aka completely dissimilar or perpendicular)IF $\langle f,g\rangle = 0$. So finally linking back to $f(t)$ defined above we can take the

A function $f(x)$ is `normalised` if $\langle f,f\rangle=1$, which can be easily achieved if we just write $\hat{f}(x) = \frac{f(x)}{\sqrt{\langle f,f\rangle}}$ then $\langle \hat{f},\hat{f}\rangle=\frac{\langle f,f\rangle}{\langle f,f\rangle} = 1$ for any function.

It is a fact that on the interval $[-\pi,\pi]$: $cos(x)$ and $sin(x)$ are `orthogonal`, thus we can find our coefficients, $a_n,b_n$ on this interval. Meaning that the set $\{sin(x), \dots,sin(nx),cos(x),\dots,cos(nx)\}$ forms an orthogonal basis (so everything is orthogonal to eachother pairwise in this set) on the set of functions (on the interval $[-\pi,\pi]$). This basis is orthonormal if every element of the set is multiplied by $\frac{1}{\sqrt{\pi}}$, since then each inner product of a function with itself in the set equals one. So why is this important?

NOTE: we can extend this interval of estimation to $[-L,L]$ by substituting $x$ for $x = \frac{\hat{x}\pi}{L}$ then $\forall \hat{x} \in [-L,L]$ the set $\{\frac{1}{\sqrt{L}}cos(\frac{n\hat{x} \pi}{L}),\frac{1}{\sqrt{L}} sin(\frac{n\hat{x} \pi}{L}): n \in \N \}$ forms an orthonormal basis on $[-L,L]$. You can check this as an exercise!

So if we now define $f(t)$ on the interval $[-L,L]$ using this above note:

$$
\begin{align}
f(t) = \sum^{\infty}_{m = 0}{a_m \cos{(\frac{m\pi t}{L})}} + \sum^{\infty}_{n = 0}{b_n \sin{(\frac{n\pi t}{L})}}
\end{align}
$$

Lets finally use this orthonormal basis that we've found to find $a_n,b_m$, to do this we use our old friend: the inner product.

$$
\begin{align*}
\langle f(t),\frac{1}{\sqrt{L}} cos(\frac{k \pi t}{L})\rangle =
\langle \sum^{\infty}_{m = 0}{a_m \cos{(\frac{m\pi t}{L})}} + \sum^{\infty}_{n = 0}{b_n \sin{(\frac{n\pi t}{L})}},\frac{1}{\sqrt{L}} cos(\frac{k \pi t}{L})\rangle \\
= \langle \sum^{\infty}_{m = 0}{a_m \cos{(\frac{m\pi t}{L})}},\frac{1}{\sqrt{L}} cos(\frac{k \pi t}{L})\rangle \\
= \sum^{\infty}_{m = 0}\langle {a_m \cos{(\frac{m\pi t}{L})}},\frac{1}{\sqrt{L}} cos(\frac{k \pi t}{L})\rangle \\
= \sum^{\infty}_{m = 0}\frac{a_m}{\sqrt{L}}\langle { \cos{(\frac{m\pi t}{L})}}, cos(\frac{k \pi t}{L})\rangle
\\
=\sqrt{L} \sum^{\infty}_{m = 0} \delta_{k,m}a_m = \sqrt{L}a_k
\end{align*}
$$

Equivalently we have that (since we know $f(t)$ remember)

$$
\begin{align*}
\langle f(t),\frac{1}{\sqrt{L}} cos(\frac{k \pi t}{L})\rangle =\frac{1}{\sqrt{L}} \int^{L}_{-L}{f(t)cos(\frac{k \pi t}{L})}dt
\end{align*}
$$

Hence finally we have that

$$
\begin{align}
a_k = \frac{1}{L} \int^{L}_{-L}{f(t)cos(\frac{k \pi t}{L})}dt
\end{align}
$$

and by similar reasoning we have that

$$
\begin{align}
b_k = \frac{1}{L} \int^{L}_{-L}{f(t)sin(\frac{k \pi t}{L})}dt
\end{align}
$$

So lets just summarise, the `inner product` finds how similar two functions are - and hence is perfect to find the frequencies that our function is made of, we then used this to find an expansion of our function in terms of sinusoudal waves.

We can find everything at once by calculating

$$
\begin{align}
a_k + ib_k = \frac{1}{L} \int^{L}_{-L}{f(t)e^{i\frac{k \pi t}{L}}}dt
\end{align}
$$
