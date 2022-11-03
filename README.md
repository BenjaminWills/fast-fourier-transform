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
