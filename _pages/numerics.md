---
layout: archive
title: "Numerics & Simulations"
permalink: /numerics/
author_profile: true
---

{% include base_path %}

<style>
  html { scroll-behavior: smooth; }
  h2, h3 { scroll-margin-top: 80px; } /* Avoid the sidebar be covered by the Header */

  /* Main part */
  #side-nav {
    position: fixed; /* move with scrolling */
    top: 80px;       
    right: 0;          
    width: 250px;      
    background: white;
  	border: 1px solid #ddd;
    border-radius: 8px 0 0 8px; /* round corner */
    box-shadow: -2px 2px 10px rgba(0,0,0,0.1);
    z-index: 1000;     /* highest priority */
    transition: transform 0.3s ease; /* animation */
    transform: translateX(0); /* default: expand */
  }

  #side-nav.minimized {
    transform: translateX(250px);
  }

  #side-nav-content {
    padding: 15px;
    overflow: hidden;
  }

  /* --- Triangle toggle tab --- */
  #nav-toggle-tab {
    position: absolute;
    left: -15px;      /* left side of the menu */
    top: 50%;          /* centering */
    transform: translateY(-50%);
    width: 15px;       
    height: 40px;      
    background: #BABABA; 
    color: white;
    border-radius: 8px 0 0 8px; 
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.6em;
    box-shadow: -2px 0 5px rgba(0,0,0,0.1);
  }

  /* triangle icon */
  #toggle-icon {
    transition: transform 0.3s ease;
    transform: rotate(0deg); 
  }

  #side-nav.minimized #toggle-icon {
    transform: rotate(180deg); 
  }

  #side-nav ul {
    list-style: none;
    padding: 0;
    margin: 0;
    font-size: 0.9em;
    line-height: 2;
  }
  #side-nav ul li a {
    text-decoration: none;
    color: #333;
    display: block;
    padding: 2px 5px;
  }
  #side-nav ul li a:hover {
    background-color: #f1f1f1;
    color: #007bff;
  }

  html[data-theme='dark'] #side-nav {
  background: #1e1e1e;       
  border-color: #333;        
  box-shadow: -2px 2px 10px rgba(0,0,0,0.5);
  }

  html[data-theme='dark'] #side-nav-content strong {
  color: #ffffff;            
  }

  html[data-theme='dark'] #side-nav ul li a {
  color: #bbbbbb;           
  }

  html[data-theme='dark'] #side-nav ul li a:hover {
  background-color: #333333; 
  color: #007bff;           
  }

  html[data-theme='dark'] #nav-toggle-tab {
  background: #ababab;       
  color: #ffffff;
  }

  /* --- Toggle dark mode --- */
  html[data-theme='dark'] #breaking-toggle-container {
  background: #6b6b6b !important;
  border-color: #454d5d !important;
  color: #eeeeee;
  }

  html[data-theme='dark'] #eq-no-breaking, 
  html[data-theme='dark'] #eq-breaking {
  background: #474747 !important;
  border: 1px solid #a6a6a6 !important;
  }

  video {
    cursor: pointer;
    transition: opacity 0.2s;
  }

  video:active {
    opacity: 0.8;
  }
</style>

<div id="side-nav">
  	<div id="nav-toggle-tab" onclick="toggleSideNav()" title="Toggle Navigation">
    	<span id="toggle-icon">
			►
		</span>
 	</div>
  	<div id="side-nav-content">
    	<strong style="display: block; margin-bottom: 10px; font-size: 1.1em;">
			Contents
		</strong>
    	<ul>
      		<li style="margin-bottom: 5px;">
		  		<a href="#mathematical-framework--numerical-methods" style="text-decoration: none; color: #007bff;">
			  		Framework
		  		</a>
	  		</li>
      		<li style="margin-bottom: 5px;">
		  		<a href="#simulations" style="text-decoration: none; color: #007bff;">
			  		Simulations
		  		</a>
		  		<ul style="list-style: none; padding-left: 12px; margin-top: 5px; border-left: 1px solid #eee; font-size: 0.85em;">
          			<li style="margin-bottom: 5px;">
            			<a href="#1-solitary-wave-propagation-over-flat-bed" style="text-decoration: none; color: #999;">
							Solitary Wave (Flat Bed)
						</a>
            		</li>
            		<li style="margin-bottom: 5px;">
            			<a href="#2-solitary-wave-propagation-over-a-slope" style="text-decoration: none; color: #999;">
							Solitary Wave (Slope)
						</a>
            		</li>
            		<li style="margin-bottom: 5px;">
            			<a href="#3-wavetrain-propagation" style="text-decoration: none; color: #999;">
							Wave Train
						</a>
            		</li>
          		</ul>
	  		</li>
    	</ul>  
  		<hr style="margin: 10px 0; border: 0; border-top: 1px solid #eee;">
  		<a href="#" style="font-size: 0.8em; color: #999; text-decoration: none;">
			↑ Back to Top
		</a>
  	</div>
