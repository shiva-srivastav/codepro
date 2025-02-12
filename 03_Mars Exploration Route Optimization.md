# Setting the Exploration Route - Solution Notes

## Problem Understanding
The problem involves finding the shortest path for a Smart robot to:
1. Start from point 'S' (Lululala location)
2. Visit all exploration points (numbered 1 to n)
3. Return to point 'S'
4. Navigate through a 2D grid with walls

## Key Constraints
- Grid is represented as R rows × C columns (max 100×100)
- Movement: Up, down, left, right (adjacent cells only)
- Cannot move through walls (marked as '#')
- Must visit all exploration points exactly once
- Must return to starting position 'S'
- Points marked as numbers 1 to 9 are exploration points

## Solution Approach

### 1. Input Processing
```java
void inputData() throws Exception {
    // Read dimensions R and C
    StringTokenizer st = new StringTokenizer(br.readLine());
    R = Integer.parseInt(st.nextToken());
    C = Integer.parseInt(st.nextToken());
    
    // Initialize map
    map = new char[R][C];
    
    // Read map row by row
    for(int r = 0; r < R; r++) {
        map[r] = br.readLine().toCharArray();
    }
}
```

### 2. Finding Important Points
```java
void findPoints() {
    points = new ArrayList<>();
    // Add starting point 'S'
    for(int i = 0; i < R; i++) {
        for(int j = 0; j < C; j++) {
            if(map[i][j] == 'S') {
                points.add(new Point(i, j));
                break;
            }
        }
    }
    
    // Add exploration points (1 to 9)
    for(char num = '1'; num <= '9'; num++) {
        for(int i = 0; i < R; i++) {
            for(int j = 0; j < C; j++) {
                if(map[i][j] == num) {
                    points.add(new Point(i, j));
                }
            }
        }
    }
}
```

### 3. Distance Calculation
- Use BFS (Breadth-First Search) to find shortest path between any two points
- Create distance matrix between all points
```java
int[][] calculateDistances() {
    int n = points.size();
    int[][] distances = new int[n][n];
    
    // Calculate distance between each pair of points
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            if(i != j) {
                distances[i][j] = bfs(points.get(i), points.get(j));
            }
        }
    }
    return distances;
}
```

### 4. Path Finding using Backtracking
- Use backtracking to try all possible orders of visiting points
- Keep track of minimum distance found so far
```java
void findMinPath(int current, int visited, int distance) {
    // If all points visited, check return to start
    if(visited == ((1 << points.size()) - 1)) {
        minDistance = Math.min(minDistance, distance + distances[current][0]);
        return;
    }
    
    // Try visiting each unvisited point
    for(int next = 1; next < points.size(); next++) {
        if((visited & (1 << next)) == 0) {
            findMinPath(next, visited | (1 << next), 
                       distance + distances[current][next]);
        }
    }
}
```

### 5. BFS Implementation
```java
int bfs(Point start, Point end) {
    Queue<Point> queue = new LinkedList<>();
    boolean[][] visited = new boolean[R][C];
    int[][] distance = new int[R][C];
    
    queue.offer(start);
    visited[start.x][start.y] = true;
    
    while(!queue.isEmpty()) {
        Point current = queue.poll();
        
        if(current.x == end.x && current.y == end.y) {
            return distance[current.x][current.y];
        }
        
        // Try all four directions
        for(int d = 0; d < 4; d++) {
            int nx = current.x + dx[d];
            int ny = current.y + dy[d];
            
            if(isValid(nx, ny) && !visited[nx][ny]) {
                visited[nx][ny] = true;
                distance[nx][ny] = distance[current.x][current.y] + 1;
                queue.offer(new Point(nx, ny));
            }
        }
    }
    return -1; // Path not found
}
```

## Time Complexity Analysis
- Input Processing: O(R×C)
- Finding Points: O(R×C)
- Distance Calculation: O(n² × R×C) where n is number of points
- Path Finding: O(n! × n) where n is number of points
- Overall: O(n! × n + n² × R×C)

## Space Complexity Analysis
- Map Storage: O(R×C)
- Distance Matrix: O(n²)
- BFS visited array: O(R×C)
- Overall: O(R×C + n²)

