# Longest increasing path in a matrix
## https://leetcode.com/problems/longest-increasing-path-in-a-matrix



# Implementation :
```java
class Solution {
    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0) return 0;
        int rows = matrix.length, cols = matrix[0].length;
        int[][] dp = new int[rows][cols];
        int res = 0;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (dp[i][j] == 0) {
                    dfs(matrix, i, j, dp, Integer.MIN_VALUE);
                    res = Math.max(res, dp[i][j]);
                }
            }
        }

        return res;
    }

    private int dfs(int[][] matrix, int row, int col, int[][] dp, int prev) {
        if (row > matrix.length - 1 || row < 0 ||
            col > matrix[0].length - 1 || col < 0 ||
           matrix[row][col] <= prev) return 0;
        if (dp[row][col] != 0) return dp[row][col];

        int left = dfs(matrix, row, col - 1, dp, matrix[row][col]);
        int right = dfs(matrix, row, col + 1, dp, matrix[row][col]);
        int up = dfs(matrix, row - 1, col, dp, matrix[row][col]);
        int down = dfs(matrix, row + 1, col, dp, matrix[row][col]);

        dp[row][col] = Math.max(left, Math.max(right, Math.max(up, down))) + 1;
        return dp[row][col];
    }
}

```

# References :
1. https://www.youtube.com/watch?v=ZAmxc4Qrqi8
2. https://evelynn.gitbooks.io/google-interview/longest_increasing_path_in_a_matrix.html
3. https://www.programcreek.com/2014/05/leetcode-longest-increasing-path-in-a-matrix-java
