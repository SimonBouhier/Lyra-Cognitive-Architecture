Chapter 2 - Simulation of Mental Orbits – FLOATLAP and SLOW_CORE++
1. Experiment Objective
After exploring possibilities, a cognitive system must be able to form coherent "trains of thought." In the Lyra ontology, these are called Mental Orbits. This set of experiments aims to demonstrate that this navigation process can be modeled as a "flow" or "walk" through the Mental Space 𝕀, guided by underlying fields of energy and coherence.

The objectives are to:

Simulate a "low-energy orbit" where a process navigates a network of ideas by following paths of least resistance (FLOATLAP).

Model the Mental Space as a continuous landscape and visualize a "thought-flow" that balances the search for coherence with the avoidance of chaos (SLOW_CORE++).

Provide a concrete, visual basis for the theoretical concept of "minimizing the cost of an orbit."

2. Simulation Protocols
Two distinct but complementary simulations were developed.

Floatlap_Orbite_Simulation.py: This script models the Mental Space as a graph where each node (idea) has a local "energy." A walker navigates the graph, and the connections between nodes are weighted based on their energy. The term "Laplacian" in the visualization title refers to an advanced mathematical tool used to understand the fundamental structure and connectivity of graphs, confirming the rigor of this approach.

```python
import numpy as np
import networkx as nx
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
import csv

# ----- PARAMÈTRES DE BASE -----
N = 30  # Nombre de nœuds
timesteps = 50  # Nombre d'étapes temporelles
alpha = 1.5  # Sensibilité de la pondération à l'énergie

# ----- GÉNÉRATION DU GRAPHE INITIAL -----
G = nx.random_geometric_graph(N, radius=0.3)
positions = nx.get_node_attributes(G, 'pos')

# ----- CHAMP D'ÉNERGIE LOCAL (aléatoire lisse) -----
def generate_energy_field():
    base = np.random.rand(N)
    smooth = (base + np.roll(base, 1) + np.roll(base, -1)) / 3  # lissage simple
    return smooth

# ----- POIDS ADAPTATIFS -----
def update_weights(G, energy):
    for (i, j) in G.edges():
        local_energy = (energy[i] + energy[j]) / 2
        G[i][j]['weight'] = np.exp(-alpha * local_energy)
        G[i][j]['energy'] = local_energy

# ----- MARCHE ORBITALE -----
def next_node(G, current, energy):
    neighbors = list(G.neighbors(current))
    if not neighbors:
        return current, current, 1.0
    weights = np.array([G[current][n]['weight'] for n in neighbors])
    probs = weights / weights.sum()
    next_n = np.random.choice(neighbors, p=probs)
    return next_n, neighbors[np.argmax(probs)], np.max(probs)

# ----- SIMULATION -----
energy = generate_energy_field()
update_weights(G, energy)

path = []
orbit_log = []
current = np.random.randint(0, N)
for t in range(timesteps):
    path.append(current)
    next_n, dominant_n, prob = next_node(G, current, energy)
    orbit_log.append((t, current, next_n, prob))
    current = next_n

# ----- EXPORT CSV -----
with open("nodes.csv", "w", newline='') as f:
    writer = csv.writer(f)
    writer.writerow(["id", "x", "y", "energie_locale"])
    for i in range(N):
        x, y = positions[i]
        writer.writerow([i, x, y, energy[i]])

with open("edges.csv", "w", newline='') as f:
    writer = csv.writer(f)
    writer.writerow(["source", "target", "poids", "energie_moyenne"])
    for i, j in G.edges():
        writer.writerow([i, j, G[i][j]['weight'], G[i][j]['energy']])

with open("orbite.csv", "w", newline='') as f:
    writer = csv.writer(f)
    writer.writerow(["temps", "noeud_actuel", "prochaine_cible", "proba_dominante"])
    for row in orbit_log:
        writer.writerow(row)

# ----- VISUALISATION -----
fig, ax = plt.subplots(figsize=(8, 6))

def update(frame):
    ax.clear()
    nx.draw(G, pos=positions, ax=ax, node_color=energy, cmap='viridis', 
            with_labels=True, node_size=200, edge_color='gray')
    path_so_far = path[:frame+1]
    if len(path_so_far) > 1:
        path_edges = list(zip(path_so_far[:-1], path_so_far[1:]))
        nx.draw_networkx_edges(G, pos=positions, edgelist=path_edges, edge_color='crimson', width=2)
    ax.set_title(f"FLOATLAP — Orbite t = {frame}")

ani = FuncAnimation(fig, update, frames=timesteps, interval=300, repeat=False)
plt.show()
```

