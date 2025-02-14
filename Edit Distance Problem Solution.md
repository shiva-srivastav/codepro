# Edit Distance Problem Solution

## Problem Understanding
The problem asks us to find the minimum number of operations needed to convert one string into another using three operations:
1. Add a character
2. Remove a character
3. Replace a character

## Solution Approach: Dynamic Programming
We use a DP table where `dp[i][j]` represents the minimum operations needed to convert first i characters of str1 to first j characters of str2.

### 1. DP Table Setup
```java
int[][] dp = new int[m + 1][n + 1];  // m = str1.length, n = str2.length

// Initialize first row and column
for (int i = 0; i <= m; i++) {
    dp[i][0] = i;    // Cost of deleting all characters
}
for (int j = 0; j <= n; j++) {
    dp[0][j] = j;    // Cost of inserting all characters
}
```

### 2. Example Walkthrough with "LOVE" to "MOVIE"

Initial DP table:
```
    M  O  V  I  E
 [0, 1, 2, 3, 4, 5]
L[1, ?, ?, ?, ?, ?]
O[2, ?, ?, ?, ?, ?]
V[3, ?, ?, ?, ?, ?]
E[4, ?, ?, ?, ?, ?]
```

### 3. DP State Transition
For each cell (i,j), we have two cases:

1. If characters match (str1[i-1] == str2[j-1]):
   ```java
   dp[i][j] = dp[i-1][j-1]  // No operation needed
   ```

2. If characters don't match:
   ```java
   dp[i][j] = 1 + min(
       dp[i-1][j],    // Deletion
       dp[i][j-1],    // Insertion
       dp[i-1][j-1]   // Replacement
   )
   ```

### 4. Step-by-Step Solution for "LOVE" to "MOVIE"

1. For 'L' vs 'M':
   - Characters don't match
   - Consider: Replace 'L' with 'M'
   - Cost = 1

2. For "LO" vs "MO":
   - 'O' matches
   - Keep previous cost
   - Total cost = 1

3. For "LOV" vs "MOV":
   - 'V' matches
   - Keep previous cost
   - Total cost = 1

4. For "LOVE" vs "MOVI":
   - Need to insert 'I'
   - Cost increases by 1
   - Total cost = 2

5. For "LOVE" vs "MOVIE":
   - 'E' matches
   - Keep previous cost
   - Final cost = 2

Final DP table:
```
    M  O  V  I  E
 [0, 1, 2, 3, 4, 5]
L[1, 1, 2, 3, 4, 5]
O[2, 2, 1, 2, 3, 4]
V[3, 3, 2, 1, 2, 3]
E[4, 4, 3, 2, 2, 2]
```

### 5. Time and Space Complexity
- Time Complexity: O(m×n)
  - We fill each cell in the DP table once
- Space Complexity: O(m×n)
  - Size of the DP table

### 6. Operations to Convert "LOVE" to "MOVIE"
1. Replace 'L' with 'M': "MOVE"
2. Insert 'I': "MOVIE"

## Key Insights
1. The DP approach breaks down the problem into smaller subproblems
2. Each cell represents the minimum operations needed for the substrings up to that point
3. The three operations (insert, delete, replace) are considered at each step
4. Base cases handle empty string scenarios

## Common Pitfalls
1. Forgetting to initialize the first row and column
2. Incorrect indexing when comparing characters (remember i-1 and j-1)
3. Not considering all three operations when characters don't match

## Complete Solution Code
```java
import java.io.*;
import java.util.*;

public class Solution {
    public static void main(String[] args) {
        // Initialize Scanner for input
        Scanner sc = new Scanner(System.in);
        
        // Read the two strings
        String str1 = sc.next();  // First string
        String str2 = sc.next();  // Second string
        
        // Calculate and print the minimum edit distance
        int result = editDistance(str1, str2);
        System.out.println(result);
    }
    
    public static int editDistance(String str1, String str2) {
        // Get lengths of both strings
        int m = str1.length();
        int n = str2.length();
        
        // Create DP table for storing results
        // dp[i][j] represents minimum operations needed to convert
        // first i characters of str1 to first j characters of str2
        int[][] dp = new int[m + 1][n + 1];
        
        // Initialize first row and column
        // Converting empty string to string of length i requires i insertions
        for (int i = 0; i <= m; i++) {
            dp[i][0] = i;  // Number of deletions needed
        }
        // Converting string of length i to empty string requires i deletions
        for (int j = 0; j <= n; j++) {
            dp[0][j] = j;  // Number of insertions needed
        }
        
        // Fill the DP table
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                // If current characters match, no operation needed
                // Use the result from previous state
                if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    // If characters don't match, take minimum of three operations
                    // 1. Replace: dp[i-1][j-1]
                    // 2. Delete:  dp[i-1][j]
                    // 3. Insert:  dp[i][j-1]
                    // Add 1 for the current operation
                    dp[i][j] = 1 + Math.min(dp[i - 1][j],      // Delete
                                   Math.min(dp[i][j - 1],       // Insert
                                          dp[i - 1][j - 1]));   // Replace
                }
            }
        }
        
        // Return the minimum number of operations needed
        return dp[m][n];
    }
}
