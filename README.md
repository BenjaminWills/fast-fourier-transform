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

A fourier series with a period of $T$ and thus a frequency of $f=\frac{1}{T}$ is given by:

$$
\begin{align}
f(t) = \sum^{\infty}_{m = 0}{a_m \cos{(\frac{2 \pi m t}{T})}} + \sum^{\infty}_{n = 0}{b_n \sin{(\frac{2 \pi m t}{T})}}
\end{align}
$$

where $a_m,b_n$ are the `coefficients` of the fourier series, that determine relative weights for each wave (i.e the amount that each frequency contributes!)

The challenge now becomes how can we find these co-efficients?

Well, a **very** important fact is that sinusoidal waves multiplied together with non equal frequencies effectively cancel out, one could view this as the 'exactness' of the match.

$case \ 1: n \neq m$

$$
\begin{align*}
\begin{gather*}
\int{sin(mt)sin(nt)}dt =\frac{1}{2} \int{cos(t(n-m)) - cos(t(n+m))}dt \\
= \frac{sin(t(n-m))}{2(n-m)} - \frac{sin(t(n+m))}{2(n+m)} + C
\end{gather*}
\end{align*}
$$

$$
\begin{align*}
\begin{gather*}
\int{cos(mt)cos(nt)}dt =\frac{1}{2} \int{cos(t(n-m)) + cos(t(n+m))}dt \\
= \frac{sin(t(n-m))}{2(n-m)} + \frac{sin(t(n+m))}{2(n+m)} + C
\end{gather*}
\end{align*}
$$

$case \ 2: n = m$

$$
\begin{align*}
\begin{gather*}
\int{sin^2{(nt)}}dt =\frac{1}{2} \int{1-cos{(2nt)}}dt \\
=\frac{t}{2} -\frac{1}{4n}sin(2nt)
\end{gather*}
\end{align*}
$$

$$
\begin{align*}
\begin{gather*}
\int{cos^2{(nt)}}dt =\frac{1}{2} \int{cos{(2nt)+1}}dt \\
= \frac{1}{4n}sin(2nt) + \frac{t}{2}
\end{gather*}
\end{align*}
$$

You may be thinking... Why do I need to know this? To answer that we must dive into the `inner product`. This is an idea from linear algebra that essentially finds the similarity between two things in a `vector space`. You may be familiar with the `dot product` for vectors, well that is an example of an inner prodcut. The space of continuously differentiable functions is also a vector space, thus has an inner product defined on it.

The inner product over the interval $[a,b]$ is defined as follows:

$$
\begin{align*}
\langle f,g\rangle = \int^{b}_{a}{f(x)g(x)dx}
\end{align*}
$$

Two functions are `orthogonal` IF $\langle f,g\rangle = 0$.
