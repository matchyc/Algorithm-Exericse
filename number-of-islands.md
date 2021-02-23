# number-of-islands 岛屿数量

给你一个由  '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

示例 1：

输入：grid = [
["1","1","1","1","0"],
["1","1","0","1","0"],
["1","1","0","0","0"],
["0","0","0","0","0"]
]
输出：1

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/number-of-islands
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 考虑

其实就是求一个图的连通分量，没别的了，时隔一年，还是能写一写。
**原始考虑**：使用了额外的 MxN 的空间，去保存每一个点的状态(有没有被访问过)，看了题解后发现，每当访问过后在原来的矩阵里将其置为 '0' 就可以了。
**值得学习优化的地方**：因为 bfs 和 dfs 都有上下左右访问的过程（开始时我尝试觉得，从左上角开始，每次递归就只用写右边和下边了，后来发现不对，因为会有点在左边...好傻）。但是当判断每一次的 i，j 有没有越界时，**不必在每次调用函数或操作时判断**是不是 i>0，因为只有 i > 0 才可以进入 i - 1 的流程 —— 我们只需要放心的写下 dfs(i - 1, j)的操作(bfs 同理)，然后再每次真正需要进入有访问的操作时，用大的if，框住i j 的范围(4个条件i大于小于，j大于小于)，这样就不用每次进入时重复写判断了。(下面我会给出例子)


## 傻傻判断dfs(且使用了额外空间)

g就是我用来标识已经被访问过的空间。

```c++
class Solution {
public:
    void solve(vector<vector<char>>& grid, vector<vector<int>>& g, int a, int b) {
        if(grid[a][b] == '0' || g[a][b] == 1)
            return;
        g[a][b] = 1;
        if(a < g.size() - 1) {
            solve(grid, g, a+1, b);
        }
        if(a > 0) {
            solve(grid, g, a-1, b);
        }
        if(b < g[0].size() - 1) {
            solve(grid, g, a, b+1);
        }
        if(b > 0) {
            solve(grid, g, a, b-1);
        }
    }

    int numIslands(vector<vector<char>>& grid) {
        int ans = 0;

        vector<vector<int>> g(grid.size(),vector<int>(grid[0].size(),0));

        for(int i = 0; i < grid.size(); ++i) {
            for(int j = 0; j < grid[0].size(); ++j) {
                if(grid[i][j] == '1' && g[i][j] == 0) {
                    solve(grid, g, i, j);
                    ++ans;
                }
            }
        }
        
        return ans;
    }
};
```

## 改进版dfs，原地修改标识已经访问过，且判断简洁，正如简介中所说

```C++
class Solution {
public:
    void solve(vector<vector<char>>& grid, int a, int b) {
        
        if(a >= 0 && a < grid.size() && b >= 0 && b < grid[0].size()) {
            if(grid[a][b] == '0')
                return;
            grid[a][b] = '0';
            solve(grid, a+1, b);
            solve(grid, a-1, b);
            solve(grid, a, b+1);
            solve(grid, a, b-1);
        }
    }

    int numIslands(vector<vector<char>>& grid) {
        int ans = 0;

        for(int i = 0; i < grid.size(); ++i) {
            for(int j = 0; j < grid[0].size(); ++j) {
                if(grid[i][j] == '1') {
                    solve(grid,i, j);
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

## BFS bfs一般用到queue，也算是复习了

```c++
class Solution {
public:
    void solve(vector<vector<char>>& g, int a, int b) {
        std::queue<pair<int,int>> q;
        q.push(make_pair(a,b));
        while(!q.empty()) {
            std::pair<int,int> temp = q.front();
            a = temp.first;
            b = temp.second;
            if(a >= 0 && a < g.size() && b >= 0 && b < g[0].size() && g[a][b] == '1') {
                q.push({a - 1, b});
                q.push({a + 1, b});
                q.push({a, b - 1});
                q.push({a, b + 1});
                g[a][b] = '0';
            }
            q.pop();
        }
    }

    int numIslands(vector<vector<char>>& grid) {
        int ans = 0;

        for(int i = 0; i < grid.size(); ++i) {
            for(int j = 0; j < grid[0].size(); ++j) {
                if(grid[i][j] == '1') {
                    solve(grid, i, j);
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```