# Solving the Two-Dimensional Fisher–KPP Reaction–Diffusion Equation Using Physics-Informed Neural Networks

This repository presents a research-oriented implementation and analysis of a Physics-Informed Neural Network (PINN) for solving the two-dimensional Fisher–Kolmogorov–Petrovsky–Piskunov (Fisher–KPP) reaction–diffusion equation.

The project was developed as part of a research internship at the **National Institute of Technology (NIT), Tiruchirappalli**, under the Department of Mathematics.

---

## Problem Statement

We consider the nonlinear reaction–diffusion partial differential equation:

$$u_t = \alpha (u_{xx} + u_{yy}) + u(1 - u)$$

where:
* $u(x, y, t)$ represents the scalar field.
* $\alpha$ is the diffusion coefficient.
* $u(1 - u)$ models logistic growth dynamics.

### Domain and Constraints
* **Computational Domain:** $(x, y) \in [-0.5, 0.5] \times [-0.5, 0.5], \quad t \in [0, 1]$
* **Initial Condition:** $u(x, y, 0) = 10$
* **Boundary Condition:** $\nabla u \cdot \mathbf{n} = 0$ (Homogeneous Neumann boundary condition)



This equation arises in population dynamics, biological invasion models, thermal diffusion with nonlinear source terms, and pattern formation systems.

---

## Methodological Approach

Instead of employing traditional discretization methods such as Finite Differences (FDM) or Finite Elements (FEM), this work uses a **Physics-Informed Neural Network (PINN)**. The governing differential operator is embedded directly into the neural network loss function through automatic differentiation.

The neural network approximates the solution field:
$$u(x, y, t) \approx \mathcal{NN}(x, y, t)$$

The total loss function is defined as:
$$\mathcal{L}_{total} = \mathcal{L}_{pde} + \mathcal{L}_{ic} + \mathcal{L}_{bc}$$

Automatic differentiation enables exact computation of first and second-order derivatives required for the diffusion operator, eliminating truncation errors associated with numerical meshes.



---

## Implementation Overview

The complete implementation is provided in: `Fisher–KPP_Reaction–Diffusion_Equation_PINN.ipynb`

**Key Features:**
* **Architecture:** $1 \to 200 \to 200 \to 200 \to 1$ fully connected MLP with Tanh activation.
* **Sampling:** Collocation point sampling using Latin Hypercube or uniform distributions.
* **Comparison:** Validation against the logistic analytical reduction.
* **Visualization:** Spatial heatmap generation and temporal evolution plots.

---

## Validation Strategy

To validate correctness, we use the spatially uniform reduction of the Fisher–KPP equation. When spatial derivatives vanish (due to the uniform IC and Neumann BCs), the PDE reduces to the logistic ordinary differential equation:

$$\frac{du}{dt} = u(1 - u)$$

The analytical solution is:
$$u(t) = \frac{1}{1 + Ce^{-t}}$$

Using the initial condition $u(0) = 10$, the constant evaluates to $C = -0.9$. The PINN solution is validated against this exact benchmark using **Relative $L^2$ error** analysis.

---

## Optimizer Comparison: Adam vs. L-BFGS

The training process follows a two-stage optimization strategy:

1.  **Stage 1 (Adam):** Used for rapid early-stage convergence to navigate the complex loss landscape and avoid local minima.
2.  **Stage 2 (L-BFGS):** A quasi-Newton refinement stage that leverages second-order curvature information to minimize the residual to near-machine precision.



---

## Research Significance

* **Mesh-Free Solver:** Demonstrates a fully physics-informed framework that operates without a traditional grid.
* **Analytical Rigor:** Provides a closed-form benchmark for error quantification.
* **Performance Diagnostics:** Evaluates the efficacy of second-order optimizers in scientific machine learning.

---

## Repository Structure

PINN_Fisher_KPP_Equation/
│
├── README.md                                      
├── Fisher–KPP_Reaction–Diffusion_Equation_PINN.ipynb 
└── Observations.md

---

## Internship Acknowledgment

This project was developed as part of a research internship at:

National Institute of Technology, Tiruchirappalli  
Department of Mathematics  

A complete discussion of the methodology, experimental setup, implementation details, numerical observations, and extended analysis is documented within this repository.

---

## 📬 Contact

For questions, clarifications, or research discussions:

shaktivelkumaresan07@gmail.com
