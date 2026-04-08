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
![Model Setup](/images/numerics/setup.png){: style="width: 50%; display: block; margin: 0 auto;"}

*Figure 1: Definition of physical variables and coordinate system for the depth-averaged model.*
{: .text-center}

The governing system is formulated as follows:
<center>
$$
\begin{aligned}
    &\frac{\partial h}{\partial t}+\frac{\partial hU}{\partial x} = 0\\
    &\frac{\partial hU}{\partial t}+\frac{\partial}{\partial x}\left(hU^2+\frac{gh^2}{2}+h^3\varphi+hP\right)= \frac{\partial}{\partial x}\left(2\nu_Th\frac{\partial U}{\partial x}\right)-gh\frac{\partial b}{\partial x}\\
    &\frac{\partial hW}{\partial t}+\frac{\partial hUW}{\partial x} = \frac{3}{2}P+3\nu_T\frac{\partial U}{\partial x}\\
    &\frac{\partial hP}{\partial t}+\frac{\partial hUP}{\partial x} = -a_c^2\left(h\frac{\partial U}{\partial x}+2W\right)\\
		&\frac{\partial h\varphi}{\partial t}+\frac{\partial hU\varphi}{\partial x} = -\frac{2}{h}\langle P^r\rangle+\frac{4\nu_T}{h}\left(\frac{\partial U}{\partial x}\right)^2-\frac{8\nu_TW}{h^2}\frac{\partial U}{\partial x}
\end{aligned}
$$
</center>
**Where:**
* $$h$$: The water depth.
* $$U, W$$: Depth-averaged horizontal and vertical velocity, respectively.
* $$P$$: Depth-averaged non-hydrostatic pressure.
* $$\varphi$$: **Enstrophy**, tracking the vorticity magnitude in the fluid.
* $$a_c$$: Acoustic sound velocity (for the pressure wave) (chosen value).
* $$\langle P^r\rangle$$: Energy dissipation due to wave breaking.
* $$\nu_T$$: Turbulent viscosity.

*Note: $\langle P^r\rangle$ and $\nu_T$ are functions of enstrophy $\varphi$.*

---

### 2. Improved Dispersive Properties

The dispersive properties is improved adapting the method of **Bonneton et al. (2011)**, where instead of the depth-averaged vertical velocity, one uses 
<center>
	$$W^\ast = w\vert_{z=b+\frac{\alpha}{2}h}$$.
</center>

The improved governing system is then written as:

<center>
$$
\begin{aligned}
    &\frac{\partial h}{\partial t}+\frac{\partial hU}{\partial x} = 0\\
    &\frac{\partial hU}{\partial t}+\frac{\partial}{\partial x}\left(hU^2+\frac{gh^2}{2}+h^3\varphi+hP\right)= \frac{\partial}{\partial x}\left(2\nu_Th\frac{\partial U}{\partial x}\right)-gh\frac{\partial b}{\partial x}\\
    &\frac{\partial hW^\ast}{\partial t}+\frac{\partial hUW^\ast}{\partial x} = \frac{3}{2}P+3\frac{\nu_T}{\alpha}\frac{\partial U}{\partial x}+4\frac{\alpha-1}{\alpha^2}{W^{\ast}}^2+\frac{\alpha-1}{2\alpha}gh^2\frac{\partial S}{\partial x}\\
    &\frac{\partial hP}{\partial t}+\frac{\partial hUP}{\partial x} = -a_c^2\left(\alpha h\frac{\partial U}{\partial x}+2W^\ast\right)\\
	&\frac{\partial hS}{\partial t}+\frac{\partial hUS}{\partial x} = 2h\frac{\partial W^\ast}{\partial x}+\frac{2}{\alpha}W^\ast S\\
	&\frac{\partial h\varphi}{\partial t}+\frac{\partial hU\varphi}{\partial x} = -\frac{2}{h}\langle P^r\rangle+\frac{4\nu_T}{h}\left(\frac{\partial U}{\partial x}\right)^2-\frac{8\nu_TW}{\alpha^2h^2}\frac{\partial U}{\partial x}
\end{aligned}
$$
</center>
**Where:**
* $$S$$: Analogue to the gradient of $$Z$$.
* $$\alpha$$: A constant affecting the dispersive properties (an optimal value of $1.159$ was found by Bonneton et al., 2011).

---

### 3. Numerical Implementation
To solve numerically the system of partial differential equations (PDEs), I implement second-order schemes designed for conservation laws:
* **Spatial Discretization:** Monotone Upstream Centered Scheme for Conservation Laws (MUSCL) with Rusanov flux and without slope limiter.
* **Temporal Integration:** Diagonally-implicit Runge-Kutta (DIRK) Implicit-Explicit (IMEX) ARS2(2,2,2) for second-order time accuracy.
* **Well-Balanced Property:** Ensuring the scheme preserves the "lake-at-rest" steady state over complex, non-flat bathymetry.
