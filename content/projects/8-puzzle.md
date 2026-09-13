---
title: "8-Puzzle Solver and Graphical Visualization"
date: 2024-08-01
tags: ["Algorithms", "Python", "Data Structures", "Pygame", "Search Algorithms"]
summary: "Optimal 8-Puzzle solver implementing Breadth-First Search (BFS) with hash-set visited state pruning in Python. Features automated file serialization (Nodes.txt, nodePath.txt) and a custom Pygame graphical engine animating the step-by-step solution path."
cover:
  image: "images/projects/8-puzzle/unsolved.png"
  alt: "8-Puzzle Solver initial and goal state visualization"
  hiddenInSingle: true
weight: 9
---

Engineered a robust Python-based computational solver and graphical animation engine for the classic 8-puzzle sliding tile problem. The system combines an unweighted graph search algorithm (Breadth-First Search) with hash-set state deduplication, automated text file serialization, and a custom Pygame rendering pipeline. The complete open-source implementation and codebase are available on GitHub: [GitHub Repository](https://github.com/GraysonGilbert/8_puzzle_problem).

## Initial State vs. Target Solved Configuration

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Initial Scrambled Board</span>
    <img src="/images/projects/8-puzzle/unsolved.png" alt="Initial scrambled 8-puzzle configuration" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Target Solved Configuration</span>
    <img src="/images/projects/8-puzzle/solved.png" alt="Target solved 8-puzzle configuration" />
  </div>
</div>

---

## Animated Solution Playback

<div class="project-video-short-wrapper">
  <div class="project-video-short">
    <iframe src="https://www.youtube.com/embed/15I6erhJCCw" title="8-Puzzle Pygame step-by-step solution animation" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  </div>
</div>

<p class="project-caption">Demonstration of the Pygame graphical visualizer stepping sequentially through the reconstructed nodePath.txt solution sequence at 1.0-second intervals.</p>

---

## Algorithmic Complexity & Architecture Matrix

| Computational Dimension | Technical Metric / Mechanism | Algorithmic Justification |
| :--- | :--- | :--- |
| **Search Algorithm** | Breadth-First Search (BFS) | Guarantees minimum move count (shortest path) on unweighted state graphs |
| **State Space Size** | 181,440 reachable states ($9! / 2$) | Half of the $9! = 362,880$ permutations are solvable due to parity invariants |
| **Visited State Lookup** | $O(1)$ Hash-Set Query | Boards flattened into immutable 9-tuples to prevent cycles and re-expansions |
| **Branching Factor ($b$)** | $2 \le b \le 4$ legal moves per state | Boundary checks dynamically constrain moves based on empty "0" tile coordinates |
| **Data Serialization** | File-backed decoupled pipeline | Solver writes `Nodes.txt`, `NodesInfo.txt`, and `nodePath.txt` for downstream consumers |
| **Rendering Canvas** | Pygame 2D Graphics | 300×300 viewport with 100×100 tile cells and timed step playback |

---

## Two-Stage Decoupled Software Pipeline

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Stage 1: Algorithmic Solver</div>
    <div class="project-arch-title">Queue-Based BFS Engine</div>
    <ul class="project-arch-list">
      <li>Represents 3×3 grid states and tracks the index of the empty "0" tile</li>
      <li>Validates grid boundaries prior to executing Up, Down, Left, and Right tile swaps</li>
      <li>Maintains parent-index tracking in memory for backward path reconstruction</li>
      <li>Serializes explored search graphs into Nodes.txt, NodesInfo.txt, and nodePath.txt</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Stage 2: Graphical Visualizer</div>
    <div class="project-arch-title">Pygame Animation Runtime</div>
    <ul class="project-arch-list">
      <li>Decouples computation from rendering by ingesting nodePath.txt</li>
      <li>Reconstructs raw text lines into 3×3 numerical state matrices</li>
      <li>Renders a 300×300 pixel canvas with 100×100 active white tiles and a grey empty slot</li>
      <li>Drives a 1.0s timed clock cycle to smoothly animate the exact solution sequence</li>
    </ul>
  </div>
</div>

---

## Breadth-First Search & Visited State Hashing

The 8-puzzle is modeled mathematically as a finite, unweighted state graph where each node represents a unique permutation of the 3×3 tile matrix, and directed edges represent valid sliding actions of adjacent tiles into the empty space (denoted as "0"). 

### Optimality via Breadth-First Search
Because state transitions carry uniform step cost (each legal slide counts as one move), Breadth-First Search (BFS) is deployed via a FIFO queue. BFS guarantees the discovery of the absolute shortest path to the goal configuration. The search systematically expands layer by layer, ensuring that no shorter path can be bypassed.

### Cycle Detection & State Hashing
To prevent infinite looping and redundant graph exploration, the search space must be pruned. In Python, mutable 2D lists cannot be hashed directly. To achieve constant-time $O(1)$ membership testing and insert operations, every 3×3 grid state is flattened into an immutable 9-tuple and registered in an in-memory hash set (`visited`). This mechanism prunes previously discovered configurations instantly, restricting expansion strictly to novel states.

### Backtracking Parent Pointers
During traversal, each generated child node records the array index of its parent node. Upon encountering the exact goal state configuration, the algorithm initiates a backward traversal from the goal node index through parent pointers up to the root initial state. This traceback isolates the exact minimal sequence of states required to solve the puzzle.

---

## File Serialization & Decoupled Architecture

Rather than coupling computation directly with graphical rendering, the software architecture establishes a file-backed, decoupled pipeline through three primary artifacts:

- **`Nodes.txt`**: Logs every board configuration explored during the breadth-first traversal in chronological order of discovery.
- **`NodesInfo.txt`**: Maintains relational metadata for every node, recording its unique integer ID, parent node index, and traversal cost.
- **`nodePath.txt`**: Contains exclusively the isolated sequence of board matrices forming the optimal solution path from root to goal.

### Engineering Benefits of Decoupling
This file serialization strategy decouples algorithmic computation from visualization runtime. The headless solver script can execute massive state graph searches in high-performance or batch environments without graphical overhead. Downstream applications—such as the custom Pygame visualizer or automated test suites—can subsequently ingest `nodePath.txt` independently for playback, verification, and UI rendering.
