---
layout: archive
title: "Numerics & Simulations"
permalink: /numerics/
author_profile: true
---

{% include base_path %}

## Mathematical Framework & Numerical Methods

### 1. The Dispersive-Hyperbolic Model
The following simulations are based on **depth-averaged models** that account for both dispersive effects and energy dissipation during wave breaking. The physical setup and variable definitions are illustrated below:
![Model Setup and Variable Definition](/images/numerics/setup.png)
*Figure 1: Definition of physical variables and coordinate system for the depth-averaged model.*

The governing system is formulated as follows:
$$
\begin{aligned}
    &\frac{\partial h}{\partial t}+\frac{\partial hU}{\partial x} = 0\\
    &\frac{\partial hU}{\partial t}+\frac{\partial}{\partial x}\left(hU^2+\frac{gh^2}{2}+h^3\varphi+hP\right)= \frac{\partial}{\partial x}\left(2\nu_Th\frac{\partial U}{\partial x}\right)-gh\frac{\partial b}{\partial x}\\
    &\frac{\partial hW}{\partial t}+\frac{\partial hUW}{\partial x} = \frac{3}{2}P+3\nu_T\frac{\partial U}{\partial x}\\
    &\frac{\partial hP}{\partial t}+\frac{\partial hUP}{\partial x} = -a_c^2\left(h\frac{\partial U}{\partial x}+2W\right)\\
		&\frac{\partial h\varphi}{\partial t}+\frac{\partial hU\varphi}{\partial x} = -\frac{2}{h}\langle P^r\rangle+\frac{4\nu_T}{h}\left(\frac{\partial U}{\partial x}\right)^2-\frac{8\nu_TW}{h^2}\frac{\partial U}{\partial x}
\end{aligned}
$$
**Where:**
* $h$: The water depth.
* $U, W$: Depth-averaged horizontal and vertical velocity, respectively.
* $P$: Depth-averaged non-hydrostatic pressure.
* $\varphi$: **Enstrophy**, tracking the vorticity magnitude in the fluid.

---

### 2. Numerical Implementation
To solve the resulting system of partial differential equations (PDEs), I implement high-order schemes designed for conservation laws:
* **Spatial Discretization:** [e.g., WENO (Weighted Essentially Non-Oscillatory)] schemes to handle sharp gradients and shock waves without spurious oscillations.
* **Temporal Integration:** RSA2(2,2,2) for second-order time accuracy.
* **Well-Balanced Property:** Ensuring the scheme preserves the "lake-at-rest" steady state over complex, non-flat bathymetry.
