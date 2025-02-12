# Setting the Exploration Route - Detailed Solution Notes

## Step-by-Step Solution Understanding

### Step 1: Understanding the Grid
```
Example Input:
5 5
.S*..    // S: Start point
..*2.    // 2: Exploration point
.....    // .: Empty space
1..**    // *: Wall/obstacle
...3.    // 1,2,3: Exploration points
```

Key Points:
1. Grid shows map of area
2. 'S' is starting position
3. Numbers are exploration points
4. '*' acts as walls/obstacles
5. '.' represents movable space

### Step 2: Point Collection
For the above example:
1. Start point (S): (0,1)
2. Point 1: (3,0)
3. Point 2: (1,3)
4. Point 4: (4,3)

Storage in code:
```java
points = [
    (0,1),  // S at index 0
    (3,0),  // Point 1
    (1,3),  // Point 2
    (4,3)   // Point 3
]
```

### Step 3: Distance Calculation
Example of BFS path finding from S to point 1:
```
Initial state:      After few steps:     Final path:
.S*..              .S*..                .S*..
..*2.              ##*2.                ##*2.
.....     →        ##...      →         ##...
1..**              1..**                1..**
...3.              ...3.                ...3.

# represents visited cells
Distance = 4 moves
```

Distance matrix creation:
```
distances[i][j] represents shortest path from point i to j

   S  1  2  3
S  0  4  5  7
1  4  0  5  6
2  5  5  0  4
3  7  6  4  0
```

### Step 4: Path Finding Using DFS
Example paths from S:
1. S → 1 → 2 → 3 → S = 4 + 5 + 4 + 7 = 20
2. S → 1 → 3 → 2 → S = 4 + 6 + 4 + 5 = 19
3. S → 2 → 1 → 3 → S = 5 + 5 + 6 + 7 = 23
4. S → 2 → 3 → 1 → S = 5 + 4 + 6 + 4 = 19
5. S → 3 → 1 → 2 → S = 7 + 6 + 5 + 5 = 23
6. S → 3 → 2 → 1 → S = 7 + 4 + 5 + 4 = 20

Minimum distance = 18 (S → 1 → 3 → 2 → S)

### Step 5: Code Execution Flow
1. Input Processing:
   ```java
   input() {
       Read R, C (5, 5)
       Read map[R][C]
       Find S and points
   }
   ```

2. Distance Calculation:
   ```java
   solve() {
       Calculate all distances using BFS
       Store in distances[][]
   }
   ```

3. Path Finding:
   ```java
   dfs() {
       Try all possible paths
       Update minDistance when complete path found
   }
   ```

### Step 6: Handling Special Cases
1. No exploration points:
   ```
   5 5
   .S...
   .....
   .....
   .....
   ..... 
   Output: 0
   ```

2. Impossible path:
   ```
   5 5
   .S*..
   ..*..
   ..*..
   1.*..
   .....
   Output: -1
   ```

3. Single point:
   ```
   5 5
   .S...
   .....
   ..1..
   .....
   .....
   Output: 4
   ```

## Problem Analysis

### Problem Statement
- A Smart robot needs to explore designated points on Mars
- Starting from position 'S' (Lululala)
- Must visit all exploration points (numbered 1 to n)
- Must return to start position
- Need to find minimum total distance

### Key Constraints
1. Grid size: R×C (1 ≤ R,C ≤ 100)
2. Cell types:
   - 'S': Starting position
   - '1' to '9': Exploration points
   - '.': Empty space
   - '*': Wall/obstacle
3. Movement: Up, down, left, right (one cell at a time)
4. Must return to start after visiting all points

## Solution Approach

### 1. Problem Type Identification
- This is a variation of the Traveling Salesman Problem (TSP)
- Additional complexity: Path finding between points in a grid with obstacles

### 2. Solution Components
1. Input Processing:
   - Read grid dimensions and map
   - Identify start point and exploration points

2. Distance Calculation:
   - Use BFS to find shortest path between each pair of points
   - Create distance matrix for quick lookup

3. Path Finding:
   - Use DFS with backtracking to try all possible orders
   - Maintain minimum total distance found

### 3. Implementation Strategy

#### A. Data Structures
```java
// Store coordinates
class Point {
    int x, y;
    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

// Global variables
static char[][] map;              // Grid map
static List<Point> points;        // All points (S + exploration points)
static int[][] distances;         // Distance matrix
static int minDistance;           // Minimum total distance found
```

#### B. Major Components

1. Input Processing
```java
static void input() throws Exception {
    // Read dimensions
    // Read map
    // Find and store points in order
    // Start point (S) always at index 0
}
```

2. BFS for Path Finding
```java
static int bfs(Point start, Point end) {
    // Use queue for BFS
    // Track visited cells and distances
    // Only move through valid cells (., S, numbers)
    // Return shortest path distance or -1 if no path
}
```

3. DFS for Path Permutation
```java
static void dfs(int current, int count, int totalDist, boolean[] visited) {
    // Base case: all points visited
    // Try visiting each unvisited point
    // Update minimum distance when complete path found
}
```

## Algorithm Flow

1. **Initialization Phase**
   - Read input grid
   - Identify start point ('S')
   - Collect all exploration points
   - Initialize distance matrix

2. **Distance Calculation Phase**
   - For each pair of points:
     - Run BFS to find shortest path
     - Store distance in matrix
     - If any path impossible, output -1

3. **Path Finding Phase**
   - Start from 'S' (index 0)
   - Try all possible orders of visiting points
   - Use backtracking with pruning
   - Track minimum total distance

4. **Output Phase**
   - Print minimum distance found
   - Handle special cases (0 points, impossible paths)

