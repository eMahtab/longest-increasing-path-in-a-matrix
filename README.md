# Longest increasing path in a matrix
## https://leetcode.com/problems/longest-increasing-path-in-a-matrix

Given an integer matrix, find the length of the longest increasing path.

From each cell, you can either move to four directions: left, right, up or down. You may NOT move diagonally or move outside of the boundary (i.e. wrap-around is not allowed).
```
Example 1:

Input: nums = 
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
] 
Output: 4 
Explanation: The longest increasing path is [1, 2, 6, 9].

Example 2:

Input: nums = 
[
  [3,4,5],
  [3,2,6],
  [2,2,1]
] 
Output: 4 
Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.
```

# Implementation 1 : Naive DFS (Time Limit Exceeded) 😟
```java
public class Solution {
	private static final int[][] directions = { { 0, 1 }, { 0, -1 }, { -1, 0 }, { 1, 0 } };
	
	public int longestIncreasingPath(int[][] matrix) {
		if (matrix == null || matrix.length == 0)
			return 0;
            
		int rows = matrix.length, columns = matrix[0].length;

		int longestPath = 1;
		for (int row = 0; row < rows; row++) {
			for (int column = 0; column < columns; column++) {
				int length = dfs(matrix, row, column);
				longestPath = Math.max(longestPath, length);
			}
		}
		return longestPath;
	}

	private int dfs(int[][] matrix, int row, int column) {
		int pathLength = 0;
		for (int[] direction : directions) {
			int x = row + direction[0], y = column + direction[1];
			if (x >= 0 && x < matrix.length && y >= 0 && y < matrix[0].length && 
				matrix[x][y] > matrix[row][column]) {
				int length = dfs(matrix, x, y);
				pathLength = Math.max(pathLength, length);
			}
		}
		return pathLength + 1;
	}
}
```

# Implementation 2 : DFS (With Memoization)
```java
public class Solution {
	private static final int[][] directions = { { 0, 1 }, { 0, -1 }, { -1, 0 }, { 1, 0 } };
	
	public int longestIncreasingPath(int[][] matrix) {
		if (matrix == null || matrix.length == 0)
			return 0;

		int rows = matrix.length, columns = matrix[0].length;
		int[][] cache = new int[rows][columns];
		int longestPath = 1;
		for (int row = 0; row < rows; row++) {
			for (int column = 0; column < columns; column++) {
				int length = dfs(matrix, row, column, cache);
				longestPath = Math.max(longestPath, length);
			}
		}
		return longestPath;
	}

	private int dfs(int[][] matrix, int row, int column, int[][] cache) {
		if (cache[row][column] != 0) return cache[row][column];
		
		for (int[] direction : directions) {
			int x = row + direction[0], y = column + direction[1];
			if (x >= 0 && x < matrix.length && y >= 0 && y < matrix[0].length && 
				matrix[x][y] > matrix[row][column]) {
				int length = dfs(matrix, x, y, cache);
				cache[row][column] = Math.max(cache[row][column], length);
			}
				
		}
		cache[row][column] = cache[row][column] + 1;
		return cache[row][column];
	}
}
```

# Implementation 3 
```java
class Solution {
    public int longestIncreasingPath(int[][] matrix) {
        if(matrix == null || matrix.length == 0)
            return 0;
        int longestPath = 1;
        int rows = matrix.length;
        int cols = matrix[0].length;
        int[][] cache = new int[rows][cols];
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                int result = dfs(matrix, i, j, matrix[i][j] - 1, cache);
                longestPath = Math.max(longestPath,  result);
            }
        }
        return longestPath;
    }
    
    private int dfs(int[][]matrix, int row, int col, int prevValue, int[][] cache) {
        if(row < 0 || row >= matrix.length || col < 0 || col >= matrix[0].length || matrix[row][col] <= prevValue)
            return 0;
        if(cache[row][col] != 0)
            return cache[row][col];
        int right = dfs(matrix, row, col + 1, matrix[row][col], cache);
        int left = dfs(matrix, row, col - 1, matrix[row][col], cache);
        int up = dfs(matrix, row - 1, col, matrix[row][col], cache);
        int down = dfs(matrix, row + 1, col, matrix[row][col], cache);
        
        int max = Math.max(right, left);
        max = Math.max(max, up);
        max = Math.max(max, down);
        cache[row][col] = 1 + max;
        return cache[row][col];
    }
    
}

```


# References :
1. https://www.youtube.com/watch?v=ZAmxc4Qrqi8
2. https://leetcode.com/articles/longest-increasing-path-matrix
3. https://www.programcreek.com/2014/05/leetcode-longest-increasing-path-in-a-matrix-java
