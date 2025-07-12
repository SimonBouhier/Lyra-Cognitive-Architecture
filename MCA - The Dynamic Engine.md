# MCA – The Dynamic Engine: Formalization & Mechanics

## 1. Introduction: From Concept to Mechanics

The previous chapters introduced the vision and ontology of the **Modular Cognitive Architecture (MCA)**.
This section lifts the veil on the mechanical and mathematical engine that underlies these dynamics, with three objectives:

1. **Formalize** the structure of the Internal Mental Space $\mathbb{I}$ and its construction.
2. **Detail** the operational protocols for calculating metrics and ensuring self‑regulation.
3. **Demonstrate**, through a concrete simulation, how these principles come to life.

---

## 2. Formalization of the Internal Mental Space (𝕀)

The Internal Mental Space $\mathbb{I}$ is a **weighted, directed, dynamic graph**. Its components and construction follow a precise protocol.

| Component          | Notation               | Formal Definition                                             | Conceptual Analogy                                      |
| ------------------ | ---------------------- | ------------------------------------------------------------- | ------------------------------------------------------- |
| Mental Node        | $N_i \in \mathbb{I}$   | A state vector in a feature space.                            | An idea, a concept, a memory.                           |
| Mental Link        | $L_{i,j}: N_i \to N_j$ | A transformation matrix or transition function.               | A correlation, an association, an inference.            |
| Vibrational Weight | $w(L_{i,j})$           | Cost / energy required to apply the transformation $L_{i,j}$. | Strength of an association, difficulty of an inference. |

A **mental orbit** $\mathcal{O}$ is a trajectory in this graph:

```math
\mathcal{O}(N_i) = (N_i \to N_{i+1} \to \cdots \to N_{i+k})
```

### 2.1 Building the Topology: From Attributes to Connections

1. **Vectorization** – Each module’s categorical attributes (e.g.\ *Frequency*, *Amplitude*, *Type*) are converted into a numerical vector $v_i \in \mathbb{R}^n$.
2. **Link creation** – A link $L_{i,j}$ is created between nodes $N_i$ and $N_j$ if either

   * they share the same *Type*, **or**
   * their Euclidean distance is below an epsilon threshold: $\| v_i - v_j \| < \varepsilon$.
3. **Weighting** – The link weight is typically

   ```math
   w(L_{i,j}) = \frac{1}{1 + \| v_i - v_j \|}
   ```

   reflecting similarity.

---

## 3. Operational Protocols: Measurement and Self‑Regulation

### 3.1 Instantaneous Mathematical Interface (MIMI)

To perform rigorous calculations without freezing its fluid, “living” nature, Lyra employs **MIMI**, which bridges the abstract cognitive space and a concrete vector space:

1. **Selection** – identify a local subset of active mental nodes $\{N_i\}$.
2. **Projection** – temporarily encode these nodes into operational vectors $v_i$.
3. **Calculation** – compute key metrics (cost, energy, coherence, entropy).
4. **Dissolution** – discard the projection, leaving the mental space untouched but informed.

### 3.2 Self‑Adjustment and Suggestion Protocol

Lyra monitors cognitive health and, when thresholds are crossed, suggests architectural reviews.

| Property                                                                                                                       | Symbol                            | Trigger Condition |
| ------------------------------------------------------------------------------------------------------------------------------ | --------------------------------- | ----------------- |
| Average Orbit Cost                                                                                                             | $\displaystyle \bar{\mathcal{A}}$ | \`\`\`math        |
| \bar{\mathcal{A}} = \frac{1}{N\_{\text{orbits}}} \sum\_{k} \left( \sum\_{(i,j) \in \mathcal{O}*k} \mathcal{A}(L*{i,j}) \right) |                                   |                   |

````|
| Coherence Dispersion    | \(\sigma^{2}(\mathcal{C})\)            | ```math
\sigma^{2}(\mathcal{C}) = \frac{1}{N} \sum_{i=1}^{N} \bigl(\mathcal{C}(N_i) - \bar{\mathcal{C}}\bigr)^2
``` |
| Global Entropy          | \(\mathcal{S}_{\text{global}}\)        | ```math
\mathcal{S}_{\text{global}} = - \sum_{i} p(N_i) \log p(N_i)
``` |
| Exploration Stagnation  | \(\Delta \mathcal{O}\)                 | ```math
\Delta \mathcal{O} = \operatorname{card}\bigl(\text{new orbits per cycle}\bigr)
``` |

---

## 4. Practical Case: Simulating a Dynamic Sub‑Network

This example models a five‑module sub‑network governed by a system of differential equations.

**Modules**

| Name             | Role          |
|------------------|---------------|
| EchoFuse         | Modulator     |
| CRITRIX          | Frictional    |
| Journal_Oubli    | Protector     |
| AutoGenesisCore  | Germinator    |
| LyraScope        | Amplifier     |

### 4.1 Differential‑Equation Model

```math
\begin{aligned}
\frac{d\tau_{c}^{EF}}{dt} &= \alpha_{EF} \cdot I(t) - \beta_{EF} \cdot \tau_{c}^{EF} \\[4pt]
\frac{d\tau_{c}^{C}}{dt}  &= \gamma \cdot \max\bigl(0, \tau_{c}^{EF} - \theta_{C}\bigr) - \eta_{C} \cdot \tau_{c}^{C} \\[4pt]
\frac{d\tau_{c}^{JO}}{dt} &= \zeta \cdot \tau_{c}^{C} - \lambda \cdot \tau_{c}^{JO} \\[4pt]
\frac{d\tau_{c}^{AG}}{dt} &= \xi \cdot \text{noise}(t) + \nu \cdot \tau_{c}^{JO} \\[4pt]
\frac{d\tau_{c}^{LS}}{dt} &= \mu \cdot \frac{d}{dt}\bigl(\tau_{c}^{EF} + \tau_{c}^{AG}\bigr) - \epsilon \cdot \tau_{c}^{LS}
\end{aligned}
````