</div>

<script>
function toggleSideNav() {
  const nav = document.getElementById('side-nav');
  nav.classList.toggle('minimized');
}
</script>

## Mathematical Framework & Numerical Methods

### 1. The Dispersive-Hyperbolic Model
<details markdown="1">
<summary style="cursor: pointer; color: #007bff; font-weight: bold;">
  Click to view Governing Equations & Variable Definitions
</summary>

<div style="margin-top: 15px; border-left: 3px solid #007bff; padding-left: 15px;" markdown="1">
	
 The physical setup and variable definitions are illustrated below:
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

*Note: $$\langle P^r\rangle = \langle P^r\rangle(\varphi)$$ and $$\nu_T = \nu_T(\varphi)$$ are functions of enstrophy $$\varphi$$.*
  
</div>
</details>

---

### 2. Improved Dispersive Properties
<details markdown="1">
<summary style="cursor: pointer; color: #007bff; font-weight: bold;">
  Click to view the model with improved dispersive properties
</summary>

<div style="margin-top: 15px; border-left: 3px solid #007bff; padding-left: 15px;" markdown="1">
	
The dispersive properties is improved adapting the method of **Bonneton et al., 2011**, where instead of the depth-averaged vertical velocity, one uses 
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
	&\frac{\partial h\varphi}{\partial t}+\frac{\partial hU\varphi}{\partial x} = -\frac{2}{h}\langle P^r\rangle+\frac{4\nu_T}{h}\left(\frac{\partial U}{\partial x}\right)^2-\frac{8\nu_TW^\ast}{\alpha^2h^2}\frac{\partial U}{\partial x}
\end{aligned}
$$
</center>
**Where:**
* $$S$$: Analogue to the gradient of $$Z$$.
* $$\alpha$$: A constant affecting the dispersive properties (an optimal value of $$1.159$$ was found by Bonneton et al., 2011).

</div>
</details>

---

### 3. Numerical Implementation
<details markdown="1">
<summary style="cursor: pointer; color: #007bff; font-weight: bold;">
  Click to view Numerical methods & Treatment of breaking
</summary>

<div style="margin-top: 15px; border-left: 3px solid #007bff; padding-left: 15px;" markdown="1">

To solve numerically the system of partial differential equations (PDEs), I implement second-order schemes designed for conservation laws:
* **Spatial Discretization:** Monotone Upstream Centered Scheme for Conservation Laws (MUSCL) with Rusanov flux and without slope limiter.
* **Temporal Integration:** Diagonally-implicit Runge-Kutta (DIRK) Implicit-Explicit (IMEX) ARS2(2,2,2) for second-order time accuracy.
* **Well-Balanced Property:** Ensuring the scheme preserves the "lake-at-rest" steady state over complex, non-flat bathymetry.
* **Treatment of breaking:** A novel breaking criterion related to the enstrophy was proposed by **Kazakova & Richard, 2019**, where a variable analogous to the enstrophy, called the virtual enstrophy $$\psi$$, is solved by an identical enstrophy equation in parallel with the original equations, to avoid introducing sudden strong discontinuity to the system when breaking happens.
<p>
  When there is 
  <span id="breaking-toggle-container" style="display: inline-block; background: #f9f9f9; padding: 4px 12px; border-radius: 20px; border: 1px solid #ddd; vertical-align: middle; white-space: nowrap;">
    <label style="display: inline-flex !important; align-items: center; cursor: pointer; font-size: 0.9em; margin-right: 15px; margin-bottom: 0; border: none;">
      <input type="radio" name="breaking-toggle" onclick="toggleBreaking('no')" checked style="display: inline-block !important; width: auto; margin: 0 5px 0 0; vertical-align: middle;">
      no breaking
    </label>
    
    <label style="display: inline-flex !important; align-items: center; cursor: pointer; font-size: 0.9em; margin-bottom: 0; border: none;">
      <input type="radio" name="breaking-toggle" onclick="toggleBreaking('yes')" style="display: inline-block !important; width: auto; margin: 0 5px 0 0; vertical-align: middle;">
      breaking
    </label>
  </span>, the model solves:
</p>

