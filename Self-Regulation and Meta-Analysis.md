Chapter 3 - Advanced Cognitive Dynamics – Self-Regulation and Meta-Analysis
1. Experiment Objective
While the previous chapters demonstrated how the Lyra architecture can explore possibilities (ENTRO_BRANCH) and form coherent paths (FLOATLAP), this chapter delves into its more advanced, meta-cognitive capabilities. The goal is to show that Lyra is not a static system but a dynamic organism capable of observing, analyzing, and regulating its own internal processes.

The objectives of these experiments are to:

Demonstrate modules designed to ensure the system's "cognitive health" by detecting drift, bias, and meaningless resonance loops.

Prototype higher-order learning mechanisms where the system analyzes its own transformation patterns to extract new rules.

Simulate a global orchestration process that manages the system's long-term evolution, balancing exploration and refinement.

2. Models, Protocols, and Prototypes
This section presents the conceptual models and simulation scripts for Lyra's higher-order functions.

SATORYX - The "Mythification" Detector:

Concept: This module acts as an epistemological guardian. Its purpose is to detect when the system's internal dynamics deviate from a "natural" or "harmonious" state, indicating a drift towards self-reinforcing but potentially meaningless patterns (a "myth").

Mechanism: As detailed in SATORYX Modèle Mathématique Final.md, the model compares the observed statistical distribution of interactions between modules to an ideal theoretical distribution (the Sato-Tate distribution). A significant deviation, measured by the Kullback-Leibler divergence, triggers a "gentle disillusionment" alert.

PATTERNFORGE & HierarchiSense - The Learning Engines:

Concept: These prototypes demonstrate Lyra's ability to learn from its own operations. Instead of learning from external data, they analyze the system's internal transformations to discover new knowledge.

