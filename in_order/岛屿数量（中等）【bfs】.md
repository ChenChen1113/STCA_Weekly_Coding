# 岛屿数量（中等）【bfs】

标签（空格分隔）： in_order

---
给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

示例 1:

    输入:
    11110
    11010
    11000
    00000
    输出: 1

示例 2:

    输入:
    11000
    11000
    00100
    00011
    输出: 3
    解释: 每座岛屿只能由水平和/或竖直方向上相邻的陆地连接而成。


### bfs：  
```c++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size();
        if(m == 0){return 0;}
        int n = grid[0].size();
        int res = 0;
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j ++){
                if(grid[i][j] == '1'){
                    res ++;
                    dfs(grid, i, j, m, n);
                }
            }
        }
        return res;
    }

    void dfs(vector<vector<char>>& grid, int i, int j, int m, int n){
        grid[i][j] = '0';
        if(i + 1 < m && grid[i+1][j] == '1') dfs(grid, i+1, j, m, n);
        if(j + 1 < n && grid[i][j+1] == '1') dfs(grid, i, j+1, m, n);
        if(i - 1 >= 0 && grid[i-1][j] == '1') dfs(grid, i-1, j, m, n);
        if(j - 1 >= 0 && grid[i][j-1] == '1') dfs(grid, i, j-1, m, n);
    }
};
```