<div id="eq-no-breaking" style="display: block; background: #fff; padding: 15px; border-radius: 10px; border: 1px solid #eee;">
  <p align="center"><i>\(\varphi\) remains zero or constant while \(\psi\) tracks the potential breaking intensity.</i></p>
  $$
  \begin{aligned}
      &\frac{\partial h}{\partial t}+\frac{\partial hU}{\partial x} = 0\\
      &\frac{\partial hU}{\partial t}+\frac{\partial}{\partial x}\left(hU^2+\frac{gh^2}{2}+h^3\varphi+hP\right)= -gh\frac{\partial b}{\partial x}\\
      &\frac{\partial hW^\ast}{\partial t}+\frac{\partial hUW^\ast}{\partial x} = \frac{3}{2}P+4\frac{\alpha-1}{\alpha^2}{W^{\ast}}^2+\frac{\alpha-1}{2\alpha}gh^2\frac{\partial S}{\partial x}\\
    &\frac{\partial hP}{\partial t}+\frac{\partial hUP}{\partial x} = -a_c^2\left(\alpha h\frac{\partial U}{\partial x}+2W^\ast\right)\\
	&\frac{\partial hS}{\partial t}+\frac{\partial hUS}{\partial x} = 2h\frac{\partial W^\ast}{\partial x}+\frac{2}{\alpha}W^\ast S\\
    &\frac{\partial h\varphi}{\partial t}+\frac{\partial hU\varphi}{\partial x} = -\frac{2}{h}\langle P^r\rangle(\varphi) \\
    &\frac{\partial h\psi}{\partial t}+\frac{\partial hU\psi}{\partial x} = -\frac{2}{h}\langle P^r\rangle(\psi)+\frac{4\nu_T(\psi)}{h}\left(\frac{\partial U}{\partial x}\right)^2-\frac{8\nu_T(\psi)W^\ast}{\alpha^2h^2}\frac{\partial U}{\partial x}
  \end{aligned}
  $$
</div>

<div id="eq-breaking" style="display: none; background: #fff; padding: 15px; border-radius: 10px; border: 1px solid #eee;">
  <p align="center"><i>The production terms are activated for \(\varphi\) and both momentum equations to dissipate energy.</i></p>
  $$
  \begin{aligned}
      &\frac{\partial h}{\partial t}+\frac{\partial hU}{\partial x} = 0\\
      &\frac{\partial hU}{\partial t}+\frac{\partial}{\partial x}\left(hU^2+\frac{gh^2}{2}+h^3\varphi+hP\right)= {\color{red}{\frac{\partial}{\partial x}\left(2\nu_T(\varphi)h\frac{\partial U}{\partial x}\right)}}-gh\frac{\partial b}{\partial x}\\
      &\frac{\partial hW^\ast}{\partial t}+\frac{\partial hUW^\ast}{\partial x} = \frac{3}{2}P{\color{red}{+3\frac{\nu_T(\varphi)}{\alpha}\frac{\partial U}{\partial x}}}+4\frac{\alpha-1}{\alpha^2}{W^{\ast}}^2+\frac{\alpha-1}{2\alpha}gh^2\frac{\partial S}{\partial x}\\
      &\frac{\partial hP}{\partial t}+\frac{\partial hUP}{\partial x} = -a_c^2\left(\alpha h\frac{\partial U}{\partial x}+2W^\ast\right)\\
	  &\frac{\partial hS}{\partial t}+\frac{\partial hUS}{\partial x} = 2h\frac{\partial W^\ast}{\partial x}+\frac{2}{\alpha}W^\ast S\\
      &\frac{\partial h\varphi}{\partial t}+\frac{\partial hU\varphi}{\partial x} = -\frac{2}{h}\langle P^r\rangle(\varphi)\color{red}{+\frac{4\nu_T(\varphi)}{h}\left(\frac{\partial U}{\partial x}\right)^2-\frac{8\nu_T(\varphi)W}{\alpha^2h^2}\frac{\partial U}{\partial x}} \\
      &\frac{\partial h\psi}{\partial t}+\frac{\partial hU\psi}{\partial x} = -\frac{2}{h}\langle P^r\rangle(\psi)+\frac{4\nu_T(\psi)}{h}\left(\frac{\partial U}{\partial x}\right)^2-\frac{8\nu_T(\psi)W^\ast}{\alpha^2h^2}\frac{\partial U}{\partial x}
  \end{aligned}
  $$
</div>

<script>
function toggleBreaking(state) {
  const noBreak = document.getElementById('eq-no-breaking');
  const yesBreak = document.getElementById('eq-breaking');
  if (state === 'no') {
    noBreak.style.display = 'block';
    yesBreak.style.display = 'none';
  } else {
    noBreak.style.display = 'none';
    yesBreak.style.display = 'block';
  }
}
</script>

