## 题目

## 思路

## 源代码

```c++
class Solution {
public:
    vector<vector<string>> res;

    void dfs(int n, int row, vector<string>& chessboard) {
        if (row == n) {
            res.push_back(chessboard);
            return;
        }
        for (int col = 0; col < n; col++) {
            if (isValid(row, col, chessboard, n)) {
                chessboard[row][col] = 'Q';
                dfs(n, row + 1, chessboard);
                chessboard[row][col] = '.';
            }
        }
    }
    bool isValid(int row, int col, vector<string>& chessboard, int n) {
        int count = 0;
        //检查列(当前列的上面不能出现皇后)
        for (int i = 0; i < row; i++) {
            if (chessboard[i][col] == 'Q') {
                return false;
            }
        }
        //检查45°是否有皇后
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (chessboard[i][j] == 'Q') {
                return false;
            }
        }
        //检测135°是否有皇后
        for (int i = row - 1, j = col + 1; i >=0 && j < n; i--, j++) {
            if (chessboard[i][j] == 'Q') {
                return false;
            }
        }

        return true;
    }
    vector<vector<string>> solveNQueens(int n) {
        vector<string> chessboard(n, string(n, '.'));
        dfs(n, 0, chessboard);
        return res;
    }
};
```

