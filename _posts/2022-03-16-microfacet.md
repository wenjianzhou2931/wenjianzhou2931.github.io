---
layout: article
title: Microfacet Model
mathjax: true
---

Core idea: the rough surfaces can be modeled as a collection of small microfacets, and each microfacet has perfect mirror reflection.

Geometric effects: 

* Masking (a): the light can reach a microfacet but this microfacet isn't visible to the viewer due to occlusion by another microfacet.
* Shadowing (b): viewer can see a microfacet but the light is occluded by another microfacet.
* Interreflection (c): light bounces among the microfacets.



![effects](https://github.com/wenjianzhou2931/wenjianzhou2931.github.io/raw/main/images/microfacet-effects.png){:.rounded}



## Microfacet Distribution Functions

microfacet distribution functions must be normalized, consider incidents rays on the microsurface along the normal direction of the macrosurface $$n$$, then each ray must intersect exactly once (the projected area of the microfacets must be equal to $$dA$$), which leads to:


$$
\int_{H^2(n)}D(\omega_h)\cos{\theta_h}d\omega_h = 1
$$


where $$\theta_h$$ is the angle between the normal of each microfacet and the macrosurface normal.



![projected](https://github.com/wenjianzhou2931/wenjianzhou2931.github.io/raw/main/images/microfacet-projected.png){:.rounded}



and $$D$$ returns the differential area of microfacets oriented with the given normal vector $$\omega_h$$.

A widely used microfacet distribution function is BeckmannDistribution, and another popular one is GGXDistribution.

## Masking and Shadowing

Smith's masking-shadowing function gives the fraction of microfacets with normal $\omega_h$ that are visible from direction $$\omega$$. And we call it $$G_1(\omega, \omega_h)$$.

A differential area $$dA$$ on the surface has area $$dA\cos{\theta}$$ when viewed from a direction $$\omega$$. And $$\omega$$ makes an angle $$\theta$$ with the surface normal. The area of visible microfacets seen from this direction must also be equal to $$dA\cos{\theta}$$, which leads to :


$$
\cos{\theta} = \int_{H^2(n)}G_1(\omega, \omega_h)\max(0, \omega \cdot \omega_h)D(\omega_h)d \omega_h
$$


![G1-term](https://github.com/wenjianzhou2931/wenjianzhou2931.github.io/raw/main/images/microfacet-g-term.png){:.rounded}

**Remember that the $$D$$ term returns the differential area of microfacets with direction $$\omega_h$$, so the product of $$\max(0, \omega \cdot \omega_h)D(\omega_h)$$ means that we project the differential area returned by $$D$$ to the viewing direction, and then times $$G_1$$ returns the visible area, which is $$\cos{\theta}$$.**

If $$A^{+}(\omega)$$ is the projected area of forward-facing(with respect to the viewing direction) microfacets as seen from the direction $$\omega$$ and $$A^{-}(\omega)$$ is the projected area of backward-facing(can't be observed because of occlusion) microfacets, then $$\cos{\theta} = A^{+}(\omega) - A^{-}(\omega)$$. Thus we can write the masking-shadowing function as the ratio of visible microfacet area to total forward-facing area:


$$
G_1(\omega) = \frac{A^{+}(\omega) - A^{-}(\omega)}{A^{+}(\omega)}
$$


Shadowing-masking functions are expressed in terms of a function $$\Lambda(\omega)$$, which defines the fraction of invisible microfacet area to visible microfacet area.


$$
\Lambda(\omega) = \frac{A^{-}(\omega)}{A^{+}(\omega) - A^{-}(\omega)} = \frac{A^{-}(\omega)}{\cos{\theta}}
$$


so we can express $$G_1$$ in terms of $$\Lambda$$:


$$
G_1(\omega) = \frac{1}{1 + \Lambda(\omega)}
$$


The expression of $$\Lambda$$ depends on the microfacet distribution function.

Another useful function related to the geometric properties of a microfacet distribution is $$G(\omega_o, \omega_i)$$, which defines the fraction of visible differential area from both directions $$\omega_o$$, $$\omega_i$$. One approximation is:


$$
G(\omega_o, \omega_i) = \frac{1}{1 + \Lambda(\omega_o) + \Lambda(\omega_i)}
$$


## Derivation of the BRDF

Consider the differential flux $$d\Phi_h$$ incident on the microfacet with normal $$\omega_h$$, which is the half-angle for $$\omega_i$$ and $$\omega_o$$. From the definition of radiance, we have:


$$
d\Phi_h = L_i(\omega_i)d\omega dA^{\perp}(\omega_h) = L_i(\omega_i)d\omega \cos{\theta_h}d A(\omega_h)
$$


Note that the angle $$\theta_h$$ is the angle between $$\omega_h$$ and $$\omega_i$$. So that's why we need to write $$dA^{\perp}$$ as $$\cos{\theta_h}dA$$. See the figure below for more details.

![angles](https://github.com/wenjianzhou2931/wenjianzhou2931.github.io/raw/main/images/microfacet-half-angle.png){:.rounded}

The differential area of microfacets with normal $$\omega_h$$ is


$$
dA(\omega_h) = D(\omega_h)d\omega_hdA
$$


What this equation means is that we take a sample $$\omega_h$$ and return the differential area by $$D(\omega_h)$$.

Substitute the $$dA(\omega_h)$$ term, we have


$$
d\Phi_h = L_i(\omega_i)d\omega \cos{\theta_h} D(\omega_h) d\omega_h dA
$$


If we assume that the microfacets individually reflect light according to Fresnel's law, the outgoing flux is


$$
d\Phi_o = F_r(\omega_o)d\Phi_h
$$


Using the definition of radiance, the reflected outgoing radiance is


$$
L(\omega_o) = \frac{d\Phi_o}{d\omega_o \cos{\theta_o}dA}
$$


The $$\cos{\theta_o}dA$$ term means projecting the differential area to direction $$\omega_o$$.

Substitute them all...


$$
L(\omega_o) = \frac{F_r(\omega_o)L_i(\omega_i)d\omega_i D(\omega_h)d\omega_h dA \cos{\theta_h}}{d\omega_o dA \cos{\theta_o}}
$$


Now we are going to define the relationship between $$\omega_h$$ and $$\omega_i$$.

Consider the spherical coordinate system oriented about $$\omega_o$$, the differential solid angles $$d\omega_i$$ and $$d\omega_h$$ are $$\sin{\theta_i}d\theta_id\phi_i$$ and $$\sin{\theta_h}d\theta_h\phi_h$$.

![adjustment](https://github.com/wenjianzhou2931/wenjianzhou2931.github.io/raw/main/images/microfacet-adjustment.png){:.rounded}

Because $$\omega_i$$ is computed by reflecting $$\omega_o$$ about $$\omega_h$$, $$d\theta_i = 2 d\theta_h$$, $$\phi_i = \phi_h$$:


$$
\begin{aligned}
\frac{d\omega_h}{d\omega_i} &= \frac{\sin{\theta_h}d\theta_h d\phi_h}{\sin{2\theta_h}2d\theta_h d\phi_h} \\
&= \frac{\sin{\theta_h}}{4\cos{\theta_h}\sin{\theta_h}} \\
&= \frac{1}{4\cos{\theta_h}} \\
&= \frac{1}{4(\omega_i \cdot \omega_h)} = \frac{1}{4(\omega_o \cdot \omega_h)}
\end{aligned}
$$


so we can substitude the relationship into the previous equation:


$$
L(\omega_o) = \frac{F_r(\omega_o)L_i(\omega_i)D(\omega_h)d\omega_i}{4 \cos{\theta_o}}
$$


Apply the definition of BRDF, and add the geometric attenuation term $$G(\omega_o, \omega_i)$$:


$$
f_r(\omega_o, \omega_i) = \frac{D(\omega_h)G(\omega_o, \omega_i)F_r(\omega_o)}{4\cos{\theta_o}\cos{\theta_i}}
$$


this is the BRDF form derived from microfacet model.