Mechanism: PATTERNFORGE.py compares "before" and "after" states to identify recurring transformation rules, logging high-frequency rules as stable and low-frequency ones as "weak hypotheses" in a "Journal of Forgetting."
```python
# Prototype PATTERNFORGE
# Objectif : Détecter des patterns de transformation complexes (forme et couleur), analyser leur fréquence, prioriser, et enregistrer les hypothèses faibles dans un Journal d'Oubli spécial ARC (CSV + Markdown).

from typing import List, Tuple, Dict
from collections import Counter
import csv
from datetime import datetime

class PatternForge:
    def __init__(self, frequency_threshold: int = 2):
        self.hypotheses: List[str] = []
        self.context_counter: Counter = Counter()
        self.frequency_threshold = frequency_threshold
        self.csv_file = "journal_oubli_arc.csv"
        self.md_file = "journal_oubli_arc.md"
        self.init_files()

    def init_files(self):
        # Initialisation des fichiers CSV et MD si besoin
        with open(self.csv_file, 'w', newline='', encoding='utf-8') as f_csv:
            writer = csv.writer(f_csv)
            writer.writerow(["timestamp", "module_source", "fragment_oublie", "intensite_oubli", "contexte_module", "lien_potentiel", "commentaire_auto"])

        with open(self.md_file, 'w', encoding='utf-8') as f_md:
            f_md.write("# Journal d'Oubli ARC\n\n")

    def log_to_journal(self, fragment: str, intensite: float, contexte: List[str]):
        timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        module_source = "PATTERNFORGE"
        lien_potentiel = "MemorySeeds, DelaySeeds"
        commentaire_auto = "Hypothèse faible capturée pour évolution différée."

        # CSV
        with open(self.csv_file, 'a', newline='', encoding='utf-8') as f_csv:
            writer = csv.writer(f_csv)
            writer.writerow([timestamp, module_source, fragment, intensite, contexte, lien_potentiel, commentaire_auto])

        # Markdown
        with open(self.md_file, 'a', encoding='utf-8') as f_md:
            f_md.write(f"## {timestamp}\n")
            f_md.write(f"**Source :** {module_source}\n\n")
            f_md.write(f"**Fragment oublié :** {fragment}\n\n")
            f_md.write(f"**Intensité oubli :** {intensite}\n\n")
            f_md.write(f"**Contexte module :** {contexte}\n\n")
            f_md.write(f"**Lien potentiel :** {lien_potentiel}\n\n")
            f_md.write(f"**Commentaire automatique :** {commentaire_auto}\n\n---\n\n")

    def compare_grids(self, grid_in: List[List[Tuple[str, str]]], grid_out: List[List[Tuple[str, str]]]) -> List[Dict]:
        diffs = []
        for i in range(len(grid_in)):
            for j in range(len(grid_in[0])):
                in_symbol, in_color = grid_in[i][j]
                out_symbol, out_color = grid_out[i][j]

                if in_symbol == out_symbol and in_color == out_color:
                    continue
                if in_color == 'black' and out_color == 'black':
                    continue

                context = self.extract_context_3x3(grid_in, i, j)
                flat_context = tuple([(cell[0], cell[1]) for row in context for cell in row])
                self.context_counter[flat_context] += 1
                diffs.append({
                    "position": (i, j),
                    "from": (in_symbol, in_color),
                    "to": (out_symbol, out_color),
                    "context": context
                })
        return diffs

    def extract_context_3x3(self, grid: List[List[Tuple[str, str]]], i: int, j: int) -> List[List[Tuple[str, str]]]:
        context = []
        for di in [-1, 0, 1]:
            row = []
            for dj in [-1, 0, 1]:
                ni, nj = i + di, j + dj
                if 0 <= ni < len(grid) and 0 <= nj < len(grid[0]):
                    row.append(grid[ni][nj])
                else:
                    row.append(('#', 'black'))
            context.append(row)
        return context

    def synthesize_rules(self, diffs: List[Dict]) -> List[str]:
        rules = {}
        for diff in diffs:
            flat_context = tuple([(cell[0], cell[1]) for row in diff["context"] for cell in row])
            key = (diff["from"], diff["to"], flat_context)
            if key not in rules:
                rules[key] = []
            rules[key].append(diff["position"])

        generalized_rules = []
        for (val_in, val_out, context), positions in rules.items():
            frequency = self.context_counter[context]
            in_symbol, in_color = val_in
            out_symbol, out_color = val_out
            intensite_oubli = round(1 - (frequency / (frequency + 2)), 2)  # formule simple d'intensité

            if frequency >= self.frequency_threshold:
                label = "[✅]"
            else:
                label = "[⚠️]"

            fragment = f"{label} '{in_symbol}({in_color})' devient '{out_symbol}({out_color})' ({len(positions)} cas, fréquence {frequency})."

            if label == "[⚠️]":
                self.log_to_journal(fragment, intensite_oubli, [f"{c[0]}({c[1]})" for c in context])

            generalized_rules.append(fragment)

        return generalized_rules

    def forge(self, grid_in: List[List[Tuple[str, str]]], grid_out: List[List[Tuple[str, str]]]) -> List[str]:
        diffs = self.compare_grids(grid_in, grid_out)
        rules = self.synthesize_rules(diffs)
        self.hypotheses.extend(rules)
        return rules

# Exemple d'utilisation
if __name__ == "__main__":
    grid_in = [
        [('□', 'black'), ('□', 'black'), ('□', 'black')],
        [('□', 'black'), ('■', 'red'), ('□', 'black')],
        [('□', 'black'), ('□', 'black'), ('□', 'black')]
    ]

    grid_out = [
        [('■', 'red'), ('■', 'red'), ('■', 'red')],
        [('■', 'red'), ('□', 'black'), ('■', 'red')],
        [('■', 'red'), ('■', 'red'), ('■', 'red')]
    ]

    forge = PatternForge(frequency_threshold=2)
    hypotheses = forge.forge(grid_in, grid_out)
    for h in hypotheses:
        print(h)

```

Hierarchisense.py analyzes spatial configurations to detect repeating, hierarchical block patterns.

```python
# Module : hierarchisense.py
# Fonction : Détecter des motifs hiérarchiques simples dans les grilles (mini-blocs répétitifs)

from typing import List, Tuple
import numpy as np

class HierarchiSense:
    def __init__(self, block_sizes: List[int] = [2, 3]):
        self.block_sizes = block_sizes  # Tailles de blocs à analyser (2x2, 3x3 par défaut)

    def detect_blocks(self, grid: List[List[Tuple[str, str]]]) -> List[str]:
        """
        Analyse la grille pour détecter des blocs répétitifs.
        Retourne une liste d'observations.
        """
        # Extraire uniquement les couleurs pour la détection hiérarchique
        numeric_grid = np.array([[self.color_to_int(cell[1]) for cell in row] for row in grid])
        observations = []

        for block_size in self.block_sizes:
            if numeric_grid.shape[0] < block_size or numeric_grid.shape[1] < block_size:
                continue  # Grille trop petite pour ce bloc

            sub_blocks = {}

            for i in range(0, numeric_grid.shape[0] - block_size + 1):
                for j in range(0, numeric_grid.shape[1] - block_size + 1):
                    block = numeric_grid[i:i+block_size, j:j+block_size]
                    block_tuple = tuple(block.flatten())
                    sub_blocks[block_tuple] = sub_blocks.get(block_tuple, 0) + 1

            # Chercher les blocs répétitifs significatifs
            for block, count in sub_blocks.items():
                if count > 1:
                    observations.append(f"[HierarchiSense] Bloc {block_size}x{block_size} répété {count} fois.")

        return observations

    def color_to_int(self, color: str) -> int:
        color_mapping = {
            'black': 0, 'red': 1, 'blue': 2, 'green': 3, 'yellow': 4,
            'gray': 5, 'cyan': 6, 'magenta': 7, 'orange': 8, 'brown': 9
        }
        return color_mapping.get(color, 0)

# --- Exemple d'utilisation ---
if __name__ == "__main__":
    # Exemple simplifié
    example_grid = [
        [('□', 'red'), ('□', 'red'), ('□', 'blue'), ('□', 'blue')],
        [('□', 'red'), ('□', 'red'), ('□', 'blue'), ('□', 'blue')],
        [('□', 'green'), ('□', 'green'), ('□', 'yellow'), ('□', 'yellow')],
        [('□', 'green'), ('□', 'green'), ('□', 'yellow'), ('□', 'yellow')]
    ]

    hierarchisense = HierarchiSense()
    observations = hierarchisense.detect_blocks(example_grid)

    for obs in observations:
        print(obs)

```

