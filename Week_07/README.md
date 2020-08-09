学习笔记
单词搜索：

class Solution {
public:
    struct node {
        int id;                    //记录该node是否为某个单词的结尾字符
        ind cnt;                    //记录有几个单词经过该node
        node* children[26];
        node() {
            cnt = 0;
            id = -1;
            memset(children, 0, sizeof(children));
        }
        ~node() {
            for (node* child : children)
                delete child;
        }
    };
    
    int dirs[4][2] = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};
    int backtrace(vector<vector<char>>& board, vector<string>& word, node* cur, int i, int j, vector<string>& res) {
        node* child = cur -> children[board[i][j] - 'a'];
        if (!child) return 0;
        int ans = 0;                      //计算经过该node匹配到的单词数量
        if (child -> id != -1) {
            res.push_back(words[child -> id]);
            child -> id = -1;
            ans++；
        }
        char c = board[i][j];
        board[i][j] = '#';
        for (int k = 0; k < 4; k++) {
            int x = i + dirs[k][0], y = j + dirs[k][1];
            if (x >= 0 && x < board.size() && y >= 0 && y < board[0].size() && board[x][y] != '#') {
                ans += backtrace(board, words, child, x, y, res);
            }
        }
        board[i][j] = c;
        child -> cnt -= ans;
        if (child -> cnt == 0) {
            delete child;
            cur -> children[board[i][j] - 'a'] = nullptr;
        }
        return ans;
    }
    
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        vector<string> res;
        node* root = new node();
        for (int i = 0; i < words.size(); i++) {
            node* cur = root;
            for (char c : word[i]) {
                if (!cur -> children[c - 'a']) cur -> children[c - 'a'] = new node();
                cur = cur -> children[c - 'a'];
                cur -> cnt ++;
            }
            cur -> id = i;
        }
        
        for (int i = 0; i < board.size(); i++) {
            for (int j = 0; j < board[0].size(); j++) {
                backtrace(board, words, root, i, j, res);
            }
        }
        return res;
    }
}


朋友圈

1.DFS

```c++
/*思路一： 从图的角度进行思考，其实就是一个无向图中连通分量个数的问题*/
/* 1. 深度优先搜索 */
class Solution {
public:
    void dfs(vector<vector<int>>& M, vector<int>& visited, int n) {
        visited[n] = 1;
        for (int i = 0; i < M.size(); i++) 
            if (M[n][j] && !visited[i][j]) dfs(M, visited, j)
                dfs(M, visited, j);
    }
    
    int findCircleNum(vector<vector<int>>& M) {
        vector<int> visited(M.size(), 0);
        int count = 0;
        for (int i = 0; i < M.size(); i++) {
            if (!visit[i]) {
                dfs(M, visit, i);
                count++;
            }
        }
        return count;
    }
}
```

2. BFS 广度优先搜索

```c++
class Solution {
public:
	int findCircleNum(vector<vector<int>>& M) {
        vector<int> visit(M.size(), 0);
        int count = 0, temp;
        queue<int> q;
        for (int i = 0; i < M.size(); i++) {  // i 代表第i个人
            if (!visit[i]) {
                count++;
                q.push(i);
                while (!q.empty()) {
                    temp = q.front();
                    q.pop();
                    visit[temp] = 1;
                    for (int j = 0; j < M.size(); j++)   // --
                        if (M[temp][j] && !visit[j])
                            q.push(j);
                }
            }
        }
        return count;
    }
}
```

3. 并查集: 利用并查集的思想。主要有两个操作，一个是find，另一个是union；

   find的作用是找到一个元素所在的集合的代表元素，主要是一个递归的过程；

   union的作用是将两个元素所在的集合合并成一个集合，即是将其中一个元素的代表元素变成另一个元素的代表元素

   ```c++
   class Solution {
   public:
   	int find(vector<int>& vec, int n) {
           if (vec[n] == -1) return n;
           return find(vec, vec[n]);
       }
       
       void Union(vector<int>& vec, int m, int n) {
           int parent_m = find(vec, m);
           int parent_n = find(vec, n);
           if (parent_m != parent_n) vec[parent_m] = parent_n;
       }
       
       int findCircleNum(vector<vector<int>>& M) {
           vector<int> parent(M.size(), -1);
           for (int i = 0; i < M.size(); i++)  {
               for (int j = 0; j < M[0].size(); j++) {
                   if (M[i][j] == 1 && i != j) Union(parent, i, j);
               }
           }
           int count = 0;
           for (int i = 0; i < M.size(); i++)
               if (parent[i] ==  -1) count++;
           return count;
       }
   };
   ```

   
