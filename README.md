# Snake Game Pathfinding Visualizer 

- Game Description
- Approaches
  - Greedy Approach
  - Hamilton Approach
    - Build a Hamiltonian Cycle
  - Helper Path functions
    - Shortest Path
    - Longest Path


## Game Description

In the game, the snake is allowed to move inside a 2-dimensional playing field (game map) surrounded by walls. At each discrete interval (a time step), the snake must move forward, turn left, or turn right as the game requires that the snake cannot stop moving. The game will randomly generate and place one piece of food on the game map whenever there is no food on the map. When the snake moves onto a piece of food, the food is eaten and the snake's length grows by one. The goal is to eat as many pieces of food as possible without ending the game by colliding the snake into itself or the walls.

In our game settings, the game map will be 8 units tall and 8 units wide consisting of 64 available spaces. The snake will initially begin at the top-left corner, facing right, with an initial length of 4 units. Therefore, the snake can eat at most 60 pieces of food before filling up the entire map.

The Game has 2 built-in approaches: greedy and hamiltonian, and both utilize different path-finding algorithms to complete the game. 

## Approaches

### Greedy Approach

Directs the snake to eat the food along the shortest path if it thinks the snake will be safe. Otherwise, it makes the snake wander around by using the helper search functions until a safe path can be found

Concretely, to find the snake **S1**'s next moving direction **D**, the solver follows the steps below:

1. Compute the shortest path **P1** from **S1**'s head to the food. If **P1** exists, go to step 2. Otherwise, go to step 4.
2. Move a virtual snake **S2** (the same as **S1**) to eat the food along path **P1**.
3. Compute the longest path **P2** from **S2**'s head to its tail. If **P2** exists, let **D** be the first direction in path **P1**. Otherwise, go to step 4.
4. Compute the longest path **P3** from **S1**'s head to its tail. If **P3** exists, let **D** be the first direction in path **P3**. Otherwise, go to step 5.
5. Let **D** be the direction that makes **S1** the farthest from the food.

### Hamilton Approach

Builds a Hamiltonian cycle on the game map first and then directs the snake to eat the food along the cycle path. To reduce the average steps the snake takes to success, it enables the snake to take shortcuts if possible. Again, it depends on finding the longest path first.

#### Build a Hamiltonian Cycle

Suppose we want to build a Hamiltonian cycle on a 4*4 map. Then our goal is to assign the path index to each point on the map. The image below shows a possible Hamiltonian cycle:

To construct the cycle above, first we fix the point 0, 1 and 2 (considering the snake's initial head and bodies are 2, 1, 0, respectively). Then we make point 1 unreachable and generate the longest path from point 2 to point 0. Finally, we join the starting point 2 and the ending point 0, which forms a Hamiltonian cycle:


Note that this algorithm is not a general way to find a Hamiltonian cycle. It only works when the initial positions of the snake's head and bodies are similar to the situation described above.


### Helper Path Functions

Provides methods to find the shortest path and the longest path from the snake's head to other points on the matrix. Used both by the Greedy and Hamiltonian Solver.

#### Shortest Path

Uses breadth-first search to find the shortest path. ntation, furing each iteration, the adjacent point in the last traversed direction will be traversed first.

#### Longest Path

The longest-path on a cyclic, undirected and unweighted graph is NP-hard. Helper function uses a heuristic approach to find suboptimal solutions: first finds the shortest path between the two points and then extends each pair of path pieces until no extensions can be found.
