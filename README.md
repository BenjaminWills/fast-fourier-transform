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