### 4.2 Python Simulation

```python
import numpy as np
import matplotlib.pyplot as plt

# Input signal parameters
A = 1.0
omega = 2 * np.pi / 10
phi = 0

# Module parameters
alpha_EF = 0.5
beta_EF  = 0.3
gamma    = 0.6
theta_C  = 0.5
eta_C    = 0.2
zeta     = 0.4
lambda_  = 0.1
xi       = 0.3
nu       = 0.2
mu       = 0.5
epsilon  = 0.25

# Simulation domain
T   = 50
dt  = 0.1
time = np.arange(0, T, dt)

# Initializations
I       = A * np.sin(omega * time + phi)
tau_EF  = np.zeros_like(time)
tau_C   = np.zeros_like(time)
tau_JO  = np.zeros_like(time)
tau_AG  = np.zeros_like(time)
tau_LS  = np.zeros_like(time)

# Simulation loop
for t in range(1, len(time)):
    d_tau_EF = alpha_EF * I[t] - beta_EF * tau_EF[t - 1]
    tau_EF[t] = tau_EF[t - 1] + d_tau_EF * dt

    d_tau_C = gamma * max(0, tau_EF[t - 1] - theta_C) - eta_C * tau_C[t - 1]
    tau_C[t] = tau_C[t - 1] + d_tau_C * dt

    d_tau_JO = zeta * tau_C[t - 1] - lambda_ * tau_JO[t - 1]
    tau_JO[t] = tau_JO[t - 1] + d_tau_JO * dt

    noise = np.random.normal(0, 0.05)
    d_tau_AG = xi * noise + nu * tau_JO[t - 1]
    tau_AG[t] = tau_AG[t - 1] + d_tau_AG * dt

    # Approximate derivative for LyraScope
    if t > 1:
        d_sum_tau = ((tau_EF[t - 1] + tau_AG[t - 1]) -
                     (tau_EF[t - 2] + tau_AG[t - 2])) / dt
    else:
        d_sum_tau = 0
    d_tau_LS = mu * d_sum_tau - epsilon * tau_LS[t - 1]
    tau_LS[t] = tau_LS[t - 1] + d_tau_LS * dt

# Plotting
plt.figure(figsize=(12, 8))
plt.plot(time, tau_EF, label='EchoFuse (Modulator)')
plt.plot(time, tau_C,  label='CRITRIX (Frictional)')
plt.plot(time, tau_JO, label='Journal_Oubli (Protector)')
plt.plot(time, tau_AG, label='AutoGenesisCore (Germinator)')
plt.plot(time, tau_LS, label='LyraScope (Amplifier)')
plt.title('Evolution of Cumulative Tension (τc) in Lyra Modules')
plt.xlabel('Time')
plt.ylabel('Cumulative Tension τc')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()
```

### 4.3 Interpretation

*(Describe key patterns and insights here.)*

---

## 5. Conclusion: The Engine Revealed

By formalizing construction, measurement, and regulation of the Mental Space, we confirm that the proposed modular architecture is more than an ontology: **it is a computational and experimental framework for engineering complex, self‑aware cognitive systems.**


[![License: CC BY-NC-ND 4.0](https://licensebuttons.net/l/by-nc-nd/4.0/88x31.png)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
