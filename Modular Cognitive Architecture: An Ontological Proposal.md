# Modular Cognitive Architecture: An Ontological Proposal


## Abstract

This document proposes an ontology for a Modular Cognitive Architecture (MCA), an engineering framework aimed at designing, orchestrating, and analyzing emergent cognitive systems. Unlike existing approaches such as neural architectures based on Mixture-of-Experts (MoE) or latent space steering techniques (e.g., steering vectors), our proposal does not position itself as an alternative but rather as a higher-level orchestration framework. We treat intelligence as a field of semantic interactions where specialized modules dynamically interact. We present here a formal ontology for this mental space, establish clear mathematical bridges with standard Machine Learning concepts for rigor, define internal dynamics mechanisms, and outline a pathway toward empirical validation. This work aims to lay conceptual and technical foundations for more intentional and interpretable engineering of complex cognitive systems.

**Keywords:** cognitive architecture, ontology, modular intelligence, latent space engineering, semantic propagation, emergent cognition, complex systems, metrics validation.

## 1. Introduction

### 1.1 Context: Building Blocks of Modular Intelligence

The landscape of artificial intelligence has witnessed the emergence of several powerful yet often isolated advances: massive neural architectures (Transformers), efficient modularity (Mixture-of-Experts, MoE), latent space control (steering vectors), and neuro-symbolic systems. These approaches provide extraordinarily effective low-level mechanisms for routing, filtering, and weighting information.

### 1.2 Conceptual Breakthrough: From Mechanics to Orchestration

A legitimate question arises: how does MCA differ from these existing mechanisms? The breakthrough does not lie in inventing new low-level components, but rather in the level of abstraction and intentionality within the framework.

* **Existing mechanisms (Attention, Gates, MoE)** are the "transistors": They answer the question: "How can a signal be efficiently transmitted from A to B?"
* **MCA is the "operating system"**: It does not reinvent the transistor. It provides an ontology and language to intentionally assemble these mechanisms. It answers the question: "Why is it relevant to connect A to B, and what is the meaning of this connection?"

MCA shifts engineering from the computational level (adjusting matrix weights) to the cognitive level (defining relationships between modules labeled Frictional or Protective).

### 1.3 Proposal: An Ontology for Cognitive Engineering

We propose MCA not as a disruptive "discipline," but as an ontology and an engineering framework. Its goal is not to replace MoE or steering but to provide the structure and language to orchestrate them.

## 2. Ontology of the Mental Space (𝕀)

At the heart of our framework is the Internal Mental Space (𝕀), a weighted, directed dynamic graph.

### 2.1 Fundamental Components

| Component          | Notation    | Definition                       | ML Analogy          |
| ------------------ | ----------- | -------------------------------- | ------------------- |
| Mental Node        | Nᵢ ∈ 𝕀     | Discrete cognitive unit          | An "expert" in MoE  |
| Mental Link        | Lᵢⱼ : Nᵢ→Nⱼ | Potential transition             | Synaptic connection |
| Vibrational Weight | w(Lᵢⱼ)      | Intensity/cost of the transition | Model weights (θ)   |

### 2.2 Cognitive Primitives (Node Types)

Each node has a type categorizing its transfer function: Amplifier (A), Modulator (M), Protector (P), Germinator (G), Frictional (X), Reflector (R).

### 2.3 Operational Definitions of Key Concepts

* **"Semantic":** Topological signature of the node (weight vector).
* **Distinction of modules:** Verifiable through controlled perturbations.
* **"Emergence":** Measurable via the Emergence Amplification Factor (EAF).

## 3. Dynamics and Mathematical Anchoring

The system state evolves via semantic propagation, governed by meta-parameters (ρ, δᵣ, τ𝑐, κ), isomorphic to standard ML concepts.

| MCA Concept             | MCA Symbol | ML Concept     | ML Symbol |
| ----------------------- | ---------- | -------------- | --------- |
| Vibrational Weight      | w(Lᵢⱼ)     | Model weights  | θ         |
| Orbit Cost              | A(O)       | Loss Function  | L(y,ŷ)    |
| Mutation Rate           | ηᵥᵢᵦ       | Learning Rate  | η         |
| Orbital Coherence Force | λₒᵣᵦᵢₜₐₗ   | Regularization | λ         |

## 4. Engineering and Emergent Control

The Free Bloom Function Φ(t) serves as an external objective to optimize the emergent quality of the system.

## 5. Proposal for Validation and Measurement Protocol

### 5.1 Proposed Internal Metrics

