# Longest increasing path in a matrix
## https://leetcode.com/problems/longest-increasing-path-in-a-matrix



# Implementation 1 : Naive DFS (Time Limit Exceeded)
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

# References :
1. https://www.youtube.com/watch?v=ZAmxc4Qrqi8
2. https://evelynn.gitbooks.io/google-interview/longest_increasing_path_in_a_matrix.html
3. https://www.programcreek.com/2014/05/leetcode-longest-increasing-path-in-a-matrix-java
