## 8-Puzzle Solver and Graphical Visualization

**Overview**
Developed a Python-based algorithmic solver and graphical visualizer for the classic 8-Puzzle problem. The project implements a Breadth-First Search (BFS) algorithm to compute the optimal sequence of moves to reach a defined goal state, paired with a custom Pygame interface to animate the resulting solution step-by-step.

**Algorithmic Architecture (BFS)**
*   **State Management:** The system models the puzzle as a 3x3 column-based grid, actively tracking the location of the empty "0" tile. Valid moves (up, down, left, right) are dynamically calculated by checking grid boundaries before executing a tile swap.
*   **Breadth-First Search:** The solver algorithm utilizes a queue to explore node states and features backtracking to extract the optimal path from the initial state to the goal state.
*   **Optimization via Hashing:** To avoid infinite loops and efficiently track visited states, every explored board configuration is flattened into a tuple and stored as a hash value.
*   **Data Output:** Upon reaching the goal state, the system traces parent node indices backward to generate the sequential path. The explored nodes, node information, and the final sequence are then written out to local text files (`Nodes.txt`, `NodesInfo.txt`, and `nodePath.txt`).

**Software & Visualization**
*   **Data Parsing:** A standalone animation script reads the generated `nodePath.txt` file and reconstructs the strings back into 3x3 numerical matrices.
*   **Graphical Interface:** Built using the Pygame library, the interface renders a 300x300 pixel game window containing a grid of 100x100 pixel cells. 
*   **Automated Animation:** Active numbered tiles are rendered in white with black text, while the empty space is represented as a solid grey block. The script iterates through the parsed matrix list, pausing for one second between loops to smoothly animate the exact sequence of moves required to solve the puzzle.


video example: https://youtube.com/shorts/15I6erhJCCw?feature=share

github repo: https://github.com/GraysonGilbert/8_puzzle_problem