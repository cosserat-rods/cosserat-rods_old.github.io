---
title: Single Rods
category: Cosserat Rods &mdash; Theory
order: 1
---

Cosserat rods are a generalization of Kirchhoff rods, which are able to model 1-d slender rods incorporating only bend and twist. Cosserat rods add the ability to consider stretching and shearing, allowing all the possible modes of deformation of the system to be considered. The **key assumption** of Cosserat rods are that their length is much larger than their radius (L >> r). This allows their dynamical behavior to approximated by averaging balance laws at every cross-section.  


## Mathematical Description of Cosserat Rods
>**TL;DR**: Cosserat rods are described by a centerline $\mathbf{r}(s, t)$ and local reference frame $\mathbf{Q}(s,t)= \\{ \bar{\mathbf{d}}_1, \bar{\mathbf{d}}_2, \bar{\mathbf{d}}_3 \\}$ which consists of a triad of orthonormal basis vectors. The dynamics of the rod are then described by the equations for conservation of linear and angular momentum throughout the rod.  

<img src="../../cosserat_rod_continuum.png" alt="image name" width="500"/>

For a Cosserat rod described by a centerline $\mathbf{r}(s, t)$ (where $s \in [0, L]$ is the arc-length of the rod and $t$ is time) we begin by defining two reference frames within which we can express a vector $\mathbf{x}$:  

The laboratory (Eulerian) frame: $\mathbf{x} = x_1 \mathbf{i} + x_2 \mathbf{j} + x_3 \mathbf{k}$  

The local (Lagrangian) frame: $\mathbf{x}_\mathcal{L} = x_1 \mathbf{d}_1 + x_2 \mathbf{d}_2 + x_3 \mathbf{d}_3$

We can define a rotation matrix $\mathbf{Q}$ between these two reference frames as $\mathbf{Q}= \\{ \bar{\mathbf{d}}_1, \bar{\mathbf{d}}_2, \bar{\mathbf{d}}_3 \\}$ allowing us to to convert between the laboratory and local reference frames via $\mathbf{x}=\mathbf{Q}\bar{\mathbf{x}}$ and $\bar{\mathbf{x}}=\mathbf{Q}^T \mathbf{x}$. The local frame describes the orientation of the rod where $\bar{\mathbf{d}}_3$ points along the centerline tangent ($\partial_s \bar{\mathbf{x}}=\bar{\mathbf{x}}_s=\partial_s \mathbf{r} = e \mathbf{t}$) when there is no shear while $\bar{\mathbf{d}}_1$ and $\bar{\mathbf{d}}_2$ span the normal-binormal plane. 

If there is shear or extension, then $\bar{\mathbf{d}}_3$ is shifted away from the tangent vector by some amount which can be described by a shear strain vector $\boldsymbol{\sigma}=\mathbf{Q} (\bar{\mathbf{x}}_s-\bar{\mathbf{d}}_3) = \mathbf{Q}\bar{\mathbf{x}}_s - \mathbf{d}_3$ in the *local* frame. When there is shear or extension, it is important to distinguish between the reference configuration ($\hat{s}$) and the deformed configuration ($s$). The stretch ratio between the two states is defined as $e=\frac{\mathrm{d} s}{\mathrm{d}\hat{s}}$. Along with shear strain, the rod can also twist and rotate. In the local (Lagrangian) reference frame, as we move along the rod, our reference frame $\mathbf{Q}(s,t)$ is continually changing. This change represents the curvature of the rod and is represented with a curvature vector $\boldsymbol{\kappa}$, which is defined as $\partial_s \mathbf{d}_j = \boldsymbol{\kappa} \times \mathbf{d}_j$. 

Now that we have established our reference frames, the next thing we need to do is define how we are going to track changes in the shape of the rod. The change in the $\mathbf{Q}(s,t)$ over time defines the angular velocity $\boldsymbol{\omega}$ of the rod ($\partial_t \mathbf{d}_j = \boldsymbol{\omega} \times \mathbf{d}_j$). We also want to define the translational velocity of each point of the centerline of the rod with a velocity $\bar{\mathbf{v}} = \partial_t\bar{\mathbf{x}} = \partial_t\mathbf{r}$.  **there are issues here with mixed notation**

Finally, there are a number of material and structural parameters that need to defined. These are generally defined in the reference ($\hat{s}$) configuration and are: 
* cross-sectional area $A$  
* mass per unit length $\rho$  
* bending stiffness matrix $\mathbf{B}$ 
* shearing stiffness matrix $\mathbf{S}$ 
* second area moment of inertia $\mathbf{I}$ 

The bending stiffness, shearing stiffness and second area moment of inertia can also be defined in terms of the shear strain, curvature, elastic modulus, shear modulus and internal forces and torques.

With all these terms defined, we can now define the balance laws that need to be satisfied at every cross-section. By balancing both linear and angular momentum at every cross-section we are able to describe the dynamics of the Cosserat rod. For a prescribed external force and couple line densities ($\bar{\mathbf{f}}$ and $\mathbf{c}$, respectively) the momentum balance equations are:

Linear Momentum: 
$\small{\rho A \cdot \partial_t^2 \bar{\mathbf{x}} = \overbrace{\partial_s \left( \frac{\mathbf{Q}^T \mathbf{S} \boldsymbol{\sigma}}{e} \right)}^{\substack{\text{internal shear/} \\\\ \text{stretch force}}} + \overbrace{e \ \bar{\mathbf{f}}}^{\substack{\text{external} \\\\ \text{force}}} }$

Angular Momentum: 
$\small{\begin{align} \frac{\rho \mathbf{I}}{e} \cdot \partial_t \boldsymbol{\omega} =&\overbrace{\partial_s \left( \frac{\mathbf{B} \boldsymbol{\kappa}}{e^3} \right) + \frac{\boldsymbol{\kappa} \times \mathbf{B} \boldsymbol{\kappa}}{e^3}}^{\text{internal bend/twist couple}} +\overbrace{\left( \mathbf{Q}\frac{\bar{\mathbf{x}}_s}{e} \times \mathbf{S} \boldsymbol{\sigma} \right)}^{\substack{\text{internal shear/} \\\\ \text{stretch couple}}} + \\\\&\underbrace{\left( \rho \mathbf{I} \cdot \frac{\boldsymbol{\omega}}{e} \right) \times \boldsymbol{\omega}}\_{\text{Lagrangian transport}}^{\phantom{o}} +\underbrace{\frac{\rho \mathbf{I} \boldsymbol{\omega}}{e^2} \cdot \partial_t e}\_\textrm{unsteady dilation} + \underbrace{e \ \mathbf{c}}\_{\substack{\text{external} \\\\ \text{couple}}} \end{align}}$

Now I am adding othe things


Solution of these equations describes 

## Useful References
<span style="color:#1F109D">*Lays out Cosserat rod theory*</span>: Gazzola, Dudte, McCormick, Mahadevan, **Forward and inverse problems in the mechanics of soft filaments**, Royal Society Open Science, 2018.  

Charles, Gazzola, Mahadevan, **Topology, geometry, and mechanics of strongly stretched and twisted filaments: solenoids, plectonemes, and artificial muscle fibers**, Physical Review Letters, 2019.  

A more complete list of publications about Cosserat rods may be found [here](../../4_publications/publications)