</div>
</details>

---

### 4. Breaking criterion
<details markdown="1">
<summary style="cursor: pointer; color: #007bff; font-weight: bold;">
  Click to view Breaking criterion
</summary>

<div style="margin-top: 15px; border-left: 3px solid #007bff; padding-left: 15px;" markdown="1">
	
To accurately determine the breaking zone, a revised breaking criterion is implemented to the 1D model based on the transition between two state-dependent quantities:
<center>
	\(\widetilde{\psi_\eta} := \dfrac{\psi\max(0,\eta)}{g}\) and \(\widetilde{\psi_h} := \dfrac{\psi h}{g}\)
</center>
The triggering mechanism is governed by two thresholds, $$\psi_0 > \psi_1 \geq 0$$, which define the activation and deactivation phases:

* **Activation**: Breaking is initiated when $$\widetilde{\psi_\eta} > \psi_0$$.
* **Persistence**: Once active, the breaking state is maintained as long as $$\widetilde{\psi_h} > \psi_1$$.
  
Furthermore, the location of turbulence generation and vortex generation for a plunging breaker is mainly in the front part of the wave. To ensure the numerical model reflects this spatial localization, a constraint is applied to the breaking zone:
<center>
$$\nu_T = 0 \quad \text{if} \quad
\begin{cases}
W < 0 & \text{(Weakly Dispersive Model)} \\
W^\ast < 0 & \text{(Improved Dispersive Model)}
\end{cases}$$
</center>
The thresholds were determined by fitting an empirical law to experimental data as a function of the bed slope $$\tan\beta$$.
</div>
</details>

## Simulations (Under construction)
The following simulations are based on the improved dispersive model. In the following simulation, the zone where the activation criterion is met is indicated by the translucent red zone. The true breaking region (not shown here), where dissipative terms are activated, is much narrower and localized at the wave front.

### 1. Solitary wave propagation over flat bed
In the experiment of **Watanabe et al., 2001**, the physical limit of solitary wave stability was investigated to determine the critical non-linearity at which a wave begins to break. The waves are characterized by the non-linearity parameter $$\mu = a/h$$, amplitude over water depth.

**Case A: Stable Solitary Wave ($$\mu = 0.66$$)**

<div style="text-align: center; margin: 20px 0;">
  <video id="stable-sim-vid" autoplay loop muted playsinline style="width: 85%; max-width: 800px; border-radius: 5px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); cursor: pointer;" title="Click to restart animation">
    <source src="/images/numerics/Stable_Solitary.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <p style="font-style: italic; color: #666; margin-top: 10px; padding: 0 10%;">
    Animation 1: Propagation of a stable solitary wave (\(\mu = 0.66\)). 
  </p>
</div>

**Case B: Unstable Solitary Wave ($$\mu = 0.78064$$)**

<div style="text-align: center; margin: 20px 0;">
  <video id="unstable-sim-vid" autoplay loop muted playsinline style="width: 85%; max-width: 800px; border-radius: 5px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); cursor: pointer;" title="Click to restart animation">
    <source src="/images/numerics/Unstable_Solitary.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <p style="font-style: italic; color: #666; margin-top: 10px; padding: 0 10%;">
    Animation 2: Spontaneous breaking of an unstable solitary wave (\(\mu = 0.78064\)). 
  </p>
</div>

### 2. Solitary wave propagation over a slope
In the experiment of **Hsiao et al., 2008**, a solitary wave of amplitude $$a_\ast$$ is generated and propagates over the
topography of slope $$\beta$$ with $$\tan \beta = 1/60$$ at a given still water level $$h_0$$. The experiment consists of 54 trials with different choices of still water level $$h_0$$ and nonlinearity parameter $$\mu = a_\ast/h_0$$ from which the wave amplitude
can be calculated. The following simulation and gauge point comparison are from trial 41, where $$h_0 = 2.2$$ m and $$\mu = 0.137$$ were chosen.

<div style="text-align: center; margin: 20px 0;">
  <video id="solitary-slope-sim-vid" autoplay loop muted playsinline style="width: 85%; max-width: 800px; border-radius: 5px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); cursor: pointer;" title="Click to restart animation">
    <source src="/images/numerics/Hsiao_Trial41.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <p style="font-style: italic; color: #666; margin-top: 10px; padding: 0 10%;">
    Animation 3: Solitary wave \(\mu = 0.137\) propagation over a slope \(\tan \beta = 1/60\). 
  </p>
</div>

