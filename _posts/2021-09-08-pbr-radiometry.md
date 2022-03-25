---
layout: article
title: Radiometry
mathjax: true
---

Radiometry provides a set of mathematical tools that describe light **propagation** and **reflection**.

Its basic quantitie are:

* flux
* irradiance / radiant exitance
* intensity
* radiance

## Energy

Begin with energy, which is measured in joules ($$ J $$). A photon at wavelength $$\lambda$$ carries energy


$$
Q = \frac{hc}{\lambda}
$$


where $$c$$ is the speed of light, $$ h $$ is Planck's constant, $$ h \approx 6.626 \times 10^{-34} m^2 kg/s $$.

**Energy measures work over some period of time.**

## Flux / Power

Flux is the total amount of energy passing through a surface or region of space per unit time. By taking the limit of differential energy per differential time:


$$
\Phi = \lim_{\Delta t \to 0}\frac{\Delta Q}{\Delta t} = \frac{dQ}{dt}
$$


Conversely, given a flux as a function of time, we can integrate over a range of times:


$$
Q = \int_{t_0}^{t_1}\Phi(t)dt
$$



## Irradiance and Radiant Exitance

the area density of flux arriving at a surface: $$ E = \Phi / A$$

so $E$ is the power per unit area

more generally, $$E(p) = \lim_{\Delta A \to 0} \frac{\Delta \Phi(p)}{\Delta A} = \frac{d \Phi(p)}{dA}$$

integrate this formular:


$$
\Phi = \int_{A} E(p) dA
$$


Lambert's Law:

![Fig5.7](https://www.pbr-book.org/3ed-2018/Color_and_Radiometry/Lamberts%20Law.svg)


$$
E_2 = \frac{\Phi \cos{\theta}}{A_2}
$$



## Solid Angle and Intensity

2D case:

![Fig5.8](https://www.pbr-book.org/3ed-2018/Color_and_Radiometry/Planar%20angle.svg)

project the shaded object onto that circle, the arc $$ s $$ will be covered (the same as the angle $$\theta$$)

3D case:

![Fig5.9](https://www.pbr-book.org/3ed-2018/Color_and_Radiometry/Solid%20angle.svg)

project it onto the sphere, the area will be covered, which is the same as **solid angle**. use $$\omega$$ to denote solid angle (normalized vector)



consider point $$p$$ as a light source, we can compute the angular density of emitted power, which is called **intensity**, denoted by $I$


$$
I = \frac{\Phi}{4 \pi} \space (unit \space sphere)
$$


more generally:


$$
I = \lim_{\Delta \omega \to 0}\frac{\Delta \Phi}{\Delta \omega} = \frac{d \Phi}{d \omega}
$$


and integrate it:


$$
\Phi = \int_{\Omega} I(\omega) d \omega
$$

## Radiance

measure irradiance with respect to solid angles


$$
L(p, \omega) = \lim_{\Delta \omega \to 0}\frac{\Delta E_{\omega}(p)}{\Delta \omega} = \frac{d E_{\omega}(p)}{d\omega}
$$


radiance is the flux density per unit area, per unit solid angle, defined by:


$$
L = \frac{d\Phi}{d \omega d A^{\bot}}
$$


where $$A^{\bot}$$ is the projected area

## Incident and Exitant Randiance Functions

![Fig5.11](https://www.pbr-book.org/3ed-2018/Color_and_Radiometry/Incident%20outgoing%20radiance.svg)

The function that describes the radiance arriving at point $$p$$ is denoted by $$L_{i}(p, \omega)$$, the function that describes the outgoing radiance is denoted by $$L_{o}(p, \omega)$$

**Note that the direction vector $$\omega$$ is oriented to point away from $$p$$**

## Luminance and Photometry

Photometry is the study of visible electromagnetic radiation in terms of its perception by the human visual system.

Luminance measures how bright a spectral power distribution appears to a human observer

## Working with Radiometric Integrals

Irradiance at a point $$p$$ with surface normal $$n$$ due to radiance over a set of directions $$\Omega$$ is


$$
E(p, n) = \int_{\Omega} L_{i}(p, \omega)|\cos\theta|d\omega
$$


![Fig5.12](https://www.pbr-book.org/3ed-2018/Color_and_Radiometry/Irradiance%20from%20radiance.svg)

## Integrals over Projected Solid Angle

The projected solid angle subtended by an object is the cosine-weighted solid angle that it subtends

![Fig5.13](https://www.pbr-book.org/3ed-2018/Color_and_Radiometry/Projected%20solid%20angle.svg)

projected solid angle is measure by 


$$
d\omega^{\bot} = |\cos\theta|d\omega
$$


So the irradiance integral over the hemisphere can be written as


$$
E(p, n) = \int_{H^{2}(n)}L_{i}(p, \omega)d\omega^{\bot}
$$


we can also compute the total **emitted** flux:


$$
\Phi = \int_{A}\int_{H^{2}(n)}L_{o}(p, \omega)\cos\theta d\omega dA \\ = \int_{A}\int_{H^{2}(n)}L_{o}(p, \omega)d \omega^{\bot} d A
$$



## Integrals over spherical coordinates

![Fig5.15](https://www.pbr-book.org/3ed-2018/Color_and_Radiometry/Sin%20dtheta%20dphi.svg)

**note: unit sphere**

in a unit sphere, the coordinate system is:


$$
x = \sin\theta \cos\phi \\y = \sin\theta \sin\phi \\ z = \cos\theta
$$


thus, $$d A = \sin\theta d\phi d\theta$$


$$
E(p, n) = \int^{2 \pi}_{0}\int^{\frac{\pi}{2}}_{0}L_{i}(p, \theta, \phi) \cos\theta \sin\theta d\theta d \phi
$$



## Integrals over Area

![Fig5.16](https://www.pbr-book.org/3ed-2018/Color_and_Radiometry/Differential%20solid%20angle%20of%20dA.svg)

clearly,


$$
d\omega = \frac{dA \cos \theta}{r^2}
$$


Therefore:


$$
E(p, n) = \int_{A}L \cos \theta_{i}\frac{\cos \theta_{o} d A}{r^2}
$$



## Surface Reflection

### The BRDF (bidirectional reflectance distribution function)

![Fig5.18](https://www.pbr-book.org/3ed-2018/Color_and_Radiometry/BRDF.svg)

BRDF describes how much incident light along $$\omega_{i}$$ is scattered from the surface in the direction $$\omega_{o}$$ (to the viewer)

Take the differential radiance at $$p$$


$$
dE(p, \omega_{i}) = L_{i}(p, \omega_{i}) \cos \theta_{i} d \omega_{i}
$$


 The differential amount of radiance reflected in the direction $$\omega_{o}$$ is proportional to the diff irradiance


$$
d L_{o}(p, \omega_{o}) \propto dE(p, \omega_{i})
$$


The constant of proportionality defines the BRDF is


$$
f_{r}(p, \omega_{o}, \omega_i) = \frac{dL_o (p, \omega_o)}{dE(p, \omega_i)} = \frac{d L_o (p, \omega_o)}{L_i (p, \omega_i) \cos \theta_{i} d\omega_i}
$$



#### TWO IMPORTANT QUALITIES

1. Reciprocity: $$f_r (p, \omega_i, \omega_o) = f_r (p, \omega_o, \omega_i)$$
2. Energy Conservation: the total energy reflected is less than or equal to the energy of incident light


$$
\int_{H^2 (n)}f_r (p, \omega_o, \omega_i) \cos \theta_i d\omega_i \leq 1
$$



### BTDF (bidirectional transmittance distribution function)

denoted by $$f_t(p, \omega_o, \omega_i)$$, but $$\omega_i$$ and $$\omega_o$$ are in opposite hemispheres around $$p$$



For convenience, denote BRDF and BTDF together as BSDF (bidirectional scattering distribution function): $$f(p, \omega_o, \omega_i)$$

so we have


$$
d L_o (p, \omega_o) = f(p, \omega_o, \omega_i) L_i (p, \omega_i) |\cos \theta_i| d\omega_i \\ \Rightarrow L_o(p, \omega_o) = \int_{S^2}f(p, \omega_o, \omega_i) L_i (p, \omega_i) |\cos \theta_i| d \omega_i
$$



### The BSSRDF

bidirectional scattering surface reflectance distribution function describes scattering from materials that exhibit a significant amout of subsurface light transport, denoted as $$S(p_o, \omega_o, p_i, \omega_i) = \frac{d L_o(p_o, \omega_o)}{d \Phi(p_i, \omega_i)}$$



$$\Phi(p_i, \omega_i)$$ means that the BSSRDF accounts for  **the hemisphere and the whole surface**

![Fig5.19](https://www.pbr-book.org/3ed-2018/Color_and_Radiometry/BSSRDF.svg)

the generalization of it requires integration over surface area and incoming direction (first integrate the hemisphere and then the surface)


$$
L_o(p_o, \omega_o) = \int_A \int_{H^2(n)}S(p_o, \omega_o, p_i, \omega_i)|\cos \theta_i|d \omega_i dA
$$
