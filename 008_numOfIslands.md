The "Number of Islands" problem is a classic example of 
using graph traversal techniques 
to identify connected components in a grid. 

Each cell in the grid represents either land (`'1'`) or water (`'0'`). 

An island is defined as a group of adjacent land cells connected horizontally or vertically. 

To solve this, 
we iterate through each cell in the grid,
and whenever we encounter a `'1'`, 
we initiate a depth-first search (DFS) 
to explore and mark all connected land cells as visited
—typically by flipping them to `'0'`.

This ensures that each island is counted only once. 

The total number of times we initiate DFS corresponds to the number of distinct islands. 

This approach is efficient and intuitive, 
especially for static grids, and can be extended to breadth-first search (BFS) or Union-Find for more dynamic or performance-sensitive scenarios.


intuitive - the logic is easy to grasp and implement




# -----

classic example 
graph traversal 
represent 
adjacent 