## Complete Solution with Detailed Implementation
```java
import java.io.*;
import java.util.*;

public class Main {
    static int R, C;
    static char[][] map;
    static ArrayList<Point> points;
    static int[][] distances;
    static int minDistance = Integer.MAX_VALUE;
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        // Read input
        StringTokenizer st = new StringTokenizer(br.readLine());
        R = Integer.parseInt(st.nextToken());
        C = Integer.parseInt(st.nextToken());
        
        // Initialize map
        map = new char[R][];
        for(int i = 0; i < R; i++) {
            map[i] = br.readLine().toCharArray();
        }
        
        // Find all points
        findPoints();
        
        // Calculate distances between all points
        distances = calculateDistances();
        
        // Find minimum path
        findMinPath(0, 1, 0);
        
        // Output result
        System.out.println(minDistance);
    }
    
    // Other methods as described above
}

class Point {
    int x, y;
    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

/**
 * Complete implementation with all helper methods and main logic
 */
public class Main {
    static int R, C;
    static char[][] map;
    static ArrayList<Point> points;
    static int[][] distances;
    static int minDistance = Integer.MAX_VALUE;
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static BufferedReader br;
    
    public static void main(String[] args) throws Exception {
        Main main = new Main();
        main.inputData();
        main.solve();
    }
    
    void inputData() throws Exception {
        br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        R = Integer.parseInt(st.nextToken());
        C = Integer.parseInt(st.nextToken());
        
        map = new char[R][];
        for(int i = 0; i < R; i++) {
            map[i] = br.readLine().toCharArray();
        }
    }
    
    void solve() {
        // Find all important points (S and exploration points)
        findPoints();
        
        // Calculate distances between all points
        distances = calculateDistances();
        
        // Find minimum path starting from S (index 0)
        findMinPath(0, 1, 0);
        
        // Output result
        System.out.println(minDistance);
    }
    
    void findPoints() {
        points = new ArrayList<>();
        
        // First find starting point 'S'
        for(int i = 0; i < R; i++) {
            for(int j = 0; j < C; j++) {
                if(map[i][j] == 'S') {
                    points.add(new Point(i, j));
                    break;
                }
            }
        }
        
        // Then find all exploration points (1-9)
        for(char num = '1'; num <= '9'; num++) {
            for(int i = 0; i < R; i++) {
                for(int j = 0; j < C; j++) {
                    if(map[i][j] == num) {
                        points.add(new Point(i, j));
                    }
                }
            }
        }
    }
    
    int[][] calculateDistances() {
        int n = points.size();
        int[][] dist = new int[n][n];
        
        // Calculate distance between each pair of points
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < n; j++) {
                if(i != j) {
                    dist[i][j] = bfs(points.get(i), points.get(j));
                }
            }
        }
        return dist;
    }
    
    void findMinPath(int current, int visited, int distance) {
        // If all points are visited, add distance to return to start
        if(visited == ((1 << points.size()) - 1)) {
            minDistance = Math.min(minDistance, distance + distances[current][0]);
            return;
        }
        
        // Try visiting each unvisited point
        for(int next = 1; next < points.size(); next++) {
            if((visited & (1 << next)) == 0) {
                findMinPath(next, visited | (1 << next), 
                          distance + distances[current][next]);
            }
        }
    }
    
    int bfs(Point start, Point end) {
        Queue<Point> queue = new LinkedList<>();
        boolean[][] visited = new boolean[R][C];
        int[][] distance = new int[R][C];
        
        queue.offer(start);
        visited[start.x][start.y] = true;
        
        while(!queue.isEmpty()) {
            Point current = queue.poll();
            
            if(current.x == end.x && current.y == end.y) {
                return distance[current.x][current.y];
            }
            
            // Try all four directions
            for(int d = 0; d < 4; d++) {
                int nx = current.x + dx[d];
                int ny = current.y + dy[d];
                
                if(isValid(nx, ny) && !visited[nx][ny] && map[nx][ny] != '#') {
                    visited[nx][ny] = true;
                    distance[nx][ny] = distance[current.x][current.y] + 1;
                    queue.offer(new Point(nx, ny));
                }
            }
        }
        return -1; // Path not found
    }
    
    boolean isValid(int x, int y) {
        return x >= 0 && x < R && y >= 0 && y < C;
    }
}
```