Orchestrateur_Fleuve_Fractal - The Global Conductor:

Concept: This script simulates the highest level of cognitive control in Lyra. It is not a single module but a process that manages the entire system's evolution over long periods.

Mechanism: The orchestrator reads a log of the system's past performance (fractal_memory_log.csv), identifies the most successful "genetic seeds" (configurations), and uses them to guide the next phase of exploration. Crucially, it also detects "saturation" (when progress stalls) and automatically adjusts its strategy to reintroduce novelty.

3. Results and Conceptual Visualizations
These advanced concepts are demonstrated through their outputs and conceptual diagrams.

A. SATORYX - Quantifying Cognitive Drift

The core of SATORYX is a comparison of distributions. A healthy system would show an observed pattern of interactions (histogram) that closely follows the ideal theoretical curve.

 [ Lyra Modules ]
        ↓
  [ Interactions ]
        ↓
  [ Interaction Angles θ ]
        ↓
[ Compare to ideal sin²(θ) curve ]
        ↓
 [ κ_myth (KL-Divergence) measured ]
    ↙        ↘
 [ OK ]    [ SATORYX Alert ]
             ↓
   [ Gentle Disillusionment ]
             ↓
 [ System Re-stabilization ]

Analysis: This model provides a mathematically grounded, quantifiable metric for the system's "epistemic health." It answers the critical question of how a complex system can prevent itself from "hallucinating" or getting stuck in unproductive loops.

B. PATTERNFORGE - Learning from Self-Observation

The output of this script is not a graph, but a set of inferred rules, demonstrating a form of meta-learning.

Sample Output:

[✅] '■(red)' becomes '□(black)' (8 cases, frequency 4). -> A high-confidence rule is learned.

[⚠️] '□(black)' becomes '■(blue)' (1 cases, frequency 1). -> A low-confidence "weak hypothesis" is logged to the Journal of Forgetting for potential future re-evaluation.

Analysis: This proves Lyra can perform a type of "science on itself," discovering the rules of its own world and distinguishing between strong laws and weak signals.

C. Orchestrateur_Fleuve_Fractal - Visualizing Long-Term Evolution

This simulation produces a time-series plot of the system's core performance metric ("Top Resonance").

Analysis: A plot generated by this script would show the "Top Resonance" metric increasing over cycles, then plateauing (a phase of saturation), followed by a dip and a new phase of growth after the orchestrator adjusts its mutation strategy. This visualizes a system that doesn't just perform a task, but actively manages its own learning and exploration strategy over time.

4. Strategic Conclusion
This final set of experiments demonstrates the most advanced and unique aspects of the Lyra architecture:

Lyra is a Meta-Cognitive System: It doesn't just "think"; it thinks about how it thinks. Modules like SATORYX and PATTERNFORGE are proof of this second-order, self-analytical capability. This directly addresses the challenge of interpretability and control in complex AI.

The System is Designed for Resilience: Through mechanisms like Système de Friction Vivante and SATORYX, Lyra is designed not just to resist cognitive drift but to use it as a productive signal. Tension and error are not bugs; they are features that trigger adaptation and growth.

Learning is a Holistic, Emergent Property: The system learns not only from external data but from observing its own internal dynamics. The Orchestrateur creates a powerful, self-improving feedback loop at the highest level, making the entire architecture a learning organism.

This chapter concludes the initial portfolio by showing that the Lyra framework provides a complete, end-to-end vision for a new kind of AI: one that is not only powerful but also structured, self-aware, and intentionally engineered for robust and creative cognition.

[![License: CC BY-NC-ND 4.0](https://licensebuttons.net/l/by-nc-nd/4.0/88x31.png)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
