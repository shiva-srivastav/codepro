# Labyrinth Path Finding Solution

## Problem Understanding
We need to find the shortest path from point A to point B in a labyrinth where:
- '.' represents floor (can walk)
- '#' represents wall (cannot walk)
- 'A' is the starting point
- 'B' is the ending point

## Solution Approach
The solution uses DFS (Depth-First Search) with backtracking to find all possible paths and return the shortest one.

### 1. Key Components

#### Pair Class
```java
static class Pair {
    String str;   // Stores the path directions
    int min;      // Stores the minimum steps
    Pair(String str, int min) {
        this.str = str;
        this.min = min;
    }
}
```

#### Direction Arrays
```java
int rowDirection[] = {0, -1, 0, 1};   // Left, Up, Right, Down
int colDirection[] = {-1, 0, 1, 0};   // Corresponding column changes
```

### 2. Algorithm Steps

1. **Input Processing**
```java
// Read dimensions
int n = sc.nextInt();  // height
int m = sc.nextInt();  // width

// Read labyrinth map
char[][] mat = new char[n][m];
for (int i = 0; i < n; i++) {
    String str = sc.next();
    for (int j = 0; j < str.length(); j++) {
        mat[i][j] = str.charAt(j);
    }
}
```

2. **Find Starting Point 'A'**
```java
for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        if (mat[i][j] == 'A') {
            pair = dfs(mat, visited, i, j, 0);
            // ...
        }
    }
}
```

3. **DFS Implementation**
```java
public static Pair dfs(char[][] mat, boolean[][] visited, int srx, int sry, int steps) {
    // Base case: Found 'B'
    if (mat[srx][sry] == 'B') {
        return new Pair("", steps);
    }

    visited[srx][sry] = true;
    Pair bestPath = null;

    // Try all four directions
    for (int i = 0; i < 4; i++) {
        int x = srx + rowDirection[i];
        int y = sry + colDirection[i];
        
        // Check if valid move
        if (isValidMove(x, y, mat, visited)) {
            Pair path = dfs(mat, visited, x, y, steps + 1);
            if (path != null) {
                // Update best path if current path is shorter
                if (bestPath == null || path.min < bestPath.min) {
                    bestPath = new Pair(direction(rowDirection[i], colDirection[i]) + path.str, path.min);
                }
            }
        }
    }
    
    visited[srx][sry] = false;  // Backtrack
    return bestPath;
}
```

### 3. Example Walkthrough
For the input:
```
5 8
########
#.A#...#
#.##.#B#
#......#
########
```

1. Find 'A' at position (1,2)
2. DFS explores:
   - Left path: Leads to solution
   - Up path: Blocked by wall
   - Right path: Longer route
   - Down path: Explored but not shortest

3. Final path found: "LDDRRRRRU" (9 steps)
   ```
   ########
   #.A#...#
   #.##.#B#
   #......#
   ########
   ```
   Path visualization: A → Left → Down → Down → Right → Right → Right → Right → Right → Up → B

### 4. Time and Space Complexity
- Time Complexity: O(4^(N×M))
  - In worst case, we might explore all possible paths
- Space Complexity: O(N×M)
  - For visited array and recursion stack

### 5. Key Insights
1. DFS with backtracking allows exploration of all possible paths
2. Visited array prevents cycles
3. Backtracking is crucial for finding alternate paths
4. Direction mapping helps in path construction

### 6. Error Handling
1. Check boundary conditions
2. Validate input characters
3. Handle case when no path exists
4. Consider edge cases like immediate adjacency of A and B

### 7. Possible Optimizations
1. Use BFS for shortest path (more efficient)
2. Add pruning conditions
3. Use distance heuristics
4. Implement iterative deepening DFS
