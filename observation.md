# Numerical Observations and Analysis

This document summarizes the numerical behavior, validation results, and optimizer performance observed in the implementation of the Physics-Informed Neural Network (PINN) for solving the two-dimensional Fisher–KPP reaction–diffusion equation.

---

## 1. Training Convergence Behavior

The composite physics-informed loss function consists of three components:
* **PDE residual loss:** Enforces the governing equation.
* **Initial condition loss:** Enforces $u(x, y, 0) = 10$.
* **Neumann boundary condition loss:** Enforces $\nabla u \cdot \mathbf{n} = 0$.

During training with the **Adam optimizer** over 5000 epochs, the total loss decreases steadily, indicating stable gradient-based learning. The resampling of collocation and boundary points at each epoch prevents computational graph reuse and enhances training robustness.



The decreasing loss confirms that:
- The neural network successfully minimizes the interior PDE residual.
- The initial condition is enforced accurately.
- The zero-flux Neumann boundary constraints are properly satisfied.

---

## 2. Validation via Logistic Reduction

To validate correctness, we consider the spatially uniform reduction. When spatial derivatives vanish, the PDE reduces to the logistic ordinary differential equation:

$$\frac{du}{dt} = u(1 - u)$$

The analytical solution is:
$$u(t) = \frac{1}{1 + Ce^{-t}}$$

Using the prescribed initial condition $u(0) = 10$, the constant evaluates to $C = -0.9$. The trained PINN solution was evaluated at the spatial center $(x, y) = (0, 0)$ and compared against this benchmark. 

The **Relative $L^2$ Error** is computed as:

\text{Relative } L_2 \text{ Error} = \frac{\| u_{\text{PINN}} - u_{\text{exact}} \|_2}{\| u_{\text{exact}} \|_2}



The observed error was significantly small, confirming the correct implementation of automatic differentiation and accurate temporal evolution.

---

## 3. Spatial Heatmap Analysis

A spatial snapshot was evaluated at time $t = 0.5$ to analyze consistency. Since the analytical solution is spatially uniform, the predicted field is expected to be constant across the domain.



**Observations:**
- **Uniformity:** The PINN heatmap shows a near-uniform spatial distribution.
- **Error:** The absolute error heatmap exhibits minimal deviations across the domain.
- **Stability:** No significant boundary artifacts or localized instabilities were detected, confirming the Neumann boundary conditions are correctly enforced.

---

## 4. Optimizer Comparison: Adam vs. L-BFGS

The training process utilized a two-stage approach:
1. **Stage 1:** Adam optimizer for rapid initial convergence.
2. **Stage 2:** L-BFGS refinement via a closure-based quasi-Newton approach.

**Observations:**
- **Adam** provided stable training dynamics and reached a solid baseline.
- **L-BFGS** successfully leveraged second-order curvature to further reduce the residual.
- The temporal profile after L-BFGS refinement exhibited superior alignment with the analytical solution compared to Adam alone.

---

## 5. Stability and Numerical Behavior

The combination of **Tanh activation functions**, **automatic differentiation**, and **epoch-wise resampling** contributed to a robust approximation. We observed no evidence of:
- Gradient explosion or vanishing gradients.
- Boundary layer oscillations.
- Spectral bias artifacts or training stagnation.

---

## 6. Overall Interpretation

The PINN framework successfully captures nonlinear logistic reaction dynamics and diffusive spatial smoothing. The close agreement with the analytical solution validates both the mathematical formulation and the implementation. The results align with established scientific machine learning literature, proving that combining Adam and L-BFGS yields high-precision solutions.

---

## 7. Concluding Observation

The numerical experiments confirm that the implemented PINN architecture provides a consistent, stable, and research-grade solution framework for the 2D Fisher–KPP equation. This approach is highly extensible to higher-dimensional systems and more complex nonlinear PDE models.
