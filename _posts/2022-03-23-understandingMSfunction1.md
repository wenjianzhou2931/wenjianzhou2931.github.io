---
layout: article
title: Notes of reading Understanding the Masking-Shadowing Function paper Part I
mathjax: true
---

## Section 2

### 2.1 Measuring Radiance on a Surface


$$
L(\omega_o, \mathcal{M}) = \frac{\int_{\mathcal{M}}\mathrm{projected \  area}(p_m)L(\omega_o, p_m)d p_m}{\int_{\mathcal{M}}\mathrm{projected \ area}(p_m)d p_m} \tag{1}
$$


The denominator is a normalization coefficient, and it gives the expression radiance units.

### 2.2 Microfacet Statistics

*The Distribution of Normals*: $$D(\omega) = \int_{\mathcal{M}}\delta_{\omega}(\omega_m(p_m))d p_m$$, the unit is $$(m^2 / sr)$$, square meters per steradian.

It provides the convertion from spatial to statistical integrals -- relate integrals over the microsurface to integrals over the sphere.

Thus the integral of $$D$$ is the area of the microsurface:


$$
\mathrm{microsurface \ area} = \int_{\mathcal{M}}dp_m = \int_{\Omega}D(\omega_m)d\omega_m \tag{2}
$$


### 2.3 Microfacet Projections

The statistical equation of masking function $$G$$ is given by


$$
\mathrm{projected \ area} = \int_{\Omega}G_1(\omega_o, \omega_m)\langle \omega_o, \omega_m \rangle D(\omega_m) d\omega_m \tag{3}
$$


$$D(\omega_m)$$ returns the differential area oriented to direction $$\omega_m$$, the clamped dot product ($$\langle \omega_o, \omega_m \rangle$$) projects the differential area to direciton $$\omega_o$$, and then times $$G_1$$ to get the fraction of visible area.

### 2.4 Constraint on the Masking Function

the projected area is acutally 1 $$m^2$$, so equation (3) can be written as


$$
\cos{\theta_o} = \int_{\Omega}G_1(\omega_o, \omega_m)\langle \omega_o, \omega_m \rangle D(\omega_m) d\omega_m \tag{4}
$$


where $$\theta_o$$ is the angle between the outgoing direction and geometric normal.

But $$G_1$$ has many cases, because we don't know what the microsurface looks like, so we need to choose a *microsurface profile*, which describes how the microfacets are organized.

## Section 3

### 3.1 Distribution of Visible Normals

Distribution of Visible Normals is the distribution of normals weighted by the projected area of each normal:


$$
D_{\omega_o}(\omega_m) = \frac{G_1(\omega_o, \omega_m)\langle \omega_o, \omega_m \rangle D(\omega_m)}{\cos{\theta_o}} \tag{5}
$$


$$\cos{\theta_o}$$ is the normalization factor

### 3.2 Construction of the BRDF

The radiance $$L(\omega_o, \omega_m)$$ of each microfacet can be expressed by the micro-BRDF $$\rho_{\mathcal{M}}(\omega_o, \omega_i, \omega_m)$$:


$$
L(\omega_o, \omega_m) = \int_{\Omega_{i}}\rho_{\mathcal{M}}(\omega_o, \omega_i, \omega_m) \langle \omega_i, \omega_m \rangle L(\omega_i) d\omega_i \tag{6}
$$


Differentiate the radiance on a microsurface:


$$
dL(\omega_o, \mathcal{M}) = \int_{\Omega}dL(\omega_o, \omega_m)D_{\omega_o}(\omega_m)d\omega_m \tag{7}
$$


substitute with equation (6):


$$
dL(\omega_o, \mathcal{M}) = L(\omega_i)d\omega_i \int_{\Omega}\rho_{\mathcal M}(\omega_o, \omega_i, \omega_m)\langle \omega_i, \omega_m \rangle D_{\omega_o}(\omega_m)d\omega_m \tag{8}
$$


and the macro-BRDF is:


$$
dL(\omega_o, \mathcal M) = \rho(\omega_o, \omega_i) \cos{\theta_i}L(\omega_i)d\omega_i \tag{9}
$$


so