<div style="text-align: center; margin: 30px 0;">
  <img src="/images/numerics/Hsiao_Trial41.png" alt="Gauge point comparison Trial 41" style="width: 70%; max-width: 900px; border-radius: 5px; box-shadow: 0 4px 8px rgba(0,0,0,0.05);">
  <p style="font-style: italic; color: #666; margin-top: 10px; padding: 0 15%;">
    Figure 2: Time series of free surface elevation at multiple gauge locations for Trial 41. Solid red lines represent numerical results, and black circles/lines denote experimental data.
  </p>
</div>

### 3. Wavetrain propagation
This section presents two simulations of wave train propagation. The first case replicates the experiment by **Cox, 1995**, in which a wave train with $$a_\ast = 0.115$$ m and $$T = 2.2$$ s propagates over a $$1/35$$ slope. The six gauge locations are marked by solid vertical lines in the simulation below.

<div style="text-align: center; margin: 20px 0;">
  <video id="wavetrain-cox-sim-vid" autoplay loop muted playsinline style="width: 85%; max-width: 800px; border-radius: 5px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); cursor: pointer;" title="Click to restart animation">
    <source src="/images/numerics/WaveTrain_Cox.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <p style="font-style: italic; color: #666; margin-top: 10px; padding: 0 10%;">
    Animation 4: Wave train propagation over a slope \(\tan \beta = 1/35\). The solid vertical lines indicate the locations of the six gauges. A 7.5-meter-wide absorbing layer is applied to the right boundary of the domain.
  </p>
</div>

The time series of free surface elevation at the six gauge points are shown in the figure below.

<div style="text-align: center; margin: 30px 0;">
  <img src="/images/numerics/WaveTrain_Cox.png" alt="Gauge point comparison Cox" style="width: 70%; max-width: 900px; border-radius: 5px; box-shadow: 0 4px 8px rgba(0,0,0,0.05);">
  <p style="font-style: italic; color: #666; margin-top: 10px; padding: 0 15%;">
    Figure 3: Time series of free surface elevation at six gauge points. Solid red lines represent numerical results, and black circles denote experimental data.
  </p>
</div>

The second case focuses on the experiment of **Beji & Battjes, 1993**, in which regular sinusoidal and irregular wave trains are generated and propagate over a submerged bar. The regular sinusoidal wave train with period $$T = 2.5$$ s and amplitudes $$a_\ast = 0.022$$ m are generated (spilling breaker type). A 5 m sponge layer is applied on the right side of the domain to absorb outgoing waves and the incoming wave is generated on the left side as a Dirichlet boundary condition. The eight gauge locations are marked by solid vertical lines in the simulation below.

<div style="text-align: center; margin: 20px 0;">
  <video id="wavetrain-bejisls-sim-vid" autoplay loop muted playsinline style="width: 85%; max-width: 800px; border-radius: 5px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); cursor: pointer;" title="Click to restart animation">
    <source src="/images/numerics/WaveTrain_BejiSLS.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <p style="font-style: italic; color: #666; margin-top: 10px; padding: 0 10%;">
    Animation 5: Wave train propagation over a submerged bar. The solid vertical lines indicate the locations of the eight gauges. 
  </p>
</div>

The time series of free surface elevation at the eight gauge points are shown in the figure below.

<div style="text-align: center; margin: 30px 0;">
  <img src="/images/numerics/WaveTrain_BejiSLS.png" alt="Gauge point comparison Beji SLS" style="width: 70%; max-width: 900px; border-radius: 5px; box-shadow: 0 4px 8px rgba(0,0,0,0.05);">
  <p style="font-style: italic; color: #666; margin-top: 10px; padding: 0 15%;">
    Figure 4: Time series of free surface elevation at eight gauge points of the regular wave train. Solid red lines represent numerical results, and black circles denote experimental data.
  </p>
</div>

<script>
  (function() {
    var initVideos = function() {
      var ids = ['stable-sim-vid', 'unstable-sim-vid', 'solitary-slope-sim-vid', 'wavetrain-cox-sim-vid', 'wavetrain-bejisls-sim-vid'];
      ids.forEach(function(id) {
        var vid = document.getElementById(id);
        if (vid) {
		  if (id === 'solitary-slope-sim-vid' || id === 'wavetrain-bejisls-sim-vid') {
            vid.playbackRate = 1.5; 
          } else {
              vid.playbackRate = 0.75;
		  }
			
          vid.onclick = function() {
            this.currentTime = 0;
            this.play();
          };
          vid.onplay = function() {
            this.playbackRate = 0.75;
          };
        }
      });
    };

    if (document.readyState === 'complete') {
      initVideos();
    } else {
      window.addEventListener('load', initVideos);
    }
  })();
</script>
