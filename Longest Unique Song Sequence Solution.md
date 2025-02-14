# Longest Unique Song Sequence Solution

## Problem Understanding
The problem asks us to find the longest sequence of consecutive songs where no song ID repeats. This is essentially finding the longest substring with unique elements, a classic sliding window problem.

## Solution Approach
The solution uses the sliding window technique with a HashSet to track unique elements. Let's break it down:

### 1. Data Structures Used
- `HashSet<Integer> seen`: Keeps track of unique songs in current window
- Two pointers: `start` and `end` to maintain the window
- `maxLength`: Tracks the longest valid sequence found

### 2. Algorithm Steps

```java
public static int longestUniqueSongSequence(int[] songs) {
    HashSet<Integer> seen = new HashSet<>();
    int maxLength = 0, start = 0;
    
    for (int end = 0; end < songs.length; end++) {
        // Step 1: Handle duplicates
        while (seen.contains(songs[end])) {
            seen.remove(songs[start]);
            start++;
        }
        
        // Step 2: Add current song
        seen.add(songs[end]);
        
        // Step 3: Update max length
        maxLength = Math.max(maxLength, end - start + 1);
    }
    
    return maxLength;
}
```

### 3. Detailed Walkthrough with Example
Let's use the sample input: [1, 2, 1, 3, 2, 7, 4, 2]

1. Initial state: 
   - Window: [1]
   - seen = {1}
   - maxLength = 1

2. Add 2:
   - Window: [1, 2]
   - seen = {1, 2}
   - maxLength = 2

3. Next is 1 (duplicate):
   - Remove first 1
   - Window: [2, 1]
   - seen = {2, 1}
   - maxLength = 2

4. Add 3:
   - Window: [2, 1, 3]
   - seen = {2, 1, 3}
   - maxLength = 3

5. Continue this process...
   - Eventually reaching the longest sequence [1, 3, 2, 7, 4]
   - maxLength = 5

### 4. Time and Space Complexity
- Time Complexity: O(n)
  - Each element is added and removed at most once
- Space Complexity: O(k)
  - k is the size of the unique elements in the window

## Key Insights
1. The sliding window technique is perfect for finding sequential patterns
2. Using a HashSet provides O(1) lookup for duplicates
3. The window expands when adding unique elements and contracts when handling duplicates

## Common Pitfalls to Avoid
1. Forgetting to remove elements from the HashSet when sliding the window
2. Not handling the case when the longest sequence is at the end
3. Incorrect window size calculation (remember it's end - start + 1)
