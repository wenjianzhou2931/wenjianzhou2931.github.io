---
layout: article
title: Notes of reading Understanding the Masking-Shadowing Function paper Part II
mathjax: true
---

## Section 4

### 4.1 The Smith Microsurface Profile

A random set of microfacets rather than a continuous surface. (the right one)

![smith-profile](https://github.com/wenjianzhou2931/wenjianzhou2931.github.io/raw/main/images/understanding-smith-profile.png)

(Still have questions here)The intuition is that the normal is a local property, and the occlution responsible for masking is a distant property (?), the local normal and the probability of occlusion is not correlated (?)

Under this assumption, the masking function can be expressed like this:


$$
G_1(\omega_o, \omega_m) = G^{\mathrm{local}}_{1}(\omega_o, \omega_m)G^{\mathrm{dist}}_{1}(\omega_o)
$$


where the local masking function is a binary discard of backfacing microfacets.

So the derivation of it can be written as:


$$
\cos{\theta_o} = \int_{\Omega}G^{\mathrm{dist}}_{1}(\omega_o, \omega_m)\chi^{+}(\omega_o \cdot \omega_m) \langle \omega_o, \omega_m \rangle D(\omega_m)d\omega_m
$$


where $$G_{1}^{\mathrm{dist}}$$ can be taken out and the step function can be removed because of the clamped dot product. So the masking function is:


$$
G^{\mathrm{dist}}_{1}(\omega_o) = \frac{\cos{\theta_o}}{\int_{\Omega} \langle \omega_o, \omega_m \rangle D(\omega_m) d\omega_m}
$$


so the complete one is:


$$
G_1(\omega_o, \omega_m) = \chi^{+}(\omega_o \cdot \omega_m)\frac{\cos{\theta_o}}{\int_{\Omega} \langle \omega_o, \omega_m \rangle D(\omega_m) d\omega_m}
$$


This is the integral form of the **exact** masking function by **Ashikhmin et al**.

The Smith Masking Function is expressed as $$\frac{1}{1 + \Lambda(\omega_o)}$$. This $$\Lambda(\omega_o)$$ function is expressed as an integral over the slopes of the microsurface (how?). And it is often considered to be approximate.

So the Masking function can be expressed like:


$$
G_1(\omega_o, \omega_m) = \frac{\chi^{+}(\omega_o \cdot \omega_m)}{1 + \Lambda(\omega_o)}
$$


The Smith Masking function is generally accurate even on correlated surfaces(even under the assumption that the microfacets are not correlated), and there is no analytical solution to the correlated masking function.

### 4.2 The V-Cavity Microsurface Profile 

This model computes the scattering on separate microsurfaces and averages their contributions. Each microsurface has only two normals $$\omega_m = (x_m, y_m, z_m)$$ and $$\omega_m' = (-x_m, -y_m, z_m)$$ (the z component should be positive or the normal will be backfacing).

![v-profile](https://github.com/wenjianzhou2931/wenjianzhou2931.github.io/raw/main/images/understanding-v-profile.png)

The distribution of normals is therefore:


$$
D(\omega) = \frac{\delta_{\omega_m}(\omega)}{2\omega_m \cdot \omega_g} + \frac{\delta_{\omega_m'}(\omega)}{2\omega_m' \cdot \omega_g}
$$



To derive the masking term, we use the conservation of the visible projected area:


$$
\begin{aligned}
\cos{\theta_o} &= \int_{\Omega}G_1(\omega_o, \omega) \langle \omega_o, \omega \rangle D(\omega) d\omega \\
&= \frac{1}{2}G_1(\omega_o, \omega_m)\frac{\langle \omega_o, \omega_m \rangle}{\omega_m \cdot \omega_g} + \frac{1}{2}G_1(\omega_o, \omega_m')\frac{\langle \omega_o, \omega_m' \rangle}{\omega_m' \cdot \omega_g}
\end{aligned}
$$


There are two cases, the two normals are visible and there is no masking, the two $$G_1$$ functions are all equal to 1, the other case is that one normal is visible and the other is invisible, and we have:


$$
\cos{\theta_o} = \frac{1}{2}G_1(\omega_o, \omega_m)\frac{\langle \omega_o, \omega_m \rangle}{\omega_m \cdot \omega_g}
$$


solution is:


$$
G_1(\omega_o, \omega_m) = 2 \frac{\cos{\theta_o}(\omega_m \cdot \omega_g)}{\langle \omega_o, \omega_m \rangle}
$$


where the $$\cos{\theta_o}$$ can be written as $$\omega_o \cdot \omega_g$$.

the result can be expressed in one formula:


$$
G_1(\omega_o, \omega_m) = \min(1, 2 \frac{(\omega_m \cdot \omega_g)(\omega_o \cdot \omega_g)}{\langle \omega_o, \omega_m \rangle})
$$


For validation, we can use the normalization of the distribution of visible normals to verify.

But V-cavity model has a problem, which is at grazing angles, the directions $$\omega_i$$ reflected by the V-cavity microsurface are too low on average compared to the directions $$\omega_i$$ reflected by physical surfaces.

![v-problem](https://github.com/wenjianzhou2931/wenjianzhou2931.github.io/raw/main/images/understanding-v-problem.png)

## Reference

* Heitz, Eric. "Understanding the masking-shadowing function in microfacet-based BRDFs." *Journal of Computer Graphics Techniques* 3.2 (2014): 32-91.