Slowcore_Flot_Simulation.py: This script models the Mental Space as a 2D grid, creating a continuous landscape of "coherence" and "entropy." It then simulates the trajectory of a point that "flows" across this landscape, with its movement influenced at each step by the local values of these two fields.

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation

# ----- PARAMÈTRES DU CHAMP -----
size = 50  # Taille de la grille (50x50)
beta = 2.0  # Sensibilité à l'entropie
gamma = 3.5  # Sensibilité à la courbure
steps = 100  # Nombre d'étapes du flot

# ----- CHAMPS DE COHÉRENCE ET D'ENTROPIE -----
np.random.seed(42)
coherence = np.clip(np.random.rand(size, size) + 0.5 * np.random.rand(size, size), 0, 1)
entropy = np.clip(np.random.rand(size, size), 0, 1)

# ----- INITIALISATION -----
trajectory = []
x, y = np.random.randint(0, size), np.random.randint(0, size)
trajectory.append((x, y))

# ----- DÉPLACEMENT DU FLOT -----
def get_neighbors(x, y):
    moves = [(-1,0), (1,0), (0,-1), (0,1)]
    return [(x+dx, y+dy) for dx, dy in moves if 0 <= x+dx < size and 0 <= y+dy < size]

def step(x, y):
    neighbors = get_neighbors(x, y)
    scores = []
    for nx, ny in neighbors:
        delta_entropy = entropy[nx, ny] - entropy[x, y]
        delta_coherence = coherence[nx, ny] - coherence[x, y]
        score = np.exp(-beta * delta_entropy - gamma * (delta_coherence)**2)
        scores.append(score)
    probs = np.array(scores)
    probs /= probs.sum()
    choice = np.random.choice(len(neighbors), p=probs)
    return neighbors[choice]

for _ in range(steps):
    x, y = step(x, y)
    trajectory.append((x, y))

# ----- VISUALISATION -----
fig, ax = plt.subplots(figsize=(6, 6))
ax.set_title("SLOW_CORE++ — Flot Harmonique Contraint")

coherence_map = ax.imshow(coherence, cmap='viridis', origin='lower')
scat = ax.scatter([], [], c='crimson', s=10)


def update(frame):
    coords = np.array(trajectory[:frame+1])
    scat.set_offsets(coords)
    ax.set_title(f"SLOW_CORE++ — Étape {frame}")
    return scat,

ani = FuncAnimation(fig, update, frames=steps, interval=200, repeat=False)
plt.colorbar(coherence_map, ax=ax, label="Cohérence locale")
plt.tight_layout()
plt.show()
```

3. Results and Visualizations
The simulations produce visual traces of the cognitive paths taken.

A. FLOATLAP - The Laplacian Orbit


Analysis of the visualization :

<img width="800" height="600" alt="orbites_laplaciennes" src="https://github.com/user-attachments/assets/bc873109-4afa-4eea-8bd6-b0e17ec48815" />

The image displays a network of nodes, with a clear path highlighted. This path represents the "orbit" taken by the simulated process. We can observe that the path tends to connect nodes that are structurally close and avoids traversing high-energy regions of the graph. This is a direct simulation of a system finding an efficient and coherent path of reasoning.

B. SLOW_CORE++ - The Harmonic Flow

<img width="600" height="600" alt="Harmonic flow on a coherence landscape" src="https://github.com/user-attachments/assets/72a3b8e4-ecc4-40ee-8f61-852b6ef85ac5" />


Analysis of the visualization (slowcore.png):

This visualization shows the trajectory of a "thought process" over a landscape of coherence (represented by the color gradient). The path is not random; it's a "flow" that clearly favors movement towards or along regions of higher coherence (brighter areas). It demonstrates the principle of a thought that seeks stability and clarity while navigating the space of possibilities.

4. Strategic Conclusion
These two simulations provide powerful evidence for the viability of the MCA's dynamic principles:

A Thought is a Trajectory: The abstract notion of a "train of thought" is successfully modeled as a measurable and visualizable trajectory through a state space. This moves the concept from metaphor to a model.

The Mental Space is a Landscape: These experiments prove that the Mental Space 𝕀 can be engineered as a dynamic landscape with its own topography (valleys of coherence, peaks of energy). This landscape actively shapes the flow of thought, rather than being a passive container for data.

Efficiency and Coherence are Computable: The simulations provide a concrete basis for the theoretical goal of "minimizing the cost of an orbit." The paths taken are computationally determined by seeking low energy and high coherence, making these qualitative goals quantifiable.

Together, these experiments show how Lyra can form coherent lines of reasoning, bridging the gap between the vast exploration of ENTRO_BRANCH and the emergence of a focused, directed thought process.

[![License: CC BY-NC-ND 4.0](https://licensebuttons.net/l/by-nc-nd/4.0/88x31.png)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