$$
\rho(\omega_o, \omega_i) = \frac{1}{\cos{\theta_o}\cos{\theta_i}}\int_{\Omega}\rho_{\mathcal M}(\omega_o, \omega_i, \omega_m)\langle \omega_o, \omega_m \rangle \langle \omega_i, \omega_m \rangle G_1(\omega_o, \omega_m)D(\omega_m)d\omega_m \tag{10}
$$


but this equation only describes single scattering in microsurface, multiple bounces have to be removed, it is achieved by a *shadowing function*, which is called $$G_2$$


$$
\rho(\omega_o, \omega_i) = \frac{1}{|\omega_g \cdot \omega_o||\omega_g \cdot \omega_i|}\int_{\Omega}\rho_{\mathcal M}(\omega_o, \omega_i, \omega_m)\langle \omega_o, \omega_m \rangle \langle \omega_i, \omega_m \rangle G_2(\omega_o, \omega_i, \omega_m)D(\omega_m)d\omega_m \tag{11}
$$


### 3.3 Construction of the BRDF with specular microfacets

because the micro-BRDF for mirror-like microfacets is 


$$
\begin{aligned}
\rho_{\mathcal M}(\omega_o, \omega_i, \omega_m) &= \| \frac{\partial \omega_h}{\partial \omega_i} \| \frac{F(\omega_o, \omega_h) \delta_{\omega_h}(\omega_m)}{|\omega_i \cdot \omega_h |}  \\
&= \frac{F(\omega_o, \omega_h)\delta_{\omega_h}(\omega_m)}{4 |\omega_i \cdot \omega_h |^{2}}
\end{aligned} \tag{12}
$$


substitude into equation (11), we arrive at


$$
\rho(\omega_o, \omega_i) = \frac{1}{|\omega_g \cdot \omega_o||\omega_g \cdot \omega_i|}\int_{\Omega}\frac{F(\omega_o, \omega_h)\delta_{\omega_h}(\omega_m)}{4 |\omega_i \cdot \omega_h |^{2}}\langle \omega_o, \omega_m \rangle \langle \omega_i, \omega_m \rangle G_2(\omega_o, \omega_i, \omega_m)D(\omega_m)d\omega_m \tag{13}
$$


the delta function $$\delta_{\omega_h}(\omega_m)$$ allows a replacement at $$\omega_m = \omega_h$$, and after doing some cancellation:


$$
\rho(\omega_o, \omega_i) = \frac{F(\omega_o, \omega_h)G_2(\omega_o, \omega_i, \omega_h)D(\omega_h)}{4|\omega_g \cdot \omega_o||\omega_g \cdot \omega_i|} \tag{14}
$$


this is the well-known equation for specular microfacet-based BRDFs

### 3.4 Construction of the BRDF with diffuse microfacets

the micro-BRDF for diffuse case is $$\rho_{\mathcal M} = \frac{1}{\pi}$$

### 3.5 The BRDF Normalization Test

White Furnace Test:


$$
\int_{\Omega_i}\rho(\omega_o, \omega_i)| \omega_g, \omega_i | d\omega_i = 1 \tag{15}
$$


However, **The White Furnace Test** cannot be used to validate common BRDF models, which incorporate only the first scattering event. So a less restrictive test must be proposed. This test verifies that the distribution of rays reflected just after the first bounce and before leaving the surface is normalized. And we achieve it by replacing masking-shadowing by masking alone, without Fresnel and shadowing, the BRDF becomes:


$$
\rho(\omega_o, \omega_i) = \frac{G_1(\omega_o, \omega_h)D(\omega_h)}{4|\omega_g \cdot \omega_o||\omega_g \cdot \omega_i|} \tag{16}
$$


and substitude it into equation (15), the *Weak White Furnace Test* equation is:


$$
\int_{\Omega_i}\frac{G_1(\omega_o, \omega_h)D(\omega_h)}{4|\omega_g \cdot \omega_o|}d\omega_i = 1
$$


## Reference

* Heitz, Eric. "Understanding the masking-shadowing function in microfacet-based BRDFs." *Journal of Computer Graphics Techniques* 3.2 (2014): 32-91.