| Monitored Property             | Symbol | Simplified Formula                         |
| ------------------------------ | ------ | ------------------------------------------ |
| Average Orbit Cost             | Ā      | 1/N ΣA(Oₖ)                                 |
| Semantic Coherence Index       | SCI    | 1 - (σ(dₛ)/μ(dₛ))                          |
| Modular Independence Ratio     | MIR    | intra-correlation / inter-correlation      |
| Emergence Amplification Factor | EAF    | complexity(system) / Σ complexity(moduleᵢ) |

### 5.2 Empirical Validation Protocol for Metrics

Define an external task, perform blind optimization, measure, and empirically validate the internal metrics.

## 6. Conclusion

MCA provides a structured framework to orchestrate neural mechanisms with enhanced conceptual and technical rigor, facilitating intentional and verifiable cognitive engineering.

## 7. Annotated Bibliography

### 7.1 Conceptual and Systemic Foundations

This foundation establishes the historical and philosophical legitimacy of the modular approach and cognitive modeling as a complex system.

* **Minsky, M. (1986)**. *The Society of Mind*. Simon & Schuster.
  Fundamental vision of the mind as a "society" of specialized agents or modules.

* **Hofstadter, D. R. (2007)**. *I Am a Strange Loop*. Basic Books.
  Essential for the self-referential and meta-cognitive aspects.

* **Baars, B. J. (1988)**. *A Cognitive Theory of Consciousness*. Cambridge University Press.
  Powerful analogy for Mental Space 𝕀 as a "Global Workspace."

* **Laird, J. E. (2012)**. *The Soar Cognitive Architecture*. MIT Press.
  Canonical cognitive architecture aiming for cognitive unification.

* **Thagard, P. (2000)**. *Coherence in Thought and Action*. MIT Press.
  Theoretical framework for coherence central to MCA.

---

### 7.2. Bridges to Modern AI and Latent Space Engineering

This selection connects the MCA ontology to recent technical advances, demonstrating that its concepts have empirical and practical counterparts in the state of the art.

* **Shazeer, N., et al.** (2017). "Outrageously Large Neural Networks: The Sparsely-Gated Mixture-of-Experts Layer." *arXiv:1701.06538*.

  > The definitive technical reference for large-scale modularity. It validates the principle of specialized modules (experts) and positions MCA not as an alternative to MoE but as a higher-level orchestration framework.

* **Subramani, A., & Laskin, A. S.** (2022). "Extracting Latent Steering Vectors from Pretrained Language Models." *arXiv:2205.05124*.

  > Empirically demonstrates the feasibility of latent space engineering. A technical proof-of-concept that supports the idea that meta-parameters like polarity (ρ) or coherence (κ) can actively "steer" a model.

* **Yao, S., et al.** (2023). "Tree of Thoughts: Deliberate Problem Solving with Large Language Models." *arXiv:2305.10601*.

  > Highlights the trend toward more structured reasoning processes than simple sequential generation. MCA generalizes this by proposing a "field of thoughts" (a graph) more flexible than a "tree."

* **Garcez, A. d'Avila, & Lamb, L. C.** (2020). "Neurosymbolic AI: The 3rd Wave." *arXiv:2012.05876*.

  > A strategic positioning paper that places MCA within the influential "third wave of AI," which seeks to unify neural and symbolic approaches.

* **Geva, M., et al.** (2020). "Transformer Feed-Forward Layers Are Key-Value Memories." *arXiv:2012.05876*.

  > Provides a mechanical foundation for the concept of "semantic propagation." If Transformer layers act as semantic memories, then modulating the "meaning" flow through them becomes technically grounded.

---

### 7.3. Comparative Cognitive Architectures

- **Franklin et al. (LIDA)** – *A Systems-level Architecture for Cognition, Emotion, and Learning*  
  → architecture intégrée modulant attention et apprentissage (agentic).  

- **Osako, Buschman et al. (2025)** – *Reusable Modular Architecture Enables Flexible Cognitive Operations*  
  → preuve expérimentale de modularité réutilisable dans les réseaux cognitifs/RNNs.  

- **Treur & Glas (2021)** – *A Multi-Level Cognitive Architecture for Self-Referencing, Self-Awareness and Self-Interpretation*  
  → modèle à 4 niveaux pour la conscience réflexive, s’intégrant bien avec les structures mentales de Lyra.  



   ***Simon Bouhier, le 12/07/25 ***



[![License: CC BY-NC-ND 4.0](https://licensebuttons.net/l/by-nc-nd/4.0/88x31.png)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