## Time Complexity Analysis

1. Input Processing: O(R×C)
2. BFS: O(R×C) per path
3. Distance Matrix: O(N² × R×C) where N = number of points
4. DFS Path Finding: O(N!) for permutations
5. Overall: O(N² × R×C + N!)

## Space Complexity Analysis

1. Grid Map: O(R×C)
2. Distance Matrix: O(N²)
3. BFS Queue/Visited: O(R×C)
4. Overall: O(R×C + N²)

## Implementation Notes

### Key Optimizations
1. Store points in order of appearance
2. Keep start point at index 0
3. Pre-calculate all distances
4. Prune DFS paths that exceed current minimum
5. Early termination for impossible cases

### Edge Cases Handled
1. No exploration points (return 0)
2. Impossible paths (return -1)
3. Invalid moves through walls
4. Different grid configurations

## Complete Implementation

### Full Code with Comments
```java
import java.io.*;
import java.util.*;

/**
 * Point class to store coordinates
 */
class Point {
    int x, y;
    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Main {
    // Global variables for grid dimensions
    static int R, C;
    // Map representation
    static char[][] map;
    // List to store all points (S and exploration points)
    static List<Point> points;
    // Matrix to store distances between points
    static int[][] distances;
    // Track minimum distance found
    static int minDistance;
    // Direction arrays for movement
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    // Starting point
    static Point start;
    
    /**
     * Main method - entry point
     */
    public static void main(String[] args) throws Exception {
        input();  // Read input
        solve();  // Find solution
    }
    
    /**
     * Input processing method
     * Reads grid dimensions and map
     * Identifies start and exploration points
     */
    static void input() throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        
        // Read dimensions
        R = Integer.parseInt(st.nextToken());
        C = Integer.parseInt(st.nextToken());
        
        // Initialize map and points list
        map = new char[R][C];
        points = new ArrayList<>();
        
        // Read map and find points
        for(int i = 0; i < R; i++) {
            String line = br.readLine();
            for(int j = 0; j < C; j++) {
                map[i][j] = line.charAt(j);
                if(map[i][j] == 'S') {
                    // Store start point
                    start = new Point(i, j);
                    points.add(0, start);  // Add at index 0
                }
                else if(Character.isDigit(map[i][j])) {
                    // Add exploration point
                    points.add(new Point(i, j));
                }
            }
        }
    }
    
    /**
     * Main solution method
     * Calculates distances and finds minimum path
     */
    static void solve() {
        // Handle case with no exploration points
        if(points.size() <= 1) {
            System.out.println("0");
            return;
        }
        
        // Calculate distances between all points
        distances = new int[points.size()][points.size()];
        for(int i = 0; i < points.size(); i++) {
            for(int j = 0; j < points.size(); j++) {
                if(i != j) {
                    distances[i][j] = bfs(points.get(i), points.get(j));
                    // If any path is impossible
                    if(distances[i][j] == -1) {
                        System.out.println("-1");
                        return;
                    }
                }
            }
        }
        
        // Find minimum path using DFS
        minDistance = Integer.MAX_VALUE;
        boolean[] visited = new boolean[points.size()];
        visited[0] = true;  // Mark start as visited
        dfs(0, 1, 0, visited);
        
        // Output result
        System.out.println(minDistance);
    }
    
    /**
     * DFS method to try all possible paths
     * @param current Current point index
     * @param count Number of points visited
     * @param totalDist Total distance so far
     * @param visited Array tracking visited points
     */
    static void dfs(int current, int count, int totalDist, boolean[] visited) {
        // Base case: all points visited
        if(count == points.size()) {
            // Update minimum distance including return to start
            minDistance = Math.min(minDistance, totalDist + distances[current][0]);
            return;
        }
        
        // Try each unvisited point
        for(int next = 1; next < points.size(); next++) {
            if(!visited[next]) {
                int newDist = totalDist + distances[current][next];
                // Pruning: only continue if path could be better than current minimum
                if(newDist < minDistance) {
                    visited[next] = true;
                    dfs(next, count + 1, newDist, visited);
                    visited[next] = false;
                }
            }
        }
    }
    
    /**
     * BFS method to find shortest path between two points
     * @param start Starting point
     * @param end Target point
     * @return Shortest distance or -1 if no path exists
     */
    static int bfs(Point start, Point end) {
        Queue<Point> queue = new LinkedList<>();
        int[][] dist = new int[R][C];
        boolean[][] visited = new boolean[R][C];
        
        // Initialize distances to -1
        for(int i = 0; i < R; i++) Arrays.fill(dist[i], -1);
        
        // Start BFS from start point
        queue.offer(start);
        visited[start.x][start.y] = true;
        dist[start.x][start.y] = 0;
        
        while(!queue.isEmpty()) {
            Point cur = queue.poll();
            
            // If reached end point
            if(cur.x == end.x && cur.y == end.y) {
                return dist[cur.x][cur.y];
            }
            
            // Try all four directions
            for(int i = 0; i < 4; i++) {
                int nx = cur.x + dx[i];
                int ny = cur.y + dy[i];
                
                // Check if move is valid
                if(nx >= 0 && nx < R && ny >= 0 && ny < C && 
                   !visited[nx][ny] && (map[nx][ny] == '.' || map[nx][ny] == 'S' || 
                                      Character.isDigit(map[nx][ny]))) {
                    visited[nx][ny] = true;
                    dist[nx][ny] = dist[cur.x][cur.y] + 1;
                    queue.offer(new Point(nx, ny));
                }
            }
        }
        return -1; // No path found
    }
}
