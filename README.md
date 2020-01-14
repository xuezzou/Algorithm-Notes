### Lintcode
> [Lintcode Ladder](https://www.lintcode.com/ladder/1/)

#### Content
- [Hack the Algorithm Interview](#hack-the-algorithm-interview)
- [Breadth First Search](#breadth-first-search)
- [Binary Search & log(n) Algorithm](#binary-search-logn-algorithm)
- [Binary Tree - Divide & Traverse](#binary-tree-divide-traverse)
- [Two-pointer](#two-pointer)
- [Implicit Graph DFS](#implicit-graph-dfs)
- [Data Structure - Stack, Queue, Hash, Heap](#hash--heap)
- [Memorization Search & Dynamic Programming](#memorization-search--dynamic-programming)
- [DP](#dp)
- [Additional Problems](#additional-problems)
- [Others](#others)
 
---

### Hack the Algorithm Interview
1. [Longest Palindrome](https://www.lintcode.com/problem/longest-palindrome/description?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param s: a string which consists of lowercase or uppercase letters
     * @return: the length of the longest palindromes that can be built
     */
    int longestPalindrome(string & s) {
        int result = 0;
        bool hasOdd = false;
        std::unordered_map<char, int> count;
        for(char ch: s) {
            ++count[ch];
        }
        for(std::unordered_map<char,int>::iterator it = count.begin(); it != count.end();++it) {
            if(it->second % 2 == 0) {
                result += it->second;
            } else {
                hasOdd = true;
                result += it->second - 1;
            }
        }
        return hasOdd ? result + 1 : result;
    }
    ```
2. [Implement strStr()](https://www.lintcode.com/problem/implement-strstr/description?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param source: 
     * @param target: 
     * @return: return the index
     */
    int strStr(string &source, string &target) {
        if(target.length() == 0) {
            return 0;
        }
        for(int i = 0; i < (int)source.length() - (int)target.length() + 1; ++i) {
            bool isFound = true;
            for(int j = 0; j < target.length() && isFound; ++j) {
                if(target[j] != source[i + j]) {
                    isFound = false;
                }
            }
            if(isFound) {
                return i;
            }
        }
        return -1;
    }
    ```
3. [Valid Palindrome](https://www.lintcode.com/problem/valid-palindrome/description?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param s: A string
     * @return: Whether the string is a valid palindrome
     */
    bool isPalindrome(string &s) {
        // if(s.length() == 0) {
        //     return true;
        // }
        int start = 0, end = s.length() - 1;
        while(start < end) {
            while(!isalnum(s[start])) {
                ++start;
            }
            while(!isalnum(s[end])){
                --end;
            }
            if(start < end && tolower(s[start++]) != tolower(s[end--])) {
                return false;
            }
        }
        return true;
    }
    ```
4. [Longest Palindromic Substring](https://www.lintcode.com/problem/longest-palindromic-substring/description?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param s: input string
     * @return: the longest palindromic substring
     */
    // using dynamic programming
    string longestPalindrome(string &s) {
        // Dynamic Programming
        int len = s.length();
        if(len <= 1) {
            return s;
        }
        bool isPalindrome[len][len] = {false}; // all initailze to false
        int longestLen = 1, startIndex = 0;
        // initailization
        for(int i = 0; i < len; ++i) {
            isPalindrome[i][i] = true;
            if(i < len - 1 && s[i] == s[i + 1]){
                isPalindrome[i][i + 1] = true;
                startIndex = i;
                longestLen = 2;
            }
        }
        // dynamic programming
        for(int i = len - 3; i >= 0; --i) {
            for(int j = i + 2; j < len; ++j) {
                if(isPalindrome[i + 1][j - 1] && (s[i] == s[j])) {
                    isPalindrome[i][j] = true;
                    if(j - i + 1 > longestLen) {
                        startIndex = i;
                        longestLen = j - i + 1;

                    }
                }
            }
        }
        return s.substr(startIndex, longestLen);
    }
    ```

### Breadth First Search
1. [Number of Islands](https://www.lintcode.com/problem/number-of-islands/description?_from=ladder&&fromId=1)
    ```c++
    int numIslands(vector<vector<bool>> &grid) {
        if(grid.size() == 0 || grid[0].size() == 0) {
            return 0;
        }
        int count = 0;
        for(int i = 0; i < grid.size(); ++i) {
            for(int j = 0; j < grid[0].size(); ++j) {
                if(grid[i][j] == 1) {
                    // bfs(i, j, grid);
                    dfs(i, j, grid);
                    ++count;
                }
            }
        }
        return count;
    }
    
    void dfs(int i, int j, vector<vector<bool>> & grid) {
        if(invalid(i, j, grid)) {
            return;
        }
        grid[i][j] = 0;
        dfs(i, j + 1, grid);
        dfs(i, j - 1, grid);
        dfs(i + 1, j, grid);
        dfs(i - 1, j, grid);
    }
    
    void bfs(int i, int j, vector<vector<bool>> & grid) {
        if(invalid(i, j, grid)) {
            return;
        }
        queue<vector<int>> queue;
        queue.push({i, j});
        grid[i][j] = 0;
        while(!queue.empty()) {
            i = queue.front()[0];
            j = queue.front()[1];
            queue.pop();
            explore_bfs(i, j + 1, grid, queue);
            explore_bfs(i, j - 1, grid, queue);
            explore_bfs(i + 1, j, grid, queue);
            explore_bfs(i - 1, j, grid, queue);
        }
    }
    
    void explore_bfs(int i, int j, vector<vector<bool>> & grid, queue<vector<int>> & queue) {
        if(invalid(i, j, grid)) {
            return;
        }
        queue.push({i, j});
        grid[i][j] = 0;
    }
    
    bool invalid(int i, int j, vector<vector<bool>> & grid) {
        return i< 0 || j < 0 || i >= grid.size() || j >= grid[0].size() || grid[i][j] == 0;
    }
    ```
2. [Binary Tree Level Order Traversal](https://www.lintcode.com/problem/binary-tree-level-order-traversal/description?_from=ladder&&fromId=1)
    ```c++
    vector<vector<int>> levelOrder(TreeNode * root) {
            vector<vector<int>> results;
            if(root == nullptr) {
                return results;
            }
            queue<TreeNode *> queue;
            queue.push(root);
            while(!queue.empty()) {
                vector<int> level;
                int size = queue.size();
                while(size-- > 0) {
                    TreeNode * curNode = queue.front();
                    queue.pop();
                    level.push_back(curNode->val); // position 1
                    if(curNode->left) {
                        queue.push(curNode->left);
                    }
                    if(curNode->right) {
                        queue.push(curNode->right);
                        
                    }
                }
                results.push_back(level);
            }
            return results;
        }
    ```
3. [Course Schedule](https://www.lintcode.com/problem/course-schedule/?_from=ladder&&fromId=1)
    ```c++
        /*
         - @param numCourses: a total of n courses
         - @param prerequisites: a list of prerequisite pairs
         - @return: true if can finish all courses or false
         */
        bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
            return bfs_topological_sort(numCourses, prerequisites);
            // return dfs_no_cycle(numCourses, prerequisites);
        }
        
        // reference: https://www.geeksforgeeks.org/detect-cycle-direct-graph-using-colors/
        bool dfs_no_cycle(int numCourses, vector<pair<int, int>> & prereq) {
            // first build the graph
            vector<unordered_set<int>> out_edges(numCourses);
            for(int i = 0; i < prereq.size(); ++i) {
                out_edges[prereq[i].second].insert(prereq[i].first);
            }
            vector<int> color(numCourses, 0);
            for(int i = 0; i < numCourses; ++i) {
                if(color[i] == 0) {
                    if(has_cycle(i, out_edges, color)) {
                        return false;
                    }
                }
            }
            return true;
        }
        
        // 0 represents not visited and 1 represnts current being processed
        // 2 represented finished it is being processed (and no need to do it again)
        // related to BACK EDGE, forming a back edge leads to a cycle
        bool has_cycle(int curNode, vector<unordered_set<int>> & out_edges, vector<int> & color) {
            color[curNode] = 1;
            for(unordered_set<int>::iterator it = out_edges[curNode].begin(); it != out_edges[curNode].end(); ++it) {
                if(color[*it] == 2) {
                    continue;
                }
                if(color[*it] == 1) {
                    return true;
                }
                if(has_cycle(*it, out_edges, color)) {
                    return true;
                }
            }
            color[curNode] = 2;
            return false;
        }

        bool bfs_topological_sort(int numCourses, vector<pair<int, int>> & prereq) {
            vector<unordered_set<int>> out_edges(numCourses); // sucessors b<-a
            vector<int> indegree(numCourses, 0);
            for(int i = 0; i < prereq.size(); ++i) {
                out_edges[prereq[i].second].insert(prereq[i].first);
            }
            // take the indegree part out to avoid duplication
            for(int i = 0; i < numCourses;++i) {
                for(unordered_set<int>::iterator it = out_edges[i].begin(); it != out_edges[i].end(); ++it) {
                    ++indegree[*it];
                }
            }
            queue<int> queue;
            // find indegree 0
            for(int i = 0; i < numCourses; ++i) {
                if(indegree[i] == 0) {
                    queue.push(i);
                }
            }
            int node = 0;
            // pop element out of the queue and find the next indegree 0 and minus its outedges' indegree by 1
            while(!queue.empty()) {
                int cur = queue.front();
                queue.pop();
                ++node;
                for(unordered_set<int>::iterator it = out_edges[cur].begin(); it != out_edges[cur].end(); ++it) {
                    if(--indegree[*it] == 0) {
                        queue.push(*it);
                    }
                }
            }
            return numCourses == node;
        }
    ```
4. [Course Schedule II](https://www.lintcode.com/problem/course-schedule-ii/description?_from=ladder&&fromId=1)
    ```c++
        vector<int> findOrder(int numCourses, vector<pair<int, int>> &prerequisites) {
            vector<unordered_set<int>> outEdges(numCourses);
            vector<int> indegree(numCourses, 0);
            
            vector<int> coursesOrdered;
            
            for(int i = 0; i < prerequisites.size(); ++i) {
                outEdges[prerequisites[i].second].insert(prerequisites[i].first);
            }
            
            for(int i = 0; i < numCourses; ++i) {
                for(unordered_set<int>::iterator it = outEdges[i].begin(); it != outEdges[i].end(); ++it){
                    ++indegree[*it];
                }
            }
            
            int node = 0;
            queue<int> queue;
            for(int i = 0; i < numCourses; ++i) {
                if(indegree[i] == 0) {
                    queue.push(i);
                }
            }
            
            while(!queue.empty()) {
                int curNode = queue.front();
                queue.pop();
                ++node;
                coursesOrdered.push_back(curNode);
                for(unordered_set<int>::iterator it = outEdges[curNode].begin(); it != outEdges[curNode].end(); ++it){
                    if(--indegree[*it] == 0) {
                        queue.push(*it);
                    }
                }
                
            }
            
            if(node != numCourses) {
                return {};
            } else {
                return coursesOrdered;
            }
        }
    ```
5. [Knight Shortest Path](https://www.lintcode.com/problem/knight-shortest-path/description?_from=ladder&&fromId=1)
    ```c++
        vector<vector<int>> directions = {{-2, -1}, {-2, 1}, {-1, 2}, {1, 2}, {2, 1}, {2, -1}, {1, -2}, {-1, -2}};

        int shortestPath(vector<vector<bool>> &grid, Point &source, Point &destination) {
            int step = 0;
            
            queue<Point> queue;
            queue.push(source);
            while(!queue.empty()){
                int size = queue.size();
                while(--size >= 0) {
                    Point curPoint = queue.front();
                    queue.pop();
                    if(curPoint.x == destination.x && curPoint.y == destination.y) {
                        return step;
                    }
                    for(int i = 0; i < directions.size(); ++i) {
                        Point newPoint = Point(curPoint.x + directions[i][0], curPoint.y + directions[i][1]);
                        if(valid(newPoint, grid)) {
                            queue.push(newPoint);
                            grid[newPoint.x][newPoint.y] = 1;
                        }
                    }
                }
                ++step;
            }
            return -1;
        }

        bool valid(Point & point, vector<vector<bool>> &grid) {
            return point.x >= 0 && point.y >= 0 && point.x < grid.size() && point.y < grid[0].size() && grid[point.x][point.y] == 0;
        }
    ```
6. [Sequence Reconstruction](https://www.lintcode.com/problem/sequence-reconstruction/description?_from=ladder&&fromId=1)
    ```c++
        /**
         - @param org: a permutation of the integers from 1 to n
         - @param seqs: a list of sequences
         - @return: true if it can be reconstructed only one or false
         */
        bool sequenceReconstruction(vector<int> &org, vector<vector<int>> &seqs) {
            // edge cases
            int numCourses = org.size();
            if(numCourses == 0) {
                return seqs.size() == 0 || seqs[0].size() == 0;
            }
            if(numCourses== 1) {
                return seqs.size() == 1 && seqs[0].size() == 1 && seqs[0][0] == org[0];
            }
            
            // first construct the graph from seqs
            vector<unordered_set<int>> graph(numCourses + 1);
           
            for(int i = 0; i < seqs.size(); ++i) {
                for(int j = 1; j < seqs[i].size(); ++j){
                    // to make sure number in seqs are valid
                    if(seqs[i][j] <= 0 || seqs[i][j] > numCourses ) {
                        return false;
                    }
                    graph[seqs[i][j - 1]].insert(seqs[i][j]);
                }
            }
            
            // compute indegree
            vector<int> indegree(numCourses + 1, 0);
            for(int i = 1; i < numCourses + 1; ++i) {
                for(unordered_set<int>::iterator it = graph[i].begin(); it != graph[i].end(); ++it){
                    ++indegree[*it];
                }
            }
            
            // perform topological sorting
            queue<int> queue;
            // find degree w/ 0
            for(int i = 1; i < numCourses + 1; ++i) {
                if(indegree[i] == 0){
                    queue.push(i);
                }
            }
                
            int node = 0;
            while(queue.size() == 1) {
                int cur = queue.front();
                queue.pop();
                if(org[node++] != cur) {
                    return false;
                }
                // minus 1 from all adjacent
                for(unordered_set<int>::iterator it = graph[cur].begin(); it != graph[cur].end(); ++it) {
                    if(--indegree[*it] == 0) {
                        queue.push(*it);
                    }
                }
            }
            return node == numCourses;
        }
    ```
7. [Clone Graph](https://www.lintcode.com/problem/clone-graph/description?_from=ladder&&fromId=1)
    ```c++
    /**
     - Definition for undirected graph.
     - struct UndirectedGraphNode {
     -     int label;
     -     vector<UndirectedGraphNode *> neighbors;
     -     UndirectedGraphNode(int x) : label(x) {};
     - };
     */
    ```
    ```c++
        UndirectedGraphNode* cloneGraph(UndirectedGraphNode* node) {
            // some points
            // map is from the existed one to the copied but one not vise versa
            // can only create it when queue it or lose linking
            // only queue it if not created before -> dead loop
            // remember to link it back even if it is created before
            if(!node) {
                return node;
            }
            // copy root first
            // build a map to map the node being copied and copied ones to avoid multiple copies
            UndirectedGraphNode* root_c = new UndirectedGraphNode(node->label);
            unordered_map<UndirectedGraphNode*, UndirectedGraphNode*> copy;
            copy[node] = root_c;
            
            // queue to build bfs
            queue<UndirectedGraphNode*> todo;
            todo.push(node);
            
            while(!todo.empty()) {
                UndirectedGraphNode* cur = todo.front();
                todo.pop();
                for(UndirectedGraphNode * neighbor: cur->neighbors) {
                    if(!copy.count(neighbor)) {
                        UndirectedGraphNode * newNeighbor = new UndirectedGraphNode(neighbor->label);
                        copy[neighbor] = newNeighbor;
                        todo.push(neighbor);
                    } 
                    copy[cur]->neighbors.push_back(copy[neighbor]);
                }
            }
            
            return root_c;
        }
    ```
8. [Topological Sorting](https://www.lintcode.com/problem/topological-sorting/description?_from=ladder&&fromId=1)
    ```c++
    /**
     - Definition for Directed graph.
     - struct DirectedGraphNode {
     -     int label;
     -     vector<DirectedGraphNode *> neighbors;
     -     DirectedGraphNode(int x) : label(x) {};
     - };
     */
    ```
    ```c++
        vector<DirectedGraphNode*> topSort(vector<DirectedGraphNode*>& graph) {
            // bfs
            // counting degree
            unordered_map<DirectedGraphNode*, int> indegree;
            for(DirectedGraphNode* node: graph) {
                for(DirectedGraphNode* neighbor: node->neighbors) {
                    ++indegree[neighbor];
                }
            }
            
            // initailzie a queue and find degree 0
            queue<DirectedGraphNode*> queue;
            // important: not contain in the map if indegree is 0!!!
            queue<DirectedGraphNode *> queue;
            for(DirectedGraphNode* node: graph) {
                if(!indegree.count(node)) {
                    queue.push(node);
                }
            }
            vector<DirectedGraphNode*> result;
            while(!queue.empty()) {
                DirectedGraphNode * cur = queue.front();
                queue.pop();
                result.push_back(cur);
                // explore its adjacency
                for(DirectedGraphNode* neighbor: cur->neighbors) {
                    if(--indegree[neighbor] == 0) {
                        queue.push(neighbor);
                    }
                }
                
            }
            return result;
        }
    ```
9. [Serialize and Deserialize Binary Tree](https://www.lintcode.com/problem/serialize-and-deserialize-binary-tree/description?_from=ladder&&fromId=1)
    ```c++
        string serialize(TreeNode * root) {
            if(root == nullptr) {
                return "{}";
            }
            // build the result string using bfs
            string result = "";
            queue<TreeNode*> queue;
            queue.push(root);
            while(!queue.empty()) {
                // int size = queue.size();
                // for(int i = 0; i < size; ++i) {
                    if (queue.front() == nullptr) {
                        result += "#,";
                        queue.pop();
                    } else {
                        TreeNode* cur = queue.front();
                        queue.pop();
                        result += std::to_string(cur->val) + ",";
                        
                        queue.push(cur->left);
                        queue.push(cur->right);
                    }
            }
            result[result.length() - 1] = '}';
            return "{" + result;
        }

        /**
         - This method will be invoked second, the argument data is what exactly
         - you serialized at method "serialize", that means the data is not given by
         - system, it's given by your own serialize method. So the format of data is
         - designed by yourself, and deserialize it here as you serialize it in 
         - "serialize" method.
         */
        TreeNode * deserialize(string &data) {
            if(data == "{}") {
                return nullptr;
            }

            queue<TreeNode*> queue;
            vector<string> tree;
            // going through the string and build the tree stored in a vector
            for(int i = 1, j = 1; i < data.length(); ++i){
                if(data[i] == ',' || data[i] == '}') {
                    // build it here
                    string val = data.substr(j, i - j);
                    j = i + 1;
                    tree.push_back(val);
                } 
            }

            int index = 0;
            TreeNode* root = new TreeNode(stoi(tree[index++]));
            queue.push(root);
            while(!queue.empty()) {
                TreeNode* curNode = queue.front();
                queue.pop();
                if(tree[index] != "#"){
                    curNode->left = new TreeNode(stoi(tree[index++]));
                    queue.push(curNode->left);
                } else {
                    curNode->left = nullptr;
                    ++index;
                }
                if(tree[index] != "#"){
                    curNode->right = new TreeNode(stoi(tree[index++]));
                    queue.push(curNode->right);
                } else {
                    curNode->right = nullptr;
                    ++index;
                }
            }
            return root;
        }
    ```
10. [Word Ladder](https://www.lintcode.com/problem/word-ladder/?_from=ladder&&fromId=1)
    ```c++
        int ladderLength(string &start, string &end, unordered_set<string> &dict) {
            // edge case
            if(start == end) {
                return 1;
            }
            if(start.length() == 0 || start.length() != end.length()) {
                return 0;
            }
            int word_size = start.length();
            queue<string> queue;
            queue.push(start);
            int step = 1;
            while(!queue.empty()) {
                int level_size = queue.size();
                ++step;
                while(level_size-- > 0) {
                    string cur_word = queue.front();
                    queue.pop();
                    for(int i = 0; i < word_size; ++i){
                        char old_ch = cur_word[i];
                        for(char new_ch = 'a'; new_ch <= 'z'; ++new_ch) {
                            if(new_ch != old_ch) {
                                cur_word[i] = new_ch;
                                if(cur_word == end) {
                                    return step;
                                }
                                if(dict.find(cur_word) != dict.end()) {
                                    queue.push(cur_word);
                                    dict.erase(cur_word);
                                }
                                
                            }
                        }
                        cur_word[i] = old_ch;
                    }
                }
            }
            return 0;
        }
    ```

---

### Binary Search & logN Algorithm
1. [Last Position of Target](https://www.lintcode.com/problem/last-position-of-target/description?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param nums: An integer array sorted in ascending order
     * @param target: An integer
     * @return: An integer
     */
    int lastPosition(vector<int> &nums, int target) {
        if(nums.size() == 0) {
            return -1;
        }
        int start = 0, end = nums.size() - 1;
        
        while(start + 1 < end) {
            int mid = start + (end - start) / 2;
            if(target >= nums[mid]) {
                start = mid;
            } else {
                end = mid;
            }
        } 

        if(nums[end] == target) {
            return end;
        }
        if(nums[start] == target) {
            return start;
        }
        return -1;
    }
    ```
2.[First Position of Target](https://www.lintcode.com/problem/first-position-of-target/description?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param nums: The integer array.
     * @param target: Target to find.
     * @return: The first position of target. Position starts from 0.
     */
    int binarySearch(vector<int> &nums, int target) {
        if(nums.size() == 0) {
            return -1;
        }
        int start = 0, end = nums.size() - 1;
        while(start + 1 < end) {
            int mid = start + (end - start) / 2;
            if(target <= nums[mid]) {
                end = mid;
            } else {
                start = mid;
            }
            
        }
        if(nums[start] == target) {
            return start;
        }
        if(nums[end] == target) {
            return end;
        }
        return -1;
    }
    ```
3. [Maximum Number in Mountain Sequence](https://www.lintcode.com/problem/maximum-number-in-mountain-sequence/description?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param nums: a mountain sequence which increase firstly and then decrease
     * @return: then mountain top
     */
    int mountainSequence(vector<int> &nums) {
        if(nums.size() == 0) {
            return 0;
        } 
        int start = 0, end = nums.size() - 1;
        while(start + 1 < end) {
            int mid = start + (end - start) / 2;
            if(nums[mid] > nums[mid - 1] && nums[mid] > nums[mid + 1]) {
                return nums[mid];
            } else if (nums[mid] > nums[mid - 1]){
                start = mid;
            } else {
                end = mid;
            }
        }
        return max(nums[start], nums[end]);
    }
    ```
4. [Find K Closest Elements](https://www.lintcode.com/problem/find-k-closest-elements/description?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param A: an integer array
     * @param target: An integer
     * @param k: An integer
     * @return: an integer array
     */
    vector<int> kClosestNumbers(vector<int> &A, int target, int k) {
        vector<int> results(k);
        if(A.size() == 0 || k == 0) {
            return results;
        }
        // find the two nearest elements of target
        int start = 0, end = A.size() - 1;
        while(start + 1 < end) {
            int mid = start + (end - start) / 2;
            if(target <= A[mid]) {
                end = mid;
            } else {
                start = mid;
            }
        }
        // now start and end are the left and right pos of the target
        int count = 0;
        while(start >= 0 && end < A.size() && count < k) {
            if(target - A[start] <=  A[end] - target) {
                results[count++] = A[start--];
            } else {
                results[count++] = A[end++];
            }
        }
        while(start >= 0 && count < k) {
            results[count++] = A[start--];
        }
        while(end < A.size() && count < k) {
            results[count++] = A[end++];
        }
        return results;
        
    }
    ```
5. [Search in a Big Sorted Array](https://www.lintcode.com/problem/search-in-a-big-sorted-array/description?_from=ladder&&fromId=1)
    ```c++
    /*
     * @param reader: An instance of ArrayReader.
     * @param target: An integer
     * @return: An integer which is the first index of target.
     */
    int searchBigSortedArray(ArrayReader * reader, int target) {
        // first get the upper bound
        int end = 1;
        while(reader->get(end) < target) {
            end *= 2;
        }
        // lower bound 
        int start = end / 2;
        while(start + 1 < end) {
            int mid = start + (end - start) / 2;
            if(target <= reader->get(mid)) {
                end = mid;
            } else {
                start = mid;
            }
        }
        if(reader->get(start) == target) {
            return start;
        }
        if(reader->get(end) == target) {
            return end;
        }
        return -1;

    }
    ```
6. [Pow(x, n)](https://www.lintcode.com/problem/powx-n/description?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param x the base number
     * @param n the power number
     * @return the result
     */
    // iteration
    // n / 2  ==  n >> 1 
    // n % 2 == n&1
    double myPow(double x, int n) {
        double result = 1;
        long m = n;
        if(n < 0) {
            x = 1 / x;
            m = -m;
        }
        while(m != 0) {
            if(m % 2 == 1) {
                result *= x;
            }
            x *= x;
            m /= 2;
        }
        return result;
    }
    ```
7. [Find Minimum in Rotated Sorted Array](https://www.lintcode.com/problem/find-minimum-in-rotated-sorted-array/description?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param nums: a rotated sorted array
     * @return: the minimum number in the array
     */
    int findMin(vector<int> &nums) {
        if(nums.size() == 0) {
            return -1;
        }
        int start = 0, end = nums.size() - 1;
        int target = nums[end];
        while(start + 1 < end) {
            int mid = start + (end - start) / 2;
            if(nums[mid] <= target) { // or here we can set target to nums[end]
                end = mid;
            } else {
                start = mid;
            }
        }
        return min(nums[start], nums[end]);
    }
    ```
8. [Fast Power](https://www.lintcode.com/problem/fast-power/?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param a: A 32bit integer
     * @param b: A 32bit integer
     * @param n: A 32bit integer
     * @return: An integer
     */
    int fastPower(int a, int b, int n) {
        long ans = 1, tmp = a;
        
        while (n != 0) {
            if (n % 2 == 1) {
                ans = (ans * tmp) % b;
            }
            tmp = (tmp * tmp) % b;
            n = n / 2;
        }
        
        return (int) ans % b;
    }
    ```
9. [Find Peak Element](https://www.lintcode.com/problem/find-peak-element/description?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param A: An integers array.
     * @return: return any of peek positions.
     */
    int findPeak(vector<int> &A) {
        if(A.size() == 0) {
            return 0;
        }
        int start = 0, end = A.size() - 1;
        while(start + 1 < end) {
            int mid = start + (end - start) / 2;
            if(nums[mid] > nums[mid + 1] && nums[mid] > nums[mid - 1]) {
                return mid;
            } 
            if(nums[mid] > nums[mid - 1]) {
                start = mid;
            } else {
                end = mid;
            }
        }
        return max(nums[start], nums[end]);
    }
    ```
10. [First Bad Version](https://www.lintcode.com/problem/first-bad-version/description?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param n: An integer
     * @return: An integer which is the first bad version.
     */
    int findFirstBadVersion(int n) {
        int start = 0, end = n;
        while(start + 1 < end) {
            int mid = start + (end - start) / 2;
            if(SVNRepo::isBadVersion(mid)) {
                end = mid;
            } else {
                start = mid;
            }
        }
        if(SVNRepo::isBadVersion(start)) {
            return start;
        }
        return end;
    }
    ```
11. [Search in Rotated Sorted Array](https://www.lintcode.com/problem/search-in-rotated-sorted-array/?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param A: an integer rotated sorted array
     * @param target: an integer to be searched
     * @return: an integer
     */
    int search(vector<int> &A, int target) {
        if(A.size() == 0) {
            return -1;
        }
        int start = 0, end = A.size() - 1;
        while(start + 1 < end) {
            int mid = start + (end - start) / 2;
            if(A[mid] == target) {
                return mid;
            }
            if(A[start] < A[mid]) {
                if(A[start] <= target && target <= A[mid]) {
                    end = mid;
                } else {
                    start = mid;
                }
            } else {
                if(A[mid] < target && target <= A[end]) {
                    start = mid;
                } else {
                    end = mid;
                }
            }
        }
        if(target == A[start]) {
            return start;
        }
        if(target == A[end]) {
            return end;
        }
        return -1;
    }
    ```
12.[Search a 2D Matrix II](https://www.lintcode.com/problem/search-a-2d-matrix-ii/description?_from=ladder&&fromId=1)
    ```c++
    /**
         - @param matrix: A list of lists of integers
         - @param target: An integer you want to search in matrix
         - @return: An integer indicate the total occurrence of target in the given matrix
         */
        int searchMatrix(vector<vector<int>> &matrix, int target) {
            int row = matrix.size();
            if(row == 0) {
                return 0;
            } 
            int col = matrix[0].size();
            if(col == 0) {
                return 0;
            }
            int count = 0;
            int  curRow = row - 1, curCol = 0;
            while(curRow >= 0 && curCol < col) {
                if(target == matrix[curRow][curCol]) {
                    ++count;
                    --curRow;
                    ++curCol;
                } else if(target < matrix[curRow][curCol]) {
                    --curRow;
                } else {
                    ++curCol;
                }
            }
            return count;
        }
    ```


---

### Binary Tree - Divide & Traverse
1. [Closest Binary Search Tree Value](https://www.lintcode.com/problem/closest-binary-search-tree-value/description?_from=ladder&&fromId=1)
    ```c++
        int closestValue(TreeNode * root, double target) {
            if(root == nullptr) {
                return -1;
            }
            int res = root->val;
            
            // iterative solution
            // while(root) {
            //     if(abs(res - target) > abs(root->val - target)) {
            //         res = root->val;
            //     }
            //     root = target > root->val ? root->right : root->left;
            // }
            
            // recursion below
            if(root->right && target > root->val) {
                int r = closestValue(root->right, target);
                if(abs(res - target) > abs(r- target)) {
                    res = r;
                }
            } else if(root->left && target < root->val) {
                int l = closestValue(root->left, target);
                if(abs(res - target) > abs(l- target)) {
                    res = l;
                }
            }
            
            return res;
        }
    ```
2. [Minimum Subtree](https://www.lintcode.com/problem/minimum-subtree/description?_from=ladder&&fromId=1)
    ```c++
        int minSum = INT_MAX;
        TreeNode * minNode = nullptr;
        
        TreeNode * findSubtree(TreeNode * root) {
            if(root == nullptr || !root->left && !root->right) {
                return root;
            }
            findSub(root);
            return minNode;
        }
        int findSub(TreeNode * root) {
            if(root == nullptr) {
                return 0;
            }
            // post order traversal
            // operation on minNode
            int curSum = findSub(root->left) + root->val + findSub(root->right);
            }
            if(curSum < minSum) {
                minSum = curSum;
                minNode = root;
            }
            return curSum;
        }
    ```
3. [Binary Tree Path](https://www.lintcode.com/problem/binary-tree-paths/)
    ```c++
        /**
         - @param root: the root of the binary tree
         - @return: all root-to-leaf paths
         */
        vector<string> result;
        vector<string> binaryTreePaths(TreeNode * root) {
            // write your code here
            if(root == nullptr) {
                return {};
            }
            findPath(root, "");
            return result;
        }
        void findPath(TreeNode * root,string str) {
            if(!root->left && !root->right) {
                result.push_back(str + to_string(root->val));
            } 
            if(root->left){
                findPath(root->left, str + to_string(root->val) +  "->");
            }
            if(root->right) {
                findPath(root->right, str +  to_string(root->val) + "->");
            }
        }
    ```
4. [Flatten Binary Tree to Linked List](https://www.lintcode.com/problem/flatten-binary-tree-to-linked-list/description?_from=ladder&&fromId=1)
    ```c++
        void flatten(TreeNode * root) {
            if(root == nullptr || !root->left && !root->right) {
                return;
            }
            flatten(root->left);
            flatten(root->right);
            TreeNode * right = root->right;
            root->right = root->left;
            root->left = nullptr;
            TreeNode * tmp = root;
            while(tmp->right) {
                tmp = tmp->right;
            }
            tmp->right = right;
            // any traversal order could work (in pre post)
        }
    ```
5. [Binary Tree Preorder Traversal](https://www.lintcode.com/problem/binary-tree-preorder-traversal/description?_from=ladder&&fromId=1)
    ```c++
        vector<int> preorderTraversal(TreeNode * root) {
            if(root == nullptr) {
                return {};
            }
            vector<int> results;
            preOrder(root, results);
            return results;
        }
        
        void preOrder(TreeNode * node, vector<int> & results) {
            if(node == nullptr) {
                return;
            }
            results.push_back(node->val);
            preOrder(node->left, results);
            preOrder(node->right, results);
        }
    ```
6. [Max Depth of Binary Tree](https://www.lintcode.com/problem/maximum-depth-of-binary-tree/description?_from=ladder&&fromId=1)
    ```c++
        // divide & conquer
        int maxDepth(TreeNode * root) {
            if(root == nullptr) {
                return 0;
            }
            return max(maxDepth(root->left), maxDepth(root->right)) + 1;
        }
        // traverse
        int depth = 0;
        int maxDepth(TreeNode * root) {
            findMaxDepth(root, 0);
            return depth;
        }
        void findMaxDepth(TreeNode * root, int curDepth) {
            if(!root) {
                depth = max(depth, curDepth);
                return;
            }
            findMaxDepth(root->left, curDepth + 1);
            findMaxDepth(root->right, curDepth + 1);
        }

    ```
7. [Subtree with Maximum Average](https://www.lintcode.com/problem/subtree-with-maximum-average/description?_from=ladder&&fromId=1)
    ```c++
        TreeNode* maxAvgNode = nullptr;
        double maxAvg = INT_MIN;
        
        TreeNode * findSubtree2(TreeNode * root) {
            // recursion + post order
            if(root == nullptr) {
                return root;
            }
            findSubTree(root);
            return maxAvgNode;
        }
        // return a double array first being the sum and second being the # of elements
        vector<double> findSubTree(TreeNode * root) {
            if(!root) {
                return {0.0, 0.0};
            }
            vector<double> leftVals = findSubTree(root->left);
            vector<double> rightVals = findSubTree(root->right);
            double newNum = leftVals[1] + rightVals[1] + 1;
            double newSum = (leftVals[0] + rightVals[0] + root->val);
            double newAvg = newSum / newNum;
            if(newAvg > maxAvg) {
                maxAvgNode = root;
                maxAvg = newAvg;
            }
            return {newSum, newNum};
        }
    ```
8. [Balanced Binary Search Tree](https://www.lintcode.com/problem/balanced-binary-tree/description?_from=ladder&&fromId=1)
    ```c++
        /**
         - @param root: The root of binary tree.
         - @return: True if this Binary tree is Balanced, or false.
         */
        int NOT_BALANCED = -1;
        bool isBalanced(TreeNode * root) {
            return depthBalanced(root) != NOT_BALANCED;
        }
        
        int depthBalanced(TreeNode * root) {
            if(root == nullptr) {
                return 0;
            }
            int left = depthBalanced(root->left);
            int right = depthBalanced(root->right);
            if(left == NOT_BALANCED || right == NOT_BALANCED) {
                return NOT_BALANCED;
            }
            if(abs(left - right) > 1) {
                return NOT_BALANCED;
            }
            return 1 + max(left, right);
        }
    ```
9. [Lowest Common Ancestor of a Binary Tree](https://www.lintcode.com/problem/lowest-common-ancestor-of-a-binary-tree/description?_from=ladder&&fromId=1)
    ```c++
        TreeNode * lowestCommonAncestor(TreeNode * root, TreeNode * A, TreeNode * B) {
            if(root == nullptr || root == A || root == B) {
                return root;
            }
            TreeNode* leftNode = lowestCommonAncestor(root->left, A, B);
            TreeNode* rightNode = lowestCommonAncestor(root->right, A, B);
            if(leftNode && rightNode) {
                return root;
            }
            if(leftNode) {
                return leftNode;
            }
            if(rightNode) {
                return rightNode;
            }
            return nullptr;
        }
    ```
10. [Lowest Common Ancestor II](https://www.lintcode.com/problem/lowest-common-ancestor-ii/description?_from=ladder&&fromId=1)
    ```c++
        ParentTreeNode * lowestCommonAncestorII(ParentTreeNode * root, ParentTreeNode * A, ParentTreeNode * B) {
            if(A == B) {
                return A;
            }
            // get path
            vector<ParentTreeNode*> pathA = getPath2Root(A);
            vector<ParentTreeNode*> pathB = getPath2Root(B);
            
            // compare pathA and pathB
            ParentTreeNode * lowestNode;
            int i = pathA.size() - 1, j = pathB.size() - 1; 
            while(i >= 0 && j >= 0) {
                if(pathA[i] != pathB[j]) {
                    break;
                }
                lowestNode = pathA[i];
                --i;
                --j;
            }
            
            return lowestNode;
        }
        
        vector<ParentTreeNode*> getPath2Root(ParentTreeNode * node) {
            vector<ParentTreeNode*> path;
            while(node) {
                path.push_back(node);
                node = node->parent;
            }
            return path;
        }
    ```
11. [Lowest Common Ancestor III](https://www.lintcode.com/problem/lowest-common-ancestor-iii/description?_from=ladder&&fromId=1)
    ```c++
    class ResultType{
    public:
        TreeNode * node;
        bool aExist;
        bool bExist;
        ResultType(TreeNode* node, bool aExist, bool bExist): node(node), aExist(aExist), bExist(bExist){}
     };    
    class Solution {
    public:
        TreeNode * lowestCommonAncestor3(TreeNode * root, TreeNode * A, TreeNode * B) {
            ResultType result = helper(root, A, B);

            if(!result.aExist || !result.bExist) {
                return nullptr;
            }
            return result.node;
        }
        
        ResultType helper(TreeNode * cur, TreeNode* A, TreeNode* B) {
            if(!cur) {
                return ResultType(nullptr, false, false);
            }
            ResultType left = helper(cur->left, A, B);
            ResultType right  = helper(cur->right, A, B);
            bool aExist = left.aExist || right.aExist || cur == A;
            bool bExist = left.bExist || right.bExist || cur == B;
            if(cur == A || cur == B) {
                return ResultType(cur, aExist, bExist);
            }
            if(left.node && right.node) {
                return ResultType(cur, aExist, bExist);
            }
            if(left.node) {
                return ResultType(left.node, aExist, bExist);
            }
            if(right.node) {
                return ResultType(right.node, aExist, bExist);
            }
            return ResultType(nullptr, aExist, bExist);
        }
    }
    ```
12.[kth smallest element in a BST](https://www.lintcode.com/problem/kth-smallest-element-in-a-bst/?_from=ladder&&fromId=1)
    ```c++
        int kth = -1;
        int index = 0;
         // inorder traversal
        int kthSmallest(TreeNode * root, int k) {
            inorder(root, k);
            return kth;
        }
        void inorder(TreeNode * root, int k) {
            if(root == nullptr) {
                return;
            }
            inorder(root->left, k);
            
            if(++index == k) {
                kth = root->val;
                return;
            }
            
            inorder(root->right, k);
        }
    ```
13.[Validate Binary Search Tree](https://www.lintcode.com/problem/validate-binary-search-tree/description?_from=ladder&&fromId=1)
    ```c++
        TreeNode * prevNode = nullptr;
        
        bool isValidBST(TreeNode * root) {
            if(root == nullptr) {
                return true;
            }
            if(!isValidBST(root->left)){
                return false;
            }
            
            if(prevNode != nullptr && prevNode->val >= root->val){
                return false;
            }
            
            prevNode = root;

            if(!isValidBST(root->right)){
                return false;
            }
            
            return true;
        }
    ```
14. [BST Iterator](https://www.lintcode.com/problem/binary-search-tree-iterator/description?_from=ladder&&fromId=1)
    ```c++
    class BSTIterator {
    public:
        stack<TreeNode *> mystack;
        /*
        - @param root: The root of binary tree.
        */
        BSTIterator(TreeNode * root) {
            while(root) {
                mystack.push(root);
                root = root->left;
            }
        }

        /*
         - @return: True if there has next node, or false
         */
        bool hasNext() {
            return !mystack.empty();
        }

        /*
         - @return: return next node
         */
        TreeNode * next() {
            if(mystack.empty()) {
                return nullptr;
            }
            TreeNode * next = mystack.top(); mystack.pop();
            TreeNode * right = next->right;
            while(right) {
                mystack.push(right);
                right = right->left;
            }
            return next;
        }
    };
    ```
15. [Closest Binary Search Tree Value II](https://www.lintcode.com/problem/closest-binary-search-tree-value-ii/description?_from=ladder&&fromId=1)
    ```c++
        vector<int> result;
        int index = 0;
        vector<int> closestKValues(TreeNode * root, double target, int k) {
            helper(root, target, k);
            return result;
        }
        
        void helper(TreeNode * cur, double target, int k) {
            if(cur == nullptr) {
                return;
            }
            helper(cur->left, target, k);
            if(result.size() == k) {
                if(abs(cur->val - target) < abs(result[index] - target)) {
                    result[index++] = cur->val;
                    index = index % k;
                }
            } else {
                result.push_back(cur->val);
            }
            helper(cur->right, target, k);
        }
    ```

### Two Pointer
1. [Middle of Linked List](https://www.lintcode.com/problem/middle-of-linked-list/description?_from=ladder&&fromId=1)
    ```c++
        ListNode * middleNode(ListNode * head) {
            if(!head || !head->next) {
                return head;
            }
            ListNode * slow = head;
            ListNode * fast = head->next;
            while(fast && fast->next) {
                slow = slow->next;
                fast = fast->next->next;
            }
            return slow;
        }
    ```
2. [Two Sum](https://www.lintcode.com/problem/two-sum/description?_from=ladder&&fromId=1)
    ```c++
         vector<int> twoSum(vector<int> &numbers, int target) {
            // using hash map
            std::unordered_map<int, int> hash;

            for(int i = 0; i < numbers.size(); ++i) {
                if(hash.find(target - numbers[i]) != hash.end()) {
                    return {hash[target - numbers[i]], i};
                }
                hash[numbers[i]] = i;
            }
            return  {};
            // using pointers but return index after sorting 
            int ptrLeft = 0, ptrRight = nums.size() - 1;
            while(ptrLeft < ptrRight) {
                if(nums[ptrLeft] + nums[ptrRight] == target) {
                    return {ptrLeft, ptrRight};
                }
                if(nums[ptrLeft] + nums[ptrRight] < target) {
                    ++ptrleft;
                } else {
                    --ptrRight;
                }
            }
            return {};
        }
    ```
3. [Window Sum](https://www.lintcode.com/problem/window-sum/description?_from=ladder&&fromId=1)
    ```c++
        vector<int> winSum(vector<int> &nums, int k) {
            vector<int> results;
            if(k == 0 || k > nums.size() || nums.size() == 0) {
                return results;
            }
            int firstSum = 0;
            for(int i = 0; i < k; ++i) {
                firstSum += nums[i];
            }
            results.push_back(firstSum);
            for(int i = 0; i < nums.size() - k; ++i) {
                firstSum = firstSum - nums[i] + nums[i + k];
                results.push_back(firstSum);
            }
            return results;
        }
    ```
4. [Two Sum III - Data structure design](https://www.lintcode.com/problem/two-sum-iii-data-structure-design/description?_from=ladder&&fromId=1)
    ```c++
    class TwoSum {
    public:
        unordered_multiset<int> nums;
        // Add the number to an internal data structure.
        void add(int number) {
            nums.insert(number);
        }

        // Find if there exists any pair of numbers which sum is equal to the value.
        bool find(int value) {
            for (int i : nums) {
                int count = i == value - i ? 2 : 1;
                if (nums.count(value - i) >= count) {
                    return true;
                }
            }
            return false;
        }
    };
    ```
5. [Move Zeros](https://www.lintcode.com/problem/move-zeroes/description?_from=ladder&&fromId=1)
    ```c++
        void moveZeroes(vector<int> &nums) {
            int nonZeroPtr = 0, arrPtr = 0;
            while(arrPtr < nums.size()) {
                if(nums[arrPtr] != 0) {
                    nums[nonZeroPtr++] = nums[arrPtr];
                }
                ++arrPtr;
            }
            while(nonZeroPtr < nums.size()) {
                nums[nonZeroPtr++] = 0;
            }
        }
    ```
6. [Remove Duplicates Numbers in Array](https://www.lintcode.com/problem/remove-duplicate-numbers-in-array/description?_from=ladder&&fromId=1)
    ```c++
        int deduplication(vector<int> &nums) {
            if(nums.size() == 0) {
                return 0;
            }
            // using hash map
            unordered_set<int> exist;
            for(int value: nums) {
                exist.insert(value);
            }
            int i = 0;
            for(unordered_set<int>::iterator it = exist.begin(); it != exist.end();++it) {
                nums[i++] = *it;
            }
            return i;
            
            // two pointer
            sort(nums.begin(), nums.end());
            int arrPtr = 1, notDup = 0;
            while(arrPtr < nums.size()) {
                if(nums[arrPtr] != nums[notDup]) {
                    nums[++notDup] = nums[arrPtr];
                }
                ++arrPtr;
            }
            return notDup + 1;
        }
    ```
7. [Sort Integer II](https://www.lintcode.com/problem/sort-integers-ii/description?_from=ladder&&fromId=1)
    ```c++
    public:
        void sortIntegers2(vector<int> &A) {
            // quickSort & mergeSort
            if(A.size() <= 1) {
                return;
            }
            int temp[A.size()];
            mergeSort(A, 0, A.size() - 1, temp);
            quickSort(A, 0, A.size() - 1);        
        }
        
    private:
        void mergeSort(vector<int> & nums, int start, int end, int temp []) {
            if(start >= end) {
                return;
            }
            int mid = start + (end - start) / 2; 
            mergeSort(nums, start, mid, temp);
            mergeSort(nums, mid + 1, end, temp);
            merge(nums, start, mid, end, temp);
        } 
        void merge(vector<int> & nums, int start, int mid, int end, int temp[]) {
            int leftIndex = start;
            int rightIndex = mid + 1;
            int tempIndex = start;
            while(leftIndex <= mid && rightIndex <= end) {
                if(nums[leftIndex] <= nums[rightIndex]) {
                    temp[tempIndex++] = nums[leftIndex++];
                } else {
                    temp[tempIndex++] = nums[rightIndex++];
                }
            }
            while(leftIndex <= mid) {
                temp[tempIndex++] = nums[leftIndex++];
            }
            while(rightIndex <= end) {
                temp[tempIndex++] = nums[rightIndex++];
            }
            for(int i = start; i <= end; ++i) {
                nums[i] = temp[i];
            }
        }
        
        void quickSort(vector<int> & nums, int start, int end) {
            if(start >= end) {
                return;
            }
            int left = start, right = end, mid = (start + end) / 2; // mid as the pivot point, random index is also good
            int pivot = nums[mid]; // pick the pivot point but not the index
            // put <= here to make sure left right pass each other to avoid dead loop like in [1 2] would end in dead loop quickSort([1 2], 0, 1)
            while(left <= right) { 
                // put the element <= pivot on left and >= on right 
                // not < or > because we want it evenly distributed, for instance if we have 1 1 1 1 1 2 then we want to divide it into 'half' but not 1 1 1 1 1 | 2 or 1 | 1 1 1 1 2 
                while(left <= right && nums[left] < pivot) { // < 
                    ++left;
                }
                while(left <= right && nums[right] > pivot) {
                    --right;
                }
                if(left <= right) {
                    std::swap(nums[left++], nums[right--]);
                }
            }
            quickSort(nums, start, right);
            quickSort(nums, left, end);   
        }
    ```
    + difference between **quickSort** and **mergeSort**
        + both use divide & conquer strategy
            + whole to part vs. part to whole
        + [why is quicksort better](https://stackoverflow.com/questions/70402/why-is-quicksort-better-than-mergesort)
        + worst case O(n^2) v.s. O(nlgn)
        + in-place  vs. auxilary space 
        + quicksort: good cache locality
        + mergesort: *stable* sort and can be easily adapted to operate on *linked lists*
8. [Partition Array](https://www.lintcode.com/problem/partition-array/description?_from=ladder&&fromId=1) 
    ```c++
        int partitionArray(vector<int> &nums, int k) {
            int start = 0, end = nums.size() - 1;
            // pivot is k using quicksort
            while(start <= end) {
                while(start <= end && nums[start] < k) {
                    ++start;
                }
                while(start <= end && nums[end] >= k) {
                    --end;
                }
                if(start <= end) {
                    swap(nums[start++], nums[end--]);
                }
            }
            return start;
        }
    ```
9. [Kth Largest Element](https://www.lintcode.com/problem/kth-largest-element/description?_from=ladder&&fromId=1)
    ```c++
        int kthLargestElement(int k, vector<int> &nums) {
            if(nums.size() == 0) {
                return -1;
            }
            return quickSelect(nums, 0, nums.size() - 1, k);
        }
        
        int quickSelect(vector<int> & nums, int start, int end ,int k) {
            if(start == end) {
                return nums[start];
            }
            int left = start, right = end;
            int pivot = nums[(start + end) / 2];
            while(left <= right) {
                while(left <= right && nums[left] > pivot) { // if < pivot then sort in ascending order and we can find kth smallest
                    ++left;
                }
                while(left <= right && nums[right] < pivot) {
                    --right;
                }
                if(left <= right) {
                    std::swap(nums[left++], nums[right--]);
                }
            }
            if(start + k - 1 <= right) {
                return quickSelect(nums, start, right, k);
            }
            if(start + k - 1 >= left) {
                return quickSelect(nums, left, end, k - (left - start));
            }
            return nums[left - 1]; // same as nums[right + 1]
        }
    ```
10. [Two Sum - Difference equals to target](https://www.lintcode.com/problem/two-sum-difference-equals-to-target/?_from=ladder&&fromId=1)
    ```c++
        vector<int> twoSum7(vector<int> nums, int target) {
            // two pointer in the same direction
            sort(nums.begin(), nums.end());
            int first = 0, second = 0;
            target= abs(target);
            while(second < nums.size()) {
                while(second<nums.size() && nums[second] - nums[first] < target) {
                    ++second;
                }
                if(nums[second] - nums[first] == target) {
                    return {nums[first], nums[second]};
                }
                ++first;
            }
            return {};

            // using hash map
            std::unordered_map<int, int> hash;
            // target = abs(target);
            for(int i = 0; i < nums.size(); ++i) {
                if(hash.find(target + nums[i]) != hash.end()) {
                    return {hash[target + nums[i]] + 1, i + 1};
                }
                if(hash.find(nums[i] - target) != hash.end()) {
                    return {hash[nums[i] - target] + 1, i + 1};
                }
                hash[nums[i]] = i;
            }
        }
    ```
11. [3Sum](https://www.lintcode.com/problem/3sum/description?_from=ladder&&fromId=1)
    ```c++
        vector<vector<int>> threeSum(vector<int> &nums) {
            vector<vector<int>> result;
            sort(nums.begin(), nums.end());
            int target = 0;
            for(int i = 0; i < nums.size(); ++i) {
                if(i > 0 && nums[i] == nums[i - 1]){
                    continue;
                }
                int start = i + 1, end = nums.size() - 1;
                int targetTwo = target - nums[i];
                // two sum
                while(start < end) {
                    if(start > i + 1 && nums[start] == nums[start - 1]) {
                        ++start;
                        continue;
                    }
                    if(nums[start] + nums[end] == targetTwo) {
                        result.push_back({nums[i], nums[start++], nums[end]}); // end-- doesn't work bc duplicate exists at the end
                    }else if(nums[start] + nums[end] < targetTwo) {
                        ++start;
                    } else {
                        --end;
                    }
                }
            }
            return result;
        }
    ```
12. [Sort Colors](https://www.lintcode.com/problem/sort-colors/description?_from=ladder&&fromId=1)
    ```c++
        void sortColors(vector<int> &a) {
            // three pointers
            if(a.size() <= 1) {
                return;
            }
            int leftPtr = 0, rightPtr = a.size() - 1;
            int index = 0;
            while(index <= rightPtr) {
                if(a[index] == 0) {
                    std::swap(a[index++], a[leftPtr++]);
                } else if(a[index] == 1) {
                    ++index;
                } else {
                    std::swap(a[index], a[rightPtr--]);
                }
            }
        }
    ```
13. [Sort Colors II](https://www.lintcode.com/problem/sort-colors-ii/description?_from=ladder&&fromId=1)
14. [Intersection of Two Linked Lists](https://www.lintcode.com/problem/intersection-of-two-linked-lists/description?_from=ladder&&fromId=1)
    ```c++
        ListNode * getIntersectionNode(ListNode * headA, ListNode * headB) {
            if(headA == nullptr || headB == nullptr) {
                return nullptr;
            }
            ListNode * linked = headA;
            while(linked -> next != nullptr) {
                linked = linked -> next;   
            }
            linked -> next = headB;
            ListNode * slow = headA;
            ListNode * fast = headA -> next;
            while(slow != fast) {
                if(fast == nullptr || fast -> next == nullptr) {
                    return nullptr;
                }
                slow = slow -> next;
                fast = fast -> next -> next;
            }
            while(headA != slow -> next) {
                headA = headA -> next;
                slow = slow -> next;
            }
            return headA;
        }
    ```
15. [Linked List Cycle](https://www.lintcode.com/problem/linked-list-cycle/description?_from=ladder&&fromId=1)
    ```c++
        bool hasCycle(ListNode * head) {
            if(head == nullptr || head->next == nullptr) {
                return false;
            }
            ListNode* slow = head;
            ListNode* fast = head->next;
            while(fast != nullptr && fast ->next != nullptr) {
                slow = slow -> next;
                fast = fast -> next ->next;
                if(slow == fast) {
                    return true;
                }
            }
            return false;
        }
    ```
16. [Linked List Cycle II](https://www.lintcode.com/problem/linked-list-cycle-ii/description?_from=ladder&&fromId=1)
    ```c++
        ListNode * detectCycle(ListNode * head) {
            if(head == nullptr || head -> next == nullptr) {
                return nullptr;
            }
            ListNode * slow = head;
            ListNode * fast = head -> next;
            while(slow != fast) {
                if(fast == nullptr || fast -> next == nullptr) {
                    return nullptr;
                }
                slow = slow -> next;
                fast = fast -> next -> next;
            }
            // find cycle
            slow = head;
            while(slow != fast -> next) {
                slow = slow -> next;
                fast = fast -> next;
            }
            return slow;
        }
    ```

---

### Implicit Graph DFS
1. [Subsets](https://www.lintcode.com/problem/subsets/description?_from=ladder&&fromId=1)
```c++
    // combination -> tree problem
    // time complexity O(2^n)

    // use traditional dfs and 0 1 approach (like in bfs) 
    vector<vector<int>> subsets(vector<int> &nums) {
        vector<vector<int>> result;
        vector<int> subset;
        sort(nums.begin(), nums.end());
        // helper(nums, 0, result, subset);
        dfs(nums, 0, result, subset);
        return result;
    }
    // traditional dfs search
    void helper(vector<int> &nums, 
                int index, 
                vector<vector<int>>& result, 
                vector<int>& subset) {
        result.push_back(subset);

        for(int i = index; i < nums.size(); ++i) {
            subset.push_back(nums[i]);
            helper(nums, i + 1, result, subset); // important here it's i + 1 not index + 1
            subset.pop_back();
        }
    }
    // use 1 0 binary tree kind of approach
    void dfs(vector<int> & nums, 
            int index, 
            vector<vector<int>> & result, 
            vector<int> & subset) {
        if(index == nums.size()) {
            result.push_back(subset);
            return;
        }
        subset.push_back(nums[index]);
        dfs(nums, index + 1, result, subset);
        subset.pop_back();
        dfs(nums, index + 1, result, subset);
    }
```
2. [Subsets II](https://www.lintcode.com/problem/subsets-ii/description?_from=ladder&&fromId=1)
```c++
    vector<vector<int>> subsetsWithDup(vector<int> &nums) {
        vector<vector<int>> result;
        vector<int> subset;
        sort(nums.begin(), nums.end());
        helper(nums, 0, result, subset);
        return result;
    }
    void helper(vector<int> & nums,
                int index,
                vector<vector<int>>& result,
                vector<int>& subset) {
        result.push_back(subset);
        // remove duplicate
        for(int i = index; i < nums.size(); ++i) {
            // i - 1 not in the set, then continue
            // if previous one is in the set, then i + 1 - 1 is i so i is start index
            // then if not in then i != start index
            // previous i is index(previous i + 1) - 1 so start index is previous i
            // or i > startIndex is i >= startIndex + 1 then there's a number between startIndex - 1 and startIndex + 1
            // so i - 1 is not in the subset
            if(i != 0 && nums[i] == nums[i - 1] && i > index) {
                continue;
            }
            
            subset.push_back(nums[i]);
            helper(nums, i + 1, result, subset);
            subset.pop_back();
        }
    }
```
3. [Permutations](https://www.lintcode.com/problem/permutations/description?_from=ladder&&fromId=1)
```c++
    vector<vector<int>> permute(vector<int> &nums) {
        vector<vector<int>> permutations;
        vector<int> subset;
        vector<bool> is_visited(nums.size(), false);
        dfs(permutations, subset, nums, is_visited);
        return permutations;
    }
    // if used or not
    void dfs(vector<vector<int>> & permutations, 
             vector<int> & subset, 
             vector<int> & nums, 
             vector<bool> & is_visited){
        if(nums.size() == subset.size()) {
            permutations.push_back(subset);
            return;
        }
        for(int i = 0; i < nums.size(); ++i) {
            if(is_visited[i]) {
                continue;
            }
            subset.push_back(nums[i]);
            is_visited[i] = true;
            dfs(permutations, subset, nums, is_visited); // is_visited
            subset.pop_back();
            is_visited[i] = false;
        }
    }
```
4. [Permutations II](https://www.lintcode.com/problem/permutations-ii/description?_from=ladder&&fromId=1)
```c++
    vector<vector<int>> permuteUnique(vector<int> &nums) {
        vector<vector<int>> permutations;
        vector<int> subset;
        sort(nums.begin(), nums.end());
        vector<bool> is_visited(nums.size(), false);
        dfs(permutations, subset, nums, is_visited);
        return permutations;
    }
    
    // time complexity O(s * n^2)
    
    // if used or not 
    // remove duplicates select representation
    void dfs(vector<vector<int>> & permutations, 
             vector<int> & subset, 
             vector<int> & nums, 
             vector<bool> & is_visited){
        if(nums.size() == subset.size()) {
            permutations.push_back(subset);
            return;
        }
        for(int i = 0; i < nums.size(); ++i) {
            if(is_visited[i]) {
                continue;
            }
            // the previous is not used in the permutation then continue
            if(i != 0 && nums[i] == nums[i - 1] && !is_visited[i - 1]) {
                continue;
            }
            
            subset.push_back(nums[i]);
            is_visited[i] = true;
            dfs(permutations, subset, nums, is_visited); // is_visited
            subset.pop_back();
            is_visited[i] = false;
        }
    }
```
5. [Split String](https://www.lintcode.com/problem/split-string/description?_from=ladder&&fromId=1)
```c++
    vector<vector<string>> splitString(string& s) {
        vector<vector<string>> result;
        vector<string> subset;
        helper(result, subset, 0 , s);
        return result;
    }
    // use 0 1 appraoch
    void helper(vector<vector<string>>& result, vector<string>& subset, int index, string & s) {
        if(index == s.length()) {
            result.push_back(subset);
            return; // remember to return after this
        }
        // option 1
        subset.push_back(s.substr(index, 1));
        helper(result, subset, index + 1 , s);
        subset.pop_back();
        // subset.erase(remove(subset.begin(), subset.end(), s.substr(index, 1)), subset.end());

        if(index >= s.length() - 1) {
            return;
        }
        // option 2
        subset.push_back(s.substr(index, 2));
        helper(result, subset, index + 2 , s);

        subset.pop_back(); // have to pop back here or it would affect
        // subset.erase(remove(subset.begin(), subset.end(), s.substr(index, 2)), subset.end());
    }
```
6. [Letter Combinations of a Phone Number](https://www.lintcode.com/problem/letter-combinations-of-a-phone-number/description?_from=ladder&&fromId=1)
```c++
    vector<string> letterCombinations(string &digits) {
        if(digits.length() == 0) {
            return {};
        }
        // hash map
        unordered_map<char, vector<char>> letter_map;
        letter_map['2'] = {'a', 'b', 'c'};
        letter_map['3'] = {'d', 'e', 'f'};
        letter_map['4'] = {'g', 'h', 'i'};
        letter_map['5'] = {'j', 'k', 'l'};
        letter_map['6'] = {'m', 'n', 'o'};
        letter_map['7'] = {'p', 'q', 'r', 's'};
        letter_map['8'] = {'t', 'u', 'v'};
        letter_map['9'] = {'w', 'x', 'y', 'z'};
        
        // dfs
        vector<string> result;
        helper(result, "", 0, letter_map, digits);
        return result;
    }
    
    void helper(vector<string> & result, string cur_str, int index, unordered_map<char, vector<char>> & letter_map, string & digits) {
        if(index == digits.length()) {
            result.push_back(cur_str);
        }
        vector<char> cur_char =  letter_map[digits[index]];
        for(char ch: cur_char) {
            helper(result, cur_str + ch, index + 1, letter_map, digits);
        } // must remember that cur_str + ch at each level but not accumulation   
    }
```
7. [Combination Sum II](https://www.lintcode.com/problem/combination-sum-ii/description?_from=ladder&&fromId=1)
```c++
    vector<vector<int>> combinationSum2(vector<int> &num, int target) {
        vector<vector<int>>  result;
        vector<int> subset;
        sort(num.begin(), num.end());
        
        dfs(result, subset, num, 0, 0, target);
        return result;
    }
    
    void dfs(vector<vector<int>>& result, vector<int>& subset,vector<int> &num, int cur_sum, int index, int target){
        if(cur_sum == target) {
            result.push_back(subset);
            return;
        }
        // optimization 
        if(cur_sum > target) {
            return;
        }
        for(int i = index; i < num.size(); ++i) {
            if(i != 0 && num[i - 1] == num[i] && i > index) {
                continue;
            }
            // important here
            if (target < num[i]) {
                break;
            }
            
            subset.push_back(num[i]);
            dfs(result, subset, num, cur_sum + num[i], i + 1, target);
            subset.pop_back();
        }
    }
```
8. [Combination Sum](https://www.lintcode.com/problem/combination-sum/description?_from=ladder&&fromId=1)
```c++
    vector<vector<int>> combinationSum(vector<int> &candidates, int target) {
        vector<vector<int>> result;
        vector<int> subset;
        sort(candidates.begin(), candidates.end());
        dfs(candidates, result, subset, 0, 0, target);
        return result;
    }
    
    void dfs(vector<int> &candidates, vector<vector<int>> & result, vector<int> & subset, int cur_sum, int index, int target){
        if(cur_sum == target) {
            result.push_back(subset);
        }
        if(cur_sum > target) {
            return;
        }
        for(int i = index; i < candidates.size(); ++i) {
            if(i != 0 && candidates[i] == candidates[i - 1] ) { // no need to do i > index bc it would be always in that case since we are iterating on the same i then we could only access thru 
                continue;
            }
            if(candidates[i] > target) {
                return;
            }
            // option 1
            // subset.push_back(candidates[i]);
            // dfs(candidates, result, subset, cur_sum + candidates[i], i + 1, target);
            // subset.pop_back();
            
            // option 2
            subset.push_back(candidates[i]);
            dfs(candidates, result, subset, cur_sum + candidates[i], i, target);
            subset.pop_back();
        }
    }
```
9. [N-Queens](https://www.lintcode.com/problem/n-queens/description?_from=ladder&&fromId=1)
```c++
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<bool>> board(n, vector<bool>(n, false));
        // initialize important here
        vector<vector<string>> result;
        dfs(n, board, 0, result); // 0 indicates cur_col
        return result;
    }
    
    void dfs(int n, vector<vector<bool>> & board, int cur_col, vector<vector<string>> & result) {
        if(cur_col >= n) {
            result.push_back(drawBoard(n, board));
            return;
        }
        for(int cur_row = 0; cur_row < n; ++cur_row) {
            if(!underAttack(n, board, cur_row, cur_col)) {
                std::cout<<cur_col;
                std::cout<<cur_row;
                board[cur_row][cur_col] = true;
                dfs(n, board, cur_col + 1, result);
                board[cur_row][cur_col] = false;
            }
        }
    }
    
    bool underAttack(int n, vector<vector<bool>> & board, int x, int y) {
        // check the same row 
        for(int cur_col = y - 1; cur_col >= 0; --cur_col) {
            if(board[x][cur_col]) {
                return true;
            }
        }
        // check upper diagonal
        int cur_row = x - 1, cur_col = y - 1;
        while(cur_row >= 0 && cur_col >= 0) {
            if(board[cur_row][cur_col]) {
                return true;
            }
            --cur_row;
            --cur_col;
        }
        // check lower diagonal
        cur_row = x + 1;
        cur_col = y - 1;
        while(cur_row < n && cur_col >= 0) {
            if(board[cur_row][cur_col]) {
                return true;
            }
            ++cur_row;
            --cur_col;
        }
        return false;
    }
    
    vector<string> drawBoard(int n, vector<vector<bool>> & board) {
        vector<string> subset;
        for(int i = 0; i < n; ++i) {
            string str = "";
            for(int j = 0; j < n; ++j) {
                if(board[i][j]){
                    str += "Q";
                } else {
                    str += ".";
                }
            }
            subset.push_back(str);
        }
        return subset;
    }
```
10. [Word Pattern II](https://www.lintcode.com/problem/word-pattern-ii/description?_from=ladder&&fromId=1)
```c++
class Solution {
public:
    /**
     * @param pattern: a string,denote pattern string
     * @param str: a string, denote matching string
     * @return: a boolean
     */
    bool wordPatternMatch(string &pattern, string &str) {
        return dfs(pattern, 0, str, 0);
    }
    
    bool dfs(string &pattern, int index_pattern, string &str, int index_str) {
        if(pattern.length() == index_pattern && str.length() == index_str) {
            return true;
        }
        if(pattern.length() == index_pattern || str.length() == index_str) {
            return false;
        }
        char cur_pattern = pattern[index_pattern];
        int remain_len = str.length() - index_str;
        // a existing pattern
        if(dict.count(cur_pattern)) {
            string expected = dict[cur_pattern];
            if(expected.length() > remain_len || str.compare(index_str, expected.length(), expected)) {
                return false;
            }
            return dfs(pattern, index_pattern + 1, str, index_str + expected.length());
        }
        // a new pattern
        for(int len = 1; len <= remain_len; ++len) {
            string word = str.substr(index_str, len);
            if(used.count(word)) {
                continue;
            }
            dict[cur_pattern] = word;
            used.insert(word);
            if(dfs(pattern, index_pattern + 1, str, index_str + word.length())) {
                return true;
            }
            dict.erase(cur_pattern);
            used.erase(word);
        }
        return false;
    }
    
// the two variable is important here
private:
    unordered_map<char, string> dict;
    unordered_set<string> used;
};
```
11. [Word Ladder II](https://www.lintcode.com/problem/word-ladder-ii/description?_from=ladder&&fromId=1)
```c++
// http://zxi.mytechroad.com/blog/searching/leetcode-126-word-ladder-ii/
class Solution {
public:
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        
        unordered_set<string> dict(wordList.begin(), wordList.end());        
        if (!dict.count(endWord)) return {};
        dict.erase(beginWord);
        dict.erase(endWord);
        
        unordered_map<string, int> steps{{beginWord, 1}};
        unordered_map<string, vector<string>> parents;
        queue<string> q;
        q.push(beginWord);
        
        vector<vector<string>> ans;
        
        const int l = beginWord.length();
        int step = 0;        
        bool found = false;
        
        while (!q.empty() && !found) {
            ++step;            
            for (int size = q.size(); size > 0; size--) {
                const string p = q.front(); q.pop();
                string w = p;
                for (int i = 0; i < l; i++) {
                    const char ch = w[i];
                    for (int j = 'a'; j <= 'z'; j++) {
                        if (j == ch) continue;
                        w[i] = j;
                        if (w == endWord) {
                            parents[w].push_back(p);
                            found = true;
                        } else {
                            // Not a new word, but another transform
                            // with the same number of steps
                            if (steps.count(w) && step < steps.at(w))
                                parents[w].push_back(p);
                        }
                        
                        if (!dict.count(w)) continue;
                        dict.erase(w);
                        q.push(w);
                        steps[w] = steps.at(p) + 1;
                        parents[w].push_back(p);
                    }
                    w[i] = ch;
                }
            }
        }
        
        if (found) {
            vector<string> curr{endWord};
            getPaths(endWord, beginWord, parents, curr, ans);
        }
    
        return ans;
    }
private:
    void getPaths(const string& word, 
                  const string& beginWord, 
                  const unordered_map<string, vector<string>>& parents,
                  vector<string>& curr,
                  vector<vector<string>>& ans) {        
        
        if (word == beginWord) {
            ans.push_back(vector<string>(curr.rbegin(), curr.rend()));
            return;
        }
        
        for (const string& p : parents.at(word)) {
            curr.push_back(p);
            getPaths(p, beginWord, parents, curr, ans);
            curr.pop_back();
        }        
    }
};
```

### Hash & Heap
1. [Implement Stack by Two Queues](https://www.lintcode.com/problem/implement-stack-by-two-queues/description?_from=ladder&&fromId=1)
```c++
// method 1: use push
class Stack {
public:
    /*
     * @param x: An integer
     * @return: nothing
     */
    queue<int> queue1;
    queue<int> queue2;
    
    void push(int x) {
        if(queue1.empty()) {
            queue1.push(x);
        } else {
            queue2.push(x);
            while(!queue1.empty()) {
                int val = queue1.front();
                queue1.pop();
                queue2.push(val);
            }
            swap(queue1, queue2);
        }
    }

    /*
     * @return: nothing
     */
    void pop() {
        // check size
        queue1.pop();
    }

    /*
     * @return: An integer
     */
    int top() {
        // check size
        return queue1.front();
    }

    /*
     * @return: True if the stack is empty
     */
    bool isEmpty() {
        return queue1.size() == 0;
    }
};

// method 2: use pop 
class Stack {
public:
    /*
     * @param x: An integer
     * @return: nothing
     */
    queue<int> queue1;
    queue<int> queue2;
    
    void push(int x) {
        queue1.push(x);
    }

    /*
     * @return: nothing
     */
    void pop() {
        // check size
        while(queue1.size() > 1) {
            int val = queue1.front();
            queue1.pop();
            queue2.push(val);
        }
        queue1.pop();
        swap(queue1, queue2);
    }

    /*
     * @return: An integer
     */
    int top() {
        // check size
        while(queue1.size() > 1) {
            int val = queue1.front();
            queue1.pop();
            queue2.push(val);
        }
        int top =  queue1.front();
        queue2.push(top);
        queue1.pop();
        swap(queue1, queue2);
        return top;
    }

    /*
     * @return: True if the stack is empty
     */
    bool isEmpty() {
        return queue1.size() == 0;
    }
};
```
2. [Implement Two Queues by Two Stacks](https://www.lintcode.com/problem/implement-queue-by-two-stacks/description?_from=ladder&&fromId=1)
```c++
public class MyQueue {
private:
    stack<int> stack1;
    stact<int> stack2;
public:
    public MyQueue() {}

    /*
     * @param element: An integer
     * @return: nothing
     */
    public void push(int element) {
        stack1.push(x);
    }

    /*
     * @return: An integer
     */
    public int pop() {
        if(stack2.empty()) {
            while(!stack1.empty()) {
                stack2.push(stact1.top());
                stact1.pop();
            }
        }
        int val = stack2.top();
        stack2.pop();
        return val;
    }

    /*
     * @return: An integer
     */
    public int top() {
        if(stack2.empty()) {
            while(!stack1.empty()) {
                stack2.push(stact1.top());
                stact1.pop();
            }
        }
        return stack2.top();
    }
}
```
3. [Moving Average from Data stream](https://www.lintcode.com/problem/moving-average-from-data-stream/description?_from=ladder&&fromId=1)
```c++
class MovingAverage {
public:
  vector<double> sum;
  int id, size;
   
  MovingAverage(int size) :sum(size + 1, 0) {
    id = 0;
    this->size = size;  
  }

  double next(int val) {
    id++;
    sum[id] = sum[id - 1] + val;
    if (id - size >= 0) {
        return (sum[id] - sum[id - size]) / size;
    } else {
        return sum[id]/ id;
    }
  }
};
```
4. [First Unique Char in a Str](https://www.lintcode.com/problem/first-unique-character-in-a-string/description?_from=ladder&&fromId=1)
```c++
    char firstUniqChar(string &str) {
        int vis[26] = {0};
        for(int i = 0; i < str.length(); ++i) {
            ++vis[str[i] - 'a'];
        }
        for(int i = 0; i<str.length() ;++i) {
            if(vis[str[i] - 'a'] == 1) {
                return str[i];
            }
        }
        return ' ';
    }
```
5. [Hash Function](https://www.lintcode.com/problem/hash-function/description?_from=ladder&&fromId=1)
```c++
    int hashCode(string &key, int HASH_SIZE) {
        long answer = 0;
        for(int i = 0; i < key.length(); ++i) {
            answer = (answer * 33 + key[i]) % HASH_SIZE;
        }
        // 10 * 33^2 + 20 * 33^1 + 30 * 33^0
        // = ((10 * 33 + 20) * 33 + 30 
        return (int)answer;
    }
```
6. [Rehashing](https://www.lintcode.com/problem/rehashing/description?_from=ladder&&fromId=1)
```c++
    vector<ListNode*> rehashing(vector<ListNode*> hashTable) {
        int newSize =  hashTable.size() * 2;
        vector<ListNode*> newHashTable(newSize, nullptr);
        // new hash function, key here is to use newSize for new Hashing  function
        for(int i = 0; i < hashTable.size(); ++i) {
            ListNode * ptr = hashTable[i];
            while(ptr != nullptr) {
                // add node to the new hash table here
                int pos = (ptr->val + newSize) % newSize;
                if(newHashTable[pos] == nullptr) {
                    newHashTable[pos] = new ListNode(ptr->val);
                } else {
                    ListNode * newPtr = newHashTable[pos];
                    while(newPtr->next != nullptr) {
                        newPtr = newPtr->next;
                    }
                    newPtr->next =  new ListNode(ptr->val);
                }
                ptr = ptr->next;
            }
        }
        return newHashTable;
    }
```
7. [Heapify](https://www.lintcode.com/problem/heapify/description?_from=ladder&&fromId=1)
```c++
    // shift up and shift down
    // shift up O(nlgn)
    // shift down O(n)
    // min-heap
    void heapify(vector<int> &A) {
        if(A.size() <= 1) {
            return;
        }
        for(int i = (A.size() - 2)/ 2; i>= 0; --i) {
            shiftdown(A, i);
        }
    }
    void shiftdown(vector<int> & A, int i) {
        while(i * 2 + 1 < A.size()) {
            int son = i * 2 + 1; // take the left son
            if(son + 1 < A.size() && A[son + 1] < A[son]) { // compute the min of the children
                son = son + 1;
            }
            // break if already satisfy smaller than the children
            if(A[i] <= A[son]) {
                break;
            }
            swap(A[son], A[i]);
            i = son;
        }
    }
```
8. [Merge K Sorted Array](https://www.lintcode.com/problem/merge-k-sorted-arrays/description?_from=ladder&&fromId=1)
```c++
class Node{
public:
    int row, col, val;
    Node(int row, int col, int val): row(row), col(col), val(val){}
    bool operator < (const Node & b) const {
        return val > b.val;
    }
};

// O(Nlgk)
class Solution {
public:
    /**
     * @param arrays: k sorted integer arrays
     * @return: a sorted array
     */
    vector<int> mergekSortedArrays(vector<vector<int>> &arrays) {
        vector<int> result;
        if(arrays.empty()) {
            return result;
        }
        // 初始将所有数组的首个元素入堆, 并记录入堆的元素是属于哪个数组的
        priority_queue<Node> min_heap;
        for(int i = 0; i < arrays.size(); ++i) {
            if(arrays[i].empty()) {
                continue;
            }
            min_heap.push(Node(i, 0, arrays[i][0]));
        }
        
        // 每次取出堆顶元素, 并放入该元素所在数组的下一个元素
        while(!min_heap.empty()) {
            Node cur = min_heap.top();
            result.push_back(cur.val);
            min_heap.pop();
            if(cur.col + 1 < arrays[cur.row].size()) {
                min_heap.push(Node(cur.row, cur.col + 1, arrays[cur.row][cur.col + 1]));
            }
        }
        
        return result;
    }
};
```
    - priority queue in c++ is **max heap** by default
9. [Insert Delete GetRandom O(1)](https://www.lintcode.com/problem/insert-delete-getrandom-o1/description?_from=ladder&&fromId=1)
```c++
class RandomizedSet {
public:
    RandomizedSet() {
        // do intialization if necessary
    }

    /*
     * @param val: a value to the set
     * @return: true if the set did not already contain the specified element or false
     */
    bool insert(int val) {
        if (nums_map.count(val)) {
            return false;
        }
        nums_map[val] = nums_list.size();
        nums_list.push_back(val);
        return true;
    }

    /*
     * @param val: a value from the set
     * @return: true if the set contained the specified element or false
     */
    bool remove(int val) {
        if(!nums_map.count(val)) {
            return false;
        }
        // swap the last element in nums_list and val
        int index = nums_map[val], last = nums_list.size() - 1;
        nums_map[last] = index; // set index
        std::swap(nums_list[index], nums_list[last]);
        // remove the last one from nums_list and nums_map;
        nums_list.pop_back();
        nums_map.erase(val);
        return true;
    }

    /*
     * @return: Get a random element from the set
     */
    int getRandom() {
        return nums_list[rand() % nums_list.size()];
    }
    
private:
    unordered_map<int, int> nums_map;
    vector<int> nums_list;
};
```
10. [k closest points](https://www.lintcode.com/problem/k-closest-points/description?_from=ladder&&fromId=1)
```c++
Point global_origin;
long getDistance(Point a, Point b) {
    return (a.x - b.x) * (a.x - b.x) + (a.y - b.y) * (a.y - b.y);
}

struct compare {  
    bool operator()(const Point &a, const Point &b) const {  
        int diff = getDistance(a, global_origin) - getDistance(b, global_origin);
        if(diff == 0) {
            diff = a.x - b.x;
        }
        if(diff == 0) {
            diff = a.y - b.y;
        }
        return diff < 0;
    }  
};

class Solution {
public:
    /**
     * @param points: a list of points
     * @param origin: a point
     * @param k: An integer
     * @return: the k closest points
     */
    vector<Point> kClosest(vector<Point> &points, Point &origin, int k) {
        global_origin = Point(origin.x, origin.y);
        priority_queue<Point, vector<Point>, compare> pq; // max distance heap
        for(Point pt: points) {
            pq.push(pt);
            if(pq.size() > k) {
                pq.pop();
            }
        }
        vector<Point> res(k);
        for(int i = k - 1; i >= 0; --i) {
            res[i] = pq.top(); pq.pop();
        }
        return res;
    }
};
```
11. [Top k Largest Numbers](https://www.lintcode.com/problem/top-k-largest-numbers/description?_from=ladder&&fromId=1)
```c++
    vector<int> topk(vector<int> &nums, int k) {
        priority_queue<int, vector<int>, greater<int>> pq;
        for(int num: nums) {
            pq.push(num);
            if(pq.size() > k) {
                pq.pop();
            }
        }
        vector<int> res(k);
        for(int i = k - 1; i >= 0; --i) {
            res[i] = pq.top();
            pq.pop();
        }
        return res;
    }
```
12. [Ugly Number 2](https://www.lintcode.com/problem/ugly-number-ii/description?_from=ladder&&fromId=1)
```c++
    int nthUglyNumber(int n) {
        // O(nlogn) HashMap + Heap
        // use Hashmap to check if it's in the list and heap to access the next cur largest
        priority_queue<long, vector<long>, greater<long>> pq;
        unordered_set<long> visited;
        vector<long> primes = {2, 3, 5};
        for(int i = 0; i < 3; ++i) {
            pq.push(primes[i]);
            visited.insert(primes[i]);
        }
        long number = 1;
        for(int i = 1; i < n; ++i) {
            number = pq.top(); pq.pop();
            for(int j = 0; j < 3; ++j) {
                if(!visited.count(primes[j] * number)) {
                    pq.push(primes[j] * number);
                    visited.insert(primes[j] * number);
                }
            }
        }
        return (int)number;

        // another method
        int uglys[n];
        uglys[0] = 1;
        int next = 0;
        int p2 = 0;
        int p3 = 0;
        int p5 = 0;
        while(++next < n) {
            int m = min(min(uglys[p2] * 2, uglys[p3] * 3), uglys[p5] * 5);
            uglys[next] = m;
            while(uglys[p2] * 2 <= m) {
                ++p2;
            }
            while(uglys[p3] * 3 <= m) {
                ++p3;
            }
            while(uglys[p5] * 5 <= m) {
                ++p5;
            }
        }
        return uglys[n - 1];
    }
```
13. [Merge K Sorted Lists](https://www.lintcode.com/problem/merge-k-sorted-lists/description?_from=ladder&&fromId=1)
```c++
 struct compare{
     bool operator() (const ListNode * A, const ListNode * B) const {
         return A->val > B->val;
     }
 };
class Solution {
public:
    /**
     * @param lists: a list of ListNode
     * @return: The head of one sorted list.
     */
    ListNode *mergeKLists(vector<ListNode *> &lists) {
        if(lists.empty()) {
            return nullptr;
        }
        priority_queue<ListNode *, vector<ListNode *>, compare> min_heap;
        for(int i = 0; i < lists.size(); ++i) {
            if(lists[i] == nullptr) {
                continue;
            }
            min_heap.push(lists[i]);
        }
        ListNode* dummy = new ListNode(0);
        ListNode * curNode = dummy;
        while(!min_heap.empty()) {
            curNode->next = min_heap.top();
            curNode = curNode->next;
            min_heap.pop();
            // add node
            if(curNode->next) {
                min_heap.push(curNode->next);
            }
        }
        return dummy->next;
    }
};

class Solution{
public:
    ListNode *mergeKLists(vector<ListNode *> &lists) {
        if(lists.size() == 1) {
            return lists[0];
        }
        vector<ListNode *> sublists;
        for(int i = 0; i < lists.size(); i+=2) {
            if(i == lists.size() - 1) {
                sublists.push_back(lists[i]);
            } else {
                sublists.push_back(merge(lists[i], lists[i + 1]));
            }
        }
        return mergeKLists(sublists);
    }
    
    ListNode * merge(ListNode * left, ListNode* right) {
        ListNode * dummy = new ListNode(0);
        ListNode * cur = dummy;
        while(left != nullptr && right != nullptr) {
            if(left->val < right->val) {
                cur->next = left;
                left = left->next;
            } else {
                cur->next = right;
                right = right->next;
            }
            cur = cur->next;
        }
        while(left != nullptr) {
            cur->next = left;
            left = left->next;
            cur = cur->next; // remember to also traverse thru cur
        }
        while(right != nullptr) {
            cur->next = right;
            right = right->next;
            cur = cur->next;
        }
        return dummy->next;
    }
};


class Solution{
public: 
    ListNode * mergeKLists(vector<ListNode*> lists) {
        // devide and conquer
        if(lists.empty()) {
            return nullptr;
        }
        return merge(lists, 0, lists.size() - 1);
    }
    
    ListNode * merge(vector<ListNode*> lists, int start, int end) {
        if(start == end) {
            return lists[start];
        }
        
        int mid = start + (end - start) / 2;
        ListNode * left = merge(lists, start, mid);
        ListNode * right = merge(lists, mid + 1, end);
        
        return merge(left, right);
    }
    
    ListNode * merge(ListNode * left, ListNode* right) {
        ListNode * dummy = new ListNode(0);
        ListNode * cur = dummy;
        while(left != nullptr && right != nullptr) {
            if(left->val < right->val) {
                cur->next = left;
                left = left->next;
            } else {
                cur->next = right;
                right = right->next;
            }
            cur = cur->next;
        }
        while(left != nullptr) {
            cur->next = left;
            left = left->next;
            cur = cur->next; // remember to also traverse thru cur
        }
        while(right != nullptr) {
            cur->next = right;
            right = right->next;
            cur = cur->next;
        }
        return dummy->next;
    }
};

// O (K N) worst case
// one linkedlist merge with another linkedList

// 3 methods O(nlgk)
// method 1
// use min_heap add first node of all list into a heap and pop and add 


// method 2
// one merge with two 3 merge with 4 
// like a tree w/ height  logk
// N * logk (only logk for every node)

// divided and conquer
// every layer is O(n) and logk is height like mergeSort
// mergeSort and quickSort !!! reallly imortant


```
13. [LRU Cache](https://www.lintcode.com/problem/lru-cache/description?_from=ladder&&fromId=1)
```c++
struct KeyValue {
    int val;
    int key;
    KeyValue * next;
    KeyValue * prev;
    KeyValue(int key, int val): key(key), val(val), prev(nullptr), next(nullptr) {}
};

class LRUCache {
private: 
    void moveToTail(KeyValue * cur) {
        // KeyValue * curCopy = cur;
        if(cur == tail) {
            return;
        }
        cur->prev->next = cur->next;
        cur->next->prev = cur->prev;
        tail->next = cur;
        cur->prev = tail;
        cur->next = nullptr;
        tail = cur;
    }
    KeyValue * head, * tail;
    unordered_map<int, KeyValue*> hash;
    int capacity, size;
    
public:
    LRUCache(int capacity): capacity(capacity), size(0){
        // dummy node
        head = new KeyValue(0, 0);
        tail = head;
    }
    
    int get(int key) {
        if (!hash.count(key)) {
            return -1;
        }
        moveToTail(hash[key]);
        return hash[key]->val;
    }
    
    void set(int key, int value) {
        if(hash.count(key)) {
            KeyValue * cur = hash[key];
            cur->val = value;
            moveToTail(cur);
            return;
        } 
        if(size < capacity) {
            KeyValue * cur = new KeyValue(key, value);
            hash[key] = cur;
            tail->next = cur;
            cur->prev = tail;
            tail = cur;
            ++size;
        } else {
            // delete first node and assume size always >= 1
            KeyValue * first = head->next;
            hash.erase(first->key);
            first->key = key;
            first->val = value;
            hash[key] = first;
            moveToTail(first);
        }
    }   
};

// version without prev ptr
class KeyValue {
public:
    int key, value;
    KeyValue *next;
    KeyValue(int key, int value) {
        next = NULL;
        this->key = key;
        this->value = value;
    }
    KeyValue() {
        this->next = NULL;
        this->key = 0;
        this->value = 0;
    }
};
class LRUCache{
private:
    void moveToTail(KeyValue *prev) {    
        if (prev->next == tail) {
            return;
        }
        
        KeyValue *node = prev->next;
        prev->next = node->next;
        if (node->next != nullptr) {
            hash[node->next->key] = prev;
        }
        tail->next = node;
        node->next = nullptr;
        hash[node->key] = tail;
        tail = node;
    }

    
public:
    unordered_map<int, KeyValue *> hash;
    KeyValue *head, *tail;
    int capacity, size;
    
    LRUCache(int capacity) {
        this->head = new KeyValue(0, 0); // dummy node
        this->tail = head;
        this->capacity = capacity;
        this->size = 0;
        hash.clear(); // clear the hash table
    }
    
    int get(int key) {
        if (!hash.count(key)) {
            return -1;
        }
        
        moveToTail(hash[key]);          //每次get，使用次数+1，最近使用，放于尾部
        return hash[key]->next->value;
    }
    
    void set(int key, int value) {       //数据放入缓存
        if (hash.find(key) != hash.end()) {      //key可以找到
            hash[key]->next->value = value;
            moveToTail(hash[key]);
        } else {
            KeyValue *node = new KeyValue(key, value);   //新建节点
            tail->next = node;                          //放于尾部
            hash[key] = tail;
            tail = node;
            size++;
            if (size > capacity) {                      //超出缓存上限
                hash.erase(head->next->key);            //删除头部数据
                // delete head->next;
                head->next = head->next->next;
                if (head->next != nullptr) { // actually not needed since head->next won't be null if it is not empty
                    hash[head->next->key] = head;
                }
                size--;
            }
        }
    }
};
```

### Memorization Search & Dynamic Programming
1. [Word Break III](https://www.lintcode.com/problem/word-break/description?_from=ladder&&fromId=1)
```c++
    // 设dp[i][j]表示从字典dict中组成子串str[i:j+1]有多少种方法。
    // 转移方程为：dp[i][j]=\sum_{k=i}^{j}dp[i][k]*dp[k+1][j]

    int wordBreak3(string& s, unordered_set<string>& dict) {
        vector<vector<int>> dp(s.length(), vector<int>(s.length()));
                
        transform(s.begin(), s.end(), s.begin(), ::tolower);
        unordered_set<string> hs;
        for(string x : dict) {
            transform(x.begin(), x.end(), x.begin(), ::tolower);
            hs.insert(x);
        }
        
        // initialize 
        for(int i = 0; i < s.length(); ++i) {
            for(int j = i; j < s.length(); ++j) {
                if(hs.count(s.substr(i, j - i + 1))) {
                    dp[i][j] = 1;
                }
            }
        }
        
        for(int i = 0; i < s.length(); ++i) {
            for(int j = i; j < s.length(); ++j) {
                for(int k = i; k < j; ++k) {
                    dp[i][j] += dp[i][k] * dp[k + 1][j];
                }
            }
        }
        
        return dp[0][s.length() - 1];
    }
```
2. [Word Break II]()
```c++
```
3. [Word Break]()
```c++
    int getMaxLen(unordered_set<string> & dict) {
        int maxLen = 0;
        for(unordered_set<string>::iterator it = dict.begin(); it != dict.end(); ++it) {
            maxLen = max(maxLen, (int)it->length());
        }
        return maxLen;
    }

    bool wordBreak(string &s, unordered_set<string> &dict) {
        if(s.length() == 0) {
            return true;
        }
        if(dict.size() == 0) {
            return false;
        }
        vector<bool> canSeg(s.length() + 1, false);
        canSeg[0] = true;
        int maxLen = getMaxLen(dict);
        for(int i = 1; i <= s.length(); ++i) {
            for(int j = min(maxLen, i); j >= 1; --j) {
                if(!canSeg[i - j]) { // since canSeg is not 0 based
                    continue;
                }
                string word = s.substr(i - j, j);
                if(dict.count(word)) {
                    canSeg[i] = true;
                    break;
                }
            }
        }
        return canSeg[s.length()];
        // or 
        vector<bool> canSeg(s.length(), false);
        int maxLen = getMaxLen(dict);
        canSeg[0] = dict.count(s.substr(0, 1));
        for(int i = 1; i < s.length(); ++i) {
            for(int j = min(maxLen, i + 1); j >= 1; --j) {
                if(i - j >= 0 && !canSeg[i - j]) {
                    continue;
                }
                if(dict.count(s.substr(i - j + 1, j))) {
                    canSeg[i] = true;
                    break;
                }
            }
        }
        return canSeg[s.length() - 1];
    } 
        if(s.length() == 0) {
            return true;
        }
        if(dict.size() == 0) {
            return false;
        }

        int maxLen = getMaxLen(dict);
        bool canSegment[s.length() ] = {false};
        canSegment[0] = sdict.count(s.substr(0, 1));

        for(int i = 1; i < s.length(); ++i) {
            for(int j = min(maxLen, i + 1); j >= 1; --j) {
                if(i >= j && !canSegment[i - j]) {
                    continue;
                }
                string word = s.substr(i - j + 1, j);
                if(dict.find(word) != dict.end()) {
                    canSegment[i] = true;
                    break;
                }
            }
        }
        return canSegment[s.length() - 1];
```
4. [Triangle](https://www.lintcode.com/problem/triangle/description?_from=ladder&&fromId=1)
```c++
    int minimumTotal(vector<vector<int>> &triangle) {
        if(triangle.size() == 0) {
            return -1;
        }
        vector<int> mark(triangle.size(), 0);
        for(int i = 0; i < triangle.size(); ++i) {
            for(int j = i; j >= 0; --j) { // have to start from the back or we would lose the info mark[j - 1]
                if(j == 0) {
                    mark[j] = mark[j] + triangle[i][j];
                } else if (j == triangle[i].size() - 1) {
                    mark[j] = mark[j - 1] + triangle[i][j];
                } else {
                    mark[j] = min(mark[j], mark[j - 1]) + triangle[i][j];
                }
            }
        }
        int ret = INT_MAX;
        for(int val: mark) {
            ret = min(ret, val);
        }
        return ret;
    }
```
5. [Longest Increasing Subsequence](https://www.lintcode.com/problem/longest-increasing-subsequence/description?_from=ladder&&fromId=1)
```c++
    //  dp
    //  Dp[i] 表示以第i个数字为**结尾**的最长上升子序列的长度。
    //  对于每个数字，枚举前面所有小于自己的数字 j，Dp[i] = max{Dp[j]} + 1. 如果没有比自己小的，Dp[i] = 1;
    // O(n^2)


    int longestIncreasingSubsequence(vector<int> &nums) {
        // dp
        vector<int> dp(nums.size(), 0);
        int maxSub = 0;
        for(int i = 0; i < nums.size(); ++i) {
            dp[i] = 1;
            // find the the max dp val and the element at that index is smaller

            for(int j = 0; j < i; ++j) {
                if(nums[j] < nums[i]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
            maxSub = max(maxSub, dp[i]);
        }
        return maxSub;
    }
```

### DP
1. [Unique Path II](https://www.lintcode.com/problem/unique-paths-ii/description?_from=ladder&&fromId=1)
```c++
    int uniquePathsWithObstacles(vector<vector<int>> &obstacleGrid) {
        if(obstacleGrid.size() == 0) {
            return 0;
        }
        int m = obstacleGrid.size(), n = obstacleGrid[0].size();
        
        vector<vector<int>> paths(m, vector<int>(n, 0));
        
        for(int i = 0; i < m; ++i) {
            if(obstacleGrid[i][0] == 1) {
                break;
            }
            paths[i][0] = 1;
        }
        for(int j = 0; j < n; ++j) {
            if(obstacleGrid[0][j] == 1) {
                break;
            }
            paths[0][j] = 1;
        }
        
        for(int i = 1; i < m; ++i) {
            for(int j = 1; j < n; ++j) {
                if(obstacleGrid[i][j] == 1) {
                    continue;
                }
                paths[i][j] = paths[i - 1][j] + paths[i][j - 1];
            }
        }
        
        return paths[m - 1][n - 1];
    }
```
2. [Unique Path](https://www.lintcode.com/problem/unique-paths/description?_from=ladder&&fromId=1)
```c++
    int uniquePaths(int m, int n) {
        if(m == 0 || n == 0) {
            return 0;
        }
        vector<vector<int>> paths(m, vector<int>(n, 0));
        // initialize
        for(int i = 0; i < m; ++i) {
            paths[i][0] = 1;
        }
        for(int i = 0; i < n; ++i) {
            paths[0][i] = 1;
        }
        // sub problems
        for(int i = 1; i < m; ++i) {
            for(int j = 1; j < n; ++j) {
                paths[i][j] = paths[i - 1][j] + paths[i][j - 1];
            }
        }
        
        return paths[m - 1][n - 1];
    }
```
3. [Jump Game](https://www.lintcode.com/problem/jump-game/description?_from=ladder&&fromId=1)
```c++
    bool canJump(vector<int> &A) {
        vector<bool> f(A.size(), false);
        f[0] = true;
        for(int i = 1; i < A.size(); ++i) {
            for(int j = 0; j < i; ++j) {
                if(f[j] && A[j] >= i - j ) {
                    f[i] = true;
                    break;
                }
            }
        }
        return f[A.size() - 1];
    }
     bool canJump(vector<int> &A) {
        vector<bool> f(A.size(), false);
        f[0] = true;
        for(int i = 0; i < A.size(); ++i) {
            if(!f[i]) {
                continue;
            }
            for(int j = 1; j <= A[i] && i + j < A.size(); ++j) {
                f[i + j] = true;
            }
        }
        return f[A.size() - 1];
    }
    bool canJump(vector<int>& nums) {
        // vector<bool> dp(nums.size(), false);
        // dp[0] = true;
        // for(int i = 1; i < nums.size(); ++i) {
        //     for(int j = 0; j < i; ++j) {
        //         if(dp[j] && j + nums[j] >= i) {
        //             dp[i] = true;
        //             break;
        //         }
        //     }
        // }
        // return dp[nums.size() - 1];
        int i = 0;
        for (int reach = 0; i < nums.size() && i <= reach; ++i)
            reach = max(i + nums[i], reach);
        return i == nums.size();
    }
```
3. [Climbing Stairs](https://www.lintcode.com/problem/climbing-stairs/?_from=ladder&&fromId=1)
```c++
    int climbStairs(int n) {
        if(n <= 1) {
            return n;
        }
        vector<int> f(n + 1, 0);
        f[1] = 1;
        f[2] = 2;

        for(int i = 3; i <= n; ++i) {
            f[i] = f[i - 1] + f[i - 2];
        }
        
        return f[n];
    }
```
4. [Min Path Sum](https://www.lintcode.com/problem/minimum-path-sum/description?_from=ladder&&fromId=1)
```c++
    int minPathSum(vector<vector<int>> &grid) {
        if(grid.size() == 0) {
            return 0;
        }
        int m = grid.size(), n = grid[0].size();
        
        vector<vector<int>> sums(m, vector<int>(n, 0));
        
        sums[0][0] = grid[0][0];
        for(int i = 1; i < m; ++i) {
            sums[i][0] = grid[i][0] + sums[i - 1][0];
        }
        for(int j = 1; j < n; ++j) {
            sums[0][j] = grid[0][j] + sums[0][j - 1];
        }
        
        for(int i = 1; i < m; ++i) {
            for(int j = 1; j < n; ++j) {
                sums[i][j] = grid[i][j] + min(sums[i - 1][j], sums[i][j - 1]);
            }
        }
        
        return sums[m - 1][n - 1];
    }
```
5. [Largest Divisible Subset](https://www.lintcode.com/problem/largest-divisible-subset/?_from=ladder&&fromId=1)
```c++
    vector<int> largestDivisibleSubset(vector<int> &nums) {
        // largest subset 
i        // father here represents the preceding position
        sort(nums.begin(), nums.end());
        vector<int> dp(nums.size(), 1), father(nums.size(), -1), res;
        int maxLen = 1, max_index = 0; // max_index is 0 so that we can return it when there's no relationship between elements
        
        for(int i = 1; i < nums.size(); ++i) {
            for(int j = 0; j < i; ++j) {
                if(nums[i] % nums[j] == 0 && dp[i] < dp[j] + 1) {
                    dp[i] = dp[j] + 1;
                    father[i] = j;
                    if(dp[i] > maxLen) {
                        maxLen = dp[i];
                        max_index = i;
                    }
                }
            }
        }
        
        while(maxLen-- > 0){
            res.push_back(nums[max_index]);
            max_index = father[max_index];
        }
    
        return res;
    }
```
6. [Russian Doll Envelop](https://www.lintcode.com/problem/russian-doll-envelopes/description?_from=ladder&&fromId=1)
```c++
    int maxEnvelopes(vector<pair<int, int>>& envelopes) {
        int n = envelopes.size();
        sort(envelopes.begin(), envelopes.end(), cmp);
        // vector<int> dp(n);
        
        // similar to longest increasing subsequence
        // int maxNum = 0;
        // for (int i = 0; i < n; i++) {
        //     dp[i] = 1; // lsb of height
        //     for(int j = 0; j < i; j++) {
        //         if(envelopes[j].second < envelopes[i].second) {
        //             dp[i] = max(dp[i], dp[j] + 1);
        //         }
        //     }
        //     maxNum = max(dp[i], maxNum);
            
        // }
        
        for (auto e : envelopes){
            auto iter = lower_bound(dp.begin(), dp.end(), e.second);
            // lower bound Returns an iterator pointing to the first element in the range [first,last) which does not compare less than val.
            // Unlike upper_bound, the value pointed by the iterator returned by this function may also be equivalent to val, and not only greater.
            // couldn't have upper bound here since then we may create same number again and again in a single sequence 

            if (iter == dp.end())
                dp.push_back(e.second);
            else if (e.second < *iter)
                *iter = e.second;
        }
    }
```

### Additional Problems
1. [Intersection of Two Arrays](https://www.lintcode.com/problem/intersection-of-two-arrays/description?_from=ladder&&fromId=1)
```c++
    vector<int> intersection(vector<int> &nums1, vector<int> &nums2) {
        sort(nums1.begin(), nums1.end());
        sort(nums2.begin(), nums2.end());
        vector<int> result;
        
        int ptr1 = 0, ptr2 = 0;
        while(ptr1 < nums1.size() && ptr2 < nums2.size()) {
            while(ptr1 + 1 < nums1.size() && nums1[ptr1] == nums1[ptr1 + 1]) {
                ++ptr1;
            }
            while(ptr2 + 1 < nums2.size() && nums2[ptr2] == nums2[ptr2 + 1]) {
                ++ptr2;
            }
            if(nums1[ptr1] == nums2[ptr2]) {
                result.push_back(nums1[ptr1]);
                ++ptr1; ++ptr2;
            } else if (nums1[ptr1] > nums2[ptr2]) {
                ++ptr2;
            } else {
                ++ptr1;
            }
        }
        return result;
    }
```
2. [Subarray Sum](https://www.lintcode.com/problem/subarray-sum/description?_from=ladder&&fromId=1)
```c++
    vector<int> subarraySum(vector<int> &nums) {
        unordered_map<int, int> hash; // find sum 0
        int sum = 0;
        hash[0] = -1;
        for(int i = 0; i < nums.size(); ++i) {
            sum += nums[i];
            if(hash.count(sum)) { // note is not -sum here is sum 
                // find
                return {hash[sum] + 1, i};
            }
            hash[sum] = i;
        }
        return {}; // not found
    }
```
3. [Merge Sorted Array](https://www.lintcode.com/problem/merge-sorted-array/description?_from=ladder&&fromId=1)
```c++
    void mergeSortedArray(int A[], int m, int B[], int n) { // key here is merge B into A
        int left = m - 1, right =  n - 1;
        // know the size would be m + n
        int ptr = m + n - 1;
        while(left >= 0 && right >= 0) {
            if(A[left] <= B[right]) {
                A[ptr--] = B[right--];
            } else {
                A[ptr--] = A[left--];
            }
        }
        while(left >= 0) {
            A[ptr--] = A[left--];
        }
        while(right >= 0) {
            A[ptr--] = B[right--];
        }
    }
```
4. [Maximum Subarray](https://www.lintcode.com/problem/maximum-subarray/description?_from=ladder&&fromId=1)
```c++
    int maxSubArray(vector<int> &nums) {
        // prefix sum so clever
        if(nums.size() == 0) {
            return 0;
        }
        if(nums.size() == 1) {
            return nums[0];
        }
        
        int sum = 0, maxSum = INT_MIN, prevMinSum = 0; // prevMinSum should be 0 instead pf nums[0] or we won't properly handle the first ele
        for(int i = 0; i < nums.size(); ++i) {
            sum += nums[i];
            maxSum = max(maxSum, sum - prevMinSum);
            prevMinSum = min(prevMinSum, sum);
        }
        return maxSum;
    }
```
5. [Subarray Sum Closest](https://www.lintcode.com/problem/subarray-sum-closest/description?_from=ladder&&fromId=1)
```c++
    class Solution {
public:
    /*
     * @param nums: A list of integers
     * @return: A list of integers includes the index of the first number and the index of the last number
     */
    struct Pair{
        int pos;
        int sum;
        Pair(int _pos, int _sum):pos(_pos), sum(_sum){}
        bool operator <(const Pair & rhs)const {
            return sum < rhs.sum || sum == rhs.sum && pos < rhs.pos;
        }
    };
    vector<int> subarraySumClosest(vector<int> &nums) {
        // sort it and take the min distance between 
        // no need to do the sum part
        if(nums.size() <= 1) {
            return {0, 0};
        }
        
        vector<Pair> sums;
        sums.push_back(Pair(-1, 0)); // important!
        int sum = 0;
        for(int i = 0; i < nums.size(); ++i) {
            sum += nums[i];
            sums.push_back(Pair(i, sum));
        }
        sort(sums.begin(), sums.end());
        for(Pair a:sums) {
            std::cout<<a.pos;
            std::cout<<" ";
            std::cout<<a.sum<<std::endl;
        }
        // compute closest sum
        int minDis = sums[1].sum - sums[0].sum;
        Pair minPair = sums[0];
        Pair maxPair = sums[1];
        
        for(int i = 1; i < nums.size(); ++i) {
            int diff = sums[i + 1].sum - sums[i].sum;
            if(diff < minDis) {
                minDis = diff;
                minPair = sums[i];
                maxPair = sums[i + 1];
            }
        }
        int minPos = min(minPair.pos, maxPair.pos);
        int maxPos = max(minPair.pos, maxPair.pos);
        return {minPos + 1, maxPos};
    }
};
```
6. [Merge Two Interval Lists](https://www.lintcode.com/problem/merge-two-sorted-interval-lists/?_from=ladder&&fromId=1)
```c++
    vector<Interval> mergeTwoInterval(vector<Interval> &list1, vector<Interval> &list2) {
        vector<Interval> results;
        int ptr1 = 0, ptr2 = 0;
        Interval last(0, 0), cur(0, 0); // a replacement first 
        while(ptr1 < list1.size() && ptr2 < list2.size()) {
            // one way
            if(list1[ptr1].start < list2[ptr2].start) {
                cur = list1[ptr1++];
                
            } else {
                cur = list2[ptr2++];
            }
            last = merge(results, last, cur);
            
        }
        while(ptr1 < list1.size()) {
             cur = list1[ptr1++];
            last = merge(results, last, cur);
        }
        while(ptr2 < list2.size()) {
            cur = list2[ptr2++];
            last = merge(results, last, cur);
        }
        
        if(last.start != 0 && last.end != 0) {
            results.push_back(last);
        }
        
        return results;
    }
    
    // merge would merge the current with the previous last one that is not added and a new one to compare
    Interval merge(vector<Interval> & results, Interval & last, Interval & cur) {
        if(last.start == 0 && last.end == 0) {
            return cur;
        }
        if(last.end < cur.start) {
            results.push_back(last);
            return cur;
        }
        // last.end >= cur.start
        last.end = max(last.end, cur.end);
        return last;
    }
```
7. [Best Time to Buy and Sell Stock](https://www.lintcode.com/problem/best-time-to-buy-and-sell-stock/description?_from=ladder&&fromId=1)
```c++
    int maxProfit(vector<int> &prices) {
        // cur - lowest perv element
        int prevMin = INT_MAX, maxProfit = 0;
        for(int ele: prices) {
            maxProfit = max(maxProfit, ele - prevMin);
            prevMin = min(ele, prevMin);
        }
        return maxProfit;
    }
```
8. [Best Time to Buy and Sell Stock: Unlimited Time](https://www.lintcode.com/problem/best-time-to-buy-and-sell-stock-ii/description?_from=ladder&&fromId=1)
```c++
    int maxProfit(vector<int> &prices) {
        int ret = 0;
        for(int i=1; i<prices.size(); i++) {
            ret += prices[i]>prices[i-1] ? prices[i]-prices[i-1] : 0;
        }
        return ret;
    }
```
9. [Best Time to Buy and Sell Stock: k times](https://www.lintcode.com/problem/best-time-to-buy-and-sell-stock-iv/description?_from=ladder&&fromId=1)
```c++
    int maxProfit(int K, vector<int> &prices) {
        // f[k, ii] represents the max profit up until prices[ii] (Note: NOT ending with prices[ii]) using at most k transactions. 
        // f[k, ii] = max(f[k, ii-1], prices[ii] - prices[jj] + f[k-1, jj]) { jj in range of [0, ii-1] }
        //          = max(f[k, ii-1], prices[ii] + max(f[k-1, jj] - prices[jj]))
        // f[0, ii] = 0; 0 times transation makes 0 profit
        // f[k, 0] = 0; if there is only one price data point you can't make any money no matter how many times you can trade
        if (prices.size() <= 1) return 0;
        if (K > prices.size() / 2){ // simple case
            int ans = 0;
            for (int i=1; i<prices.size(); ++i){
                ans += max(prices[i] - prices[i-1],0);
            }
            return ans;
        }
        else {
            // int K = 2; // number of max transation allowed
            vector<vector<int>> f(K+1, vector<int>(prices.size(), 0));
            for (int kk = 1; kk <= K; kk++) {
                int tmpMax = f[kk-1][0] - prices[0];
                for (int ii = 1; ii < prices.size(); ii++) {
                    f[kk][ii] = max(f[kk][ii-1], prices[ii] + tmpMax);
                    tmpMax = max(tmpMax, f[kk-1][ii] - prices[ii]);
                }
            }
            return f[K][prices.size() - 1];
        }
    }
```
10. [Max Product Subarray](https://www.lintcode.com/problem/maximum-product-subarray/description?_from=ladder&&fromId=1)
```c++
    // https://wdxtub.com/interview/14520604915082.html
    int maxProduct(vector<int> &nums) { 
        vector<int> f(nums.size()), g(nums.size());
        f[0] = nums[0];
        g[0] = nums[0];
        int m = nums[0];
        for(int i = 1; i < nums.size(); ++i) {
            f[i] = max(max(nums[i], f[i - 1] * nums[i]), g[i - 1] * nums[i]);
            g[i] = min(min(nums[i], f[i - 1] * nums[i]), g[i - 1] * nums[i]);
            m = max(f[i], m);
        }
        return m;
    }
```
11. [Median of Two Sorted Arrays](https://www.lintcode.com/problem/median-of-two-sorted-arrays/description?_from=ladder&&fromId=1)
- could just use merge and count O(m + n)
- [some advanced algorithm O(log(m + n))](https://www.geeksforgeeks.org/median-two-sorted-arrays-different-sizes-ologminn-m/
https://blog.csdn.net/yutianzuijin/article/details/11499917)



### Others
[sell stock k times max profit](https://www.geeksforgeeks.org/maximum-profit-by-buying-and-selling-a-share-at-most-k-times/)
```c++
// Function to find out maximum profit by buying 
// & selling a share atmost k times given stock 
// price of n days 
int maxProfit(int price[], int n, int k) 
{ 
    // table to store results of subproblems 
    // profit[t][i] stores maximum profit using 
    // atmost t transactions up to day i (including 
    // day i) 
    int profit[k + 1][n + 1]; 
  
    // For day 0, you can't earn money 
    // irrespective of how many times you trade 
    for (int i = 0; i <= k; i++) 
        profit[i][0] = 0; 
  
    // profit is 0 if we don't do any transation 
    // (i.e. k =0) 
    for (int j = 0; j <= n; j++) 
        profit[0][j] = 0; 
  
    // fill the table in bottom-up fashion 
    for (int i = 1; i <= k; i++) { 
        for (int j = 1; j < n; j++) { 
            int max_so_far = INT_MIN; 
  
            for (int m = 0; m < j; m++) 
                max_so_far = max(max_so_far, 
                                 price[j] - price[m] + profit[i - 1][m]); 
  
            profit[i][j] = max(profit[i][j - 1], max_so_far); 
        } 
    } 
  
    return profit[k][n - 1]; 
} 
```
[Buy and sell stock unlimited times](https://www.geeksforgeeks.org/stock-buy-sell/)
```c++
// This function finds the buy sell 
// schedule for maximum profit 
void stockBuySell(int price[], int n) 
{ 
    // Prices must be given for at least two days 
    if (n == 1) 
        return; 
  
    int count = 0; // count of solution pairs 
  
    // solution vector 
    Interval sol[n / 2 + 1]; 
  
    // Traverse through given price array 
    int i = 0; 
    while (i < n - 1) { 
        // Find Local Minima. Note that the limit is (n-2) as we are 
        // comparing present element to the next element. 
        while ((i < n - 1) && (price[i + 1] <= price[i])) 
            i++; 
  
        // If we reached the end, break 
        // as no further solution possible 
        if (i == n - 1) 
            break; 
  
        // Store the index of minima 
        sol[count].buy = i++; 
  
        // Find Local Maxima. Note that the limit is (n-1) as we are 
        // comparing to previous element 
        while ((i < n) && (price[i] >= price[i - 1])) 
            i++; 
  
        // Store the index of maxima 
        sol[count].sell = i - 1; 
  
        // Increment count of buy/sell pairs 
        count++; 
    } 
  
    // print solution 
    if (count == 0) 
        cout << "There is no day when buying"
             << " the stock will make profitn"; 
    else { 
        for (int i = 0; i < count; i++) 
            cout << "Buy on day: " << sol[i].buy 
                 << "\t Sell on day: " << sol[i].sell << endl; 
    } 
  
    return; 
} 
```
[Reverse a Linked List](https://www.lintcode.com/problem/reverse-linked-list/description)
```c++
ListNode* reverseList(ListNode* head){
    if(!head || !head->next) {
        return head;
    }
    ListNode * pre = nullptr;
    ListNode * cur = head;
    while(cur) {
        ListNode * next = cur->next;
        cur->next = pre;
        pre = cur;
        cur = next;
    }
    return pre;
}

    // recursive version
    ListNode* reverseList(ListNode* head) {
        if (!head || !head->next) return head;
        ListNode *newHead = reverseList(head->next);
        head->next->next = head;
        head->next = NULL;
        return newHead;
    }
```
[Minimum Size Subarray Sum](https://www.lintcode.com/problem/minimum-size-subarray-sum/description)
```c++
    int minimumSize(vector<int> &nums, int s) {
        // two pointer
        int right = 0, left = 0;
        int len = INT_MAX;
        int sum = 0;
        while(right < nums.size()) {
            sum += nums[right];
            while (sum >= s) {
                len = min(len, right - left + 1);
                sum -= nums[left++];
            }
            ++right;
        }
        return len == INT_MAX ? -1 : len;
    }
```

```c++
sort(vector.begin(), vector.end(), greater<int>());  // sort in descending order
// in particular order
struct Interval 
{ 
    int start, end; 
}; 
bool compareInterval(Interval i1, Interval i2) 
{ 
    return (i1.start < i2.start); 
}  
sort(vector.begin(), vector.end(), compareInterval); 
// result: [1,9] [2,4] [4,7] [6,8] 
```
[Remove Duplicates from Unsorted List](https://www.jiuzhang.com/solutions/remove-duplicates-from-unsorted-list/#tag-highlight-lang-cpp)
```c++
    ListNode *removeDuplicates(ListNode *head) {
        map<int,bool> mp;
        if (!head)
            return head;
        mp[head->val] = true;
        ListNode *tail = head;
        ListNode *now = head->next;
        while (now) {
            if (mp.find(now->val) == mp.end()) {
                mp[now->val] = true;
                tail->next = now;
                tail = tail->next;
            }
            now = now->next;
        }
        tail->next = nullptr;
        return head;
    }
```
[Random Node from Linked List]()
```c++
    /** shuffle an array */
    for (int i = arr.size() – 1; i > 0; i–)
    {
        // Pick a random index from 0 to i
        int j = rand() % (i + 1);

        // Swap arr[i] with the element
        // at random index
        swap(arr[i], arr[j]);
    }

    // head is the head ptr
    /** Returns a random node's value. */
    int getRandom() {
        int res = head->val, i = 2;
        ListNode *cur = head->next;
        while (cur) {
            int j = rand() % i;
            if (j == 0) res = cur->val;
            ++i;
            cur = cur->next;
        }
        return res;
    }
```

```c++
// Deletes the second element (vec[1])
vec.erase(vec.begin() + 1);
// Deletes the second through third elements (vec[1], vec[2])
vec.erase(vec.begin() + 1, vec.begin() + 3);
```
[Minimum Window Substring](https://www.lintcode.com/problem/minimum-window-substring/description?_from=ladder&&fromId=97)
```c++
    string minWindow(string s, string t) {
        // two ptrs first moving the right one to expand and then move the left one
        // it won't affect other char not in t because when they -1 they won't >= 0 and when they + 1 the won't > 0
        string res = "";
        unordered_map<char, int> letterCnt;
        int left = 0, cnt = 0, minLen = INT_MAX;
        for (char c : t) ++letterCnt[c];
        for (int i = 0; i < s.size(); ++i) {
            if (--letterCnt[s[i]] >= 0) ++cnt;
            while (cnt == t.size()) {
                if (minLen > i - left + 1) {
                    minLen = i - left + 1;
                    res = s.substr(left, minLen);
                }
                if (++letterCnt[s[left]] > 0) --cnt;
                ++left;
            }
        }
        return res;
    }
    // Reference: https://www.cnblogs.com/grandyang/p/4340948.html
```

[Inorder Successor in BST](https://www.lintcode.com/problem/inorder-successor-in-bst/description)
https://www.geeksforgeeks.org/inorder-successor-in-binary-search-tree/
```c++
    TreeNode * inorderSuccessor(TreeNode * root, TreeNode * p) {
        TreeNode * sucessor = nullptr;
        while(root && root->val != p->val) {
            if(p->val < root->val) {
                sucessor = root;
                root = root->left;
            } else {
                root = root->right;
            }
        }
        if(!root) {
            return nullptr;
        }
        if(!root->right) {
            return sucessor;
        }
        root = root->right;
        while(root->left) {
            root = root->left;
        }
        return root;
    }

// using parent ptrs
//     1) If right subtree of node is not NULL, then succ lies in right subtree. Do following.
// Go to right subtree and return the node with minimum key value in right subtree.
// 2) If right sbtree of node is NULL, then succ is one of the ancestors. Do following.
// Travel up using the parent pointer until you see a node which is left child of it’s parent. The parent of such a node is the succ.

struct node * inOrderSuccessor(struct node *root, struct node *n) 
{ 
  // step 1 of the above algorithm  
  if( n->right != NULL ) 
    return minValue(n->right); 
  
  // step 2 of the above algorithm 
  struct node *p = n->parent; 
  while(p != NULL && n == p->right) 
  { 
     n = p; 
     p = p->parent; 
  } // now n is the left child of its parent

  return p; 
} 
```
[Inorder Predecessor in BST](https://www.lintcode.com/problem/inorder-predecessor-in-bst/)
```c++
    TreeNode * inorderPredecessor(TreeNode * root, TreeNode * p) {
        TreeNode * pre = nullptr;
        while(root && root->val != p->val) {
            if(p->val > root->val) {
                pre = root;
                root = root->right;
            } else {
                root = root->left;
            }
        }
        if(!root) {
            return nullptr;
        }
        
        if(!root->left) {
            return pre;
        }
        root = root->left;
        while(root->right) {
            root = root->right;
        }
        return root;
    }
```

[Product Except Self]()
```c++
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> res(nums.size(), 1);
        for(int i = 0; i < nums.size() - 1; ++i) {
            res[i + 1] = res[i] * nums[i];
        }
        int bwd = 1;
        for(int i = nums.size() - 1; i >= 0; --i) {
            res[i] *= bwd;
            bwd *= nums[i];
        }
        return res;
    }
```

```c++
std::transform(word.begin(), word.end(), word.begin(), ::tolower);
unordered_set<string> ban(banned.begin(), banned.end()); // banned is a vector
```
[anagram](https://leetcode.com/problems/valid-anagram/)
```c++
    bool isAnagram(string s, string t) {
        if(s.length() != t.length()) return false;
        std::unordered_map<int, int> map;
        for(int i = 0; i < s.length(); ++i) {
            map[s[i]]++;
            map[t[i]]--;
        }
        for (std::unordered_map<int, int>::iterator  it = map.begin(); it != map.end(); ++it)
       {
            if(it->second != 0) return false;
       }
        return true;
    }
```
[Group anagram](https://leetcode.com/problems/group-anagrams/discuss/?currentPage=1&orderBy=most_votes&query=)
```c++
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> mp;
        for (string s : strs) {
            string t = s; 
            sort(t.begin(), t.end());
            mp[t].push_back(s);
        }
        vector<vector<string>> anagrams;
        for (auto p : mp) { 
            anagrams.push_back(p.second);
        }
        return anagrams;
    }
// counting sort 
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> mp;
        for (string s : strs) {
            mp[strSort(s)].push_back(s);
        }
        vector<vector<string>> anagrams;
        for (auto p : mp) { 
            anagrams.push_back(p.second);
        }
        return anagrams;
    }
private:
    string strSort(string s) {
        int counter[26] = {0};
        for (char c : s) {
            counter[c - 'a']++;
        }
        string t;
        for (int c = 0; c < 26; c++) {
            t += string(counter[c], c + 'a');
        }
        return t;
    }
};
```
[Longest Arithmetic Sequence](https://leetcode.com/problems/longest-arithmetic-sequence/)
```c++
    int longestArithSeqLength(vector<int>& A) {
        unordered_map<int, unordered_map<int, int>> dp;
        int res = 2, n = A.size();
        for (int i = 0; i < n; ++i)
            for (int j = i + 1; j < n; ++j)  {
                int d = A[j] - A[i];
                dp[d][j] = dp[d].count(i) ? dp[d][i] + 1 : 2;
                res = max(res, dp[d][j]);
            }
        return res;
    }
```
[Remove K digits](https://www.cnblogs.com/grandyang/p/5883736.html)
```c++
    string removeKdigits(string num, int k) {
        string res = "";
        int n = num.size(), keep = n - k;
        for (char c : num) {
            while (k && res.size() && res.back() > c) {
                res.pop_back();
                --k;
            }
            res.push_back(c);
        }
        res.resize(keep);
        while (!res.empty() && res[0] == '0') res.erase(res.begin());
        return res.empty() ? "0" : res;
    }
```
[Check if it is complete](https://leetcode.com/discuss/interview-question/307340/Facebook-phone-interview-Check-if-BT-is-CBT)
```c++
   bool isCompleteTree(TreeNode * root) {
        Queue<TreeNode *> queue;
        queue.push(root);
        bool seenEmpty = false;
        
        while(!queue.empty()) {
            TreeNode* curr = queue.front();
            queue.pop();
            if (curr == nullptr) {
                seenEmpty = true;
                continue;
            } else if (seenEmpty) {
                return false;
            }
            
            queue.push(curr.left);
            queue.push(curr.right);
        }

        return true;
    }
```
[Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/discuss/75350/Clean-C%2B%2B-Solution-and-Explaination-O(mn)-space-with-O(1)-time)
```c++
class NumMatrix {
private:
    int row, col;
    vector<vector<int>> sums;
public:
    NumMatrix(vector<vector<int>> &matrix) {
        row = matrix.size();
        col = row>0 ? matrix[0].size() : 0;
        sums = vector<vector<int>>(row+1, vector<int>(col+1, 0));
        for(int i=1; i<=row; i++) {
            for(int j=1; j<=col; j++) {
                sums[i][j] = matrix[i-1][j-1] + 
                             sums[i-1][j] + sums[i][j-1] - sums[i-1][j-1] ;
            }
        }
    }

    int sumRegion(int row1, int col1, int row2, int col2) {
        return sums[row2+1][col2+1] - sums[row2+1][col1] - sums[row1][col2+1] + sums[row1][col1];
    }
};
```
[Inorder Tree Traversal without Recursion]()
```c++
/* Iterative function for inorder tree 
   traversal */
void inOrder(struct Node *root) 
{ 
    stack<Node *> s; 
    Node *curr = root; 
  
    while (curr != NULL || s.empty() == false) 
    { 
        /* Reach the left most Node of the 
           curr Node */
        while (curr !=  NULL) 
        { 
            /* place pointer to a tree node on 
               the stack before traversing 
              the node's left subtree */
            s.push(curr); 
            curr = curr->left; 
        } 
  
        /* Current must be NULL at this point */
        curr = s.top(); 
        s.pop(); 
  
        cout << curr->data << " "; 
  
        /* we have visited the node and its 
           left subtree.  Now, it's right 
           subtree's turn */
        curr = curr->right; 
  
    } /* end of while */
} 
```
[Trim BST]()
```c++
 TreeNode* trimBST(TreeNode* root, int L, int R) {
        if (!root) return nullptr;
        if (root->val < L) return trimBST(root->right, L, R);
        if (root->val > R) return trimBST(root->left, L, R);
        root->left = trimBST(root->left, L, R);
        root->right = trimBST(root->right, L, R);
        return root;
    }
```
[Add Two numbers II](https://leetcode.com/problems/add-two-numbers-ii/)
```c++
    stack<int> s1;
    stack<in2> s2;
    while (l1) {
        s1.push(l1->val);
        l1 = l1->next;
    }
    while (l2) {
        s2.push(l2->val);
        l2 = l2->next;
    }
    ListNode * retHead = new ListNode(0);
    int pre = 0;
    while (!s1.empty() || !s2.empty() || pre > 0) {
        int tmp1 = s1.empty() ? 0 : s1.top(); s1.pop();
        int tmp2 = s2.empty() ? 0 : s2.top(); s2.pop();
        int sum = tmp1 + tmp2 + pre;
        pre = sum / 10;
        ListNode next = new ListNode(sum % 10);
        next->next = retHead->next;
        retHead->next = next;
    }
    return retHead->next;
```
[Partial Labels](https://leetcode.com/problems/partition-labels/submissions/)
```c++
    vector<int> partitionLabels(string S) {
        vector<int> res;
        unordered_map<char, int> lastSeen;
        for(int i = 0; i < S.length(); ++i) {
            lastSeen[S[i]] = i;
        }
        int last = 0, start = 0;
        for(int i = 0; i < S.length(); ++i) {
            last = max(last, lastSeen[S[i]]);
            if (i == last) {
                res.push_back(i - start + 1);
                start = i + 1;
            }
        }
        return res;
    }
```

[Rectangle Overlap](https://www.geeksforgeeks.org/find-two-rectangles-overlap/)
```c++
// Returns true if two rectangles (l1, r1) and (l2, r2) overlap 
bool doOverlap(Point l1, Point r1, Point l2, Point r2) 
{ 
    // If one rectangle is on left side of other 
    if (l1.x > r2.x || l2.x > r1.x) 
        return false; 
  
    // If one rectangle is above other 
    if (l1.y < r2.y || l2.y < r1.y) 
        return false; 
  
    return true; 
} 
```
[Knapsack Problem](https://www.geeksforgeeks.org/0-1-knapsack-problem-dp-10/)
```c++
// Returns the maximum value that can be put in a knapsack of capacity W 
int knapSack(int W, int wt[], int val[], int n) 
{ 
   int i, w; 
   int K[n+1][W+1]; 
  
   // Build table K[][] in bottom up manner 
   for (i = 0; i <= n; i++) 
   { 
       for (w = 0; w <= W; w++) 
       { 
           if (i==0 || w==0) 
               K[i][w] = 0; 
           else if (wt[i-1] <= w) 
                 K[i][w] = max(val[i-1] + K[i-1][w-wt[i-1]],  K[i-1][w]); 
           else
                 K[i][w] = K[i-1][w]; 
       } 
   } 
  
   return K[n][W]; 
} 
```
[LRU Cache](https://www.geeksforgeeks.org/lru-cache-implementation/)
https://hackernoon.com/14-patterns-to-ace-any-coding-interview-question-c5bb3357f6ed

[Median of Stream of Running Integers](https://www.geeksforgeeks.org/median-of-stream-of-running-integers-using-stl/)
```c++
// function to calculate med of stream 
void printMedians(double arr[], int n) 
{ 
    // max heap to store the smaller half elements 
    priority_queue<double> s; 
  
    // min heap to store the greater half elements 
    priority_queue<double,vector<double>,greater<double> > g; 
  
    double med = arr[0]; 
    s.push(arr[0]); 
  
    cout << med << endl; 
  
    // reading elements of stream one by one 
    /*  At any time we try to make heaps balanced and 
        their sizes differ by at-most 1. If heaps are 
        balanced,then we declare median as average of 
        min_heap_right.top() and max_heap_left.top() 
        If heaps are unbalanced,then median is defined 
        as the top element of heap of larger size  */
    for (int i=1; i < n; i++) 
    { 
        double x = arr[i]; 
  
        // case1(left side heap has more elements) 
        if (s.size() > g.size()) 
        { 
            if (x < med) 
            { 
                g.push(s.top()); 
                s.pop(); 
                s.push(x); 
            } 
            else
                g.push(x); 
  
            med = (s.top() + g.top())/2.0; 
        } 
  
        // case2(both heaps are balanced) 
        else if (s.size()==g.size()) 
        { 
            if (x < med) 
            { 
                s.push(x); 
                med = (double)s.top(); 
            } 
            else
            { 
                g.push(x); 
                med = (double)g.top(); 
            } 
        } 
  
        // case3(right side heap has more elements) 
        else
        { 
            if (x > med) 
            { 
                s.push(g.top()); 
                g.pop(); 
                g.push(x); 
            } 
            else
                s.push(x); 
  
            med = (s.top() + g.top())/2.0; 
        } 
  
        cout << med << endl; 
    } 
} 
```
[Cur Off Trees Golf Event](https://leetcode.com/problems/cut-off-trees-for-golf-event/discuss/107403/C%2B%2B-Sort-%2B-BFS-with-explanation)
```c++
// 1) Sort tree positions based on tree height; 
// 2) BFS to find shortest path between two points; 
public:
    int cutOffTree(vector<vector<int>>& forest) {
        if (forest.empty() || forest[0].empty()) return 0;
        int m = forest.size(), n = forest[0].size();
        vector<vector<int>> trees;
        // get all the tree positions and sort based on height
        // trees[i][0] is height. The default comparison of vector compare first element before other elements.
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (forest[i][j] > 1) trees.push_back({forest[i][j], i, j});
            }
        }
        sort(trees.begin(), trees.end());
        int ans = 0;
        // accumulate all the paths
        for (int i = 0, cur_row = 0, cur_col = 0; i < trees.size(); i++) {
            int step = next_step(forest, cur_row, cur_col, trees[i][1], trees[i][2]);
            // if next tree cannot be reached, step = -1;
            if (step == -1) return -1;
            ans += step;
            cur_row = trees[i][1];
            cur_col = trees[i][2];
        }
        return ans;
    }
private:
    // BFS to find shortest path to next tree; if cannot reach next tree, return -1
    int next_step(vector<vector<int>>& forest, int sr, int sc, int er, int ec) {
        if (sr == er && sc == ec) return 0;
        int m = forest.size(), n = forest[0].size();
        queue<pair<int, int>> myq;
        myq.push({sr, sc}); 
        vector<vector<int>> visited(m, vector<int>(n, 0));
        visited[sr][sc] = 1;
        int step = 0;
        vector<int> dir = {-1, 0, 1, 0, -1};
        while (!myq.empty()) {
            step++;
            int sz = myq.size();
            for (int i = 0; i < sz; i++) {
                int row = myq.front().first, col = myq.front().second;
                myq.pop();
                for (int i = 0; i < 4; i++) {
                    int r = row + dir[i], c = col + dir[i+1];
                    if (r < 0 || r >= m || c < 0 || c >= n || visited[r][c] == 1 || forest[r][c] == 0) continue;
                    if (r == er && c == ec) return step;
                    visited[r][c] = 1;
                    myq.push({r, c});
                }
            }
        }
        return -1;
    }
```
[Top K Frequent Words ](https://www.youtube.com/watch?v=POERw4yDVBw)
```c++
class Solution {
    // O(n log k) time and O(n) extra space
private:
    typedef pair<string, int> Node;
    typedef function<bool(const Node&, const Node&)> Compare;
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string, int> count;
        for (const string& word : words)
            ++count[word];
        
        Compare comparator = [](const Node& a, const Node& b) {
            // order by alphabet ASC
            if (a.second == b.second) 
                return  a.first < b.first;
            // order by freq DESC
            return a.second > b.second;
        };
        
        // Min heap by frequency
        priority_queue<Node, vector<Node>, Compare> q(comparator);
        
        // O(n*logk)
        for (const auto& kv : count) {
            q.push(kv);
            if (q.size() > k) q.pop();
        }
        
        vector<string> ans;
        
        while (!q.empty()) {
            ans.push_back(q.top().first);
            q.pop();
        }
        
        std::reverse(ans.begin(), ans.end());
        return ans;
    }
};
```
[Top K Frequent Elements](https://www.youtube.com/watch?v=lm6pBga98-w)
```c++
// Time complexity: O(n) + O(nlogk)
// Space complexity: O(n)
//  min heap
vector<int> topKFrequent(vector<int>& nums, int k) {
    unordered_map<int, int> count;        
    for (const int num : nums)
      ++count[num];
    priority_queue<pair<int, int>> q;
    for (const auto& pair : count) {
      q.emplace(-pair.second, pair.first);
      if (q.size() > k) q.pop();
    }
    vector<int> ans;
    for (int i = 0; i < k; ++i) {
      ans.push_back(q.top().second);
      q.pop();
    }
    return ans;
  }
  // bucket sort time and space complexity both O(n) 
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> count;
        int max_freq = 1;
        for (const int num : nums)
            max_freq = max(max_freq, ++count[num]);
        map<int, vector<int>> buckets;
        for (const auto& kv : count)
            buckets[kv.second].push_back(kv.first);
        vector<int> ans;
        for (int i = max_freq; i >= 1; --i) {
            auto it = buckets.find(i);
            if (it == buckets.end()) continue;
            ans.insert(ans.end(), it->second.begin(), it->second.end());
            if (ans.size() == k) return ans;
        }
        return ans;
    }
```
[Custom Sort String](https://www.cnblogs.com/grandyang/p/9190143.html)
```c++
    string customSortString(string S, string T) {
        string res = "";
        unordered_map<char, int> m;
        for (char c : T) ++m[c];
        for (char c : S) {
            res += string(m[c], c);
            m[c] = 0;
        }
        for (auto a : m) {
            res += string(a.second, a.first);
        }
        return res;
    }
```
[Update Matrix](https://www.cnblogs.com/seagoo/p/6759993.html)
```c++
    vector<vector<int>> updateMatrix(vector<vector<int>>& matrix) {
        queue<vector<int>> queue;
        for(int i = 0; i < matrix.size(); ++i) {
            for(int j = 0; j < matrix[0].size(); ++j) {
                if(matrix[i][j] == 0) {
                    queue.push({i ,j});
                } else {
                    matrix[i][j] = INT_MAX;
                }
            }
        }
        vector<vector<int>> dirs = {{0, -1},{-1, 0},{0, 1},{1, 0}};
        while(!queue.empty()) {
            vector<int> cell = queue.front();
            queue.pop();
            for(int i = 0; i < dirs.size(); ++i) {
                int r = cell[0] + dirs[i][0];
                int c = cell[1] + dirs[i][1];
                if(r >= 0 && r < matrix.size() && c >= 0 && c < matrix[0].size() && matrix[cell[0]][cell[1]] + 1 < matrix[r][c]) {
                    matrix[r][c] = matrix[cell[0]][cell[1]] + 1;
                    queue.push({r, c});
                }
            }
        }
        return matrix;
    }
```
[About Priority Queue in c++](https://blog.csdn.net/AAMahone/article/details/82787184)
```c++
// `
struct node
{
    int x, y;
    node(int x,int y):x(x),y(y){}
};
 
struct cmp
{
    bool operator()(const node & a,const node & b) const
    {
        if(a.x == b.x)  return a.y >= b.y;
        else return a.x > b.x;
    }
};
priority_queue<node,vector<node>,cmp> pq;    //带有三个参数的优先队列;

// 2
struct node
{
      int x, y;
      node(int x, int y):x(x),y(y){}
      bool operator< (const node &b) const   //写在里面只用一个b，但是要用const和&修饰，并且外面还要const修饰;
      {
           if(x == b.x)  return y >= b.y;
           else return x > b.x;
      }
};
priority_queue<node> pq;
```
[Find Friend Circle](https://blog.csdn.net/edc599/article/details/79623898)
```c++
    int findCircleNum(vector<vector<int>>& M) {
        if(M.size() == 0 || M[0].size() == 0) {
            return 0;
        }
        
        int count = 0;
        int m = M.size();
        vector<bool> visited(m, false);
        for(int i = 0; i < m; ++i) {
            if(!visited[i]) {
                find(i, m, visited, M);
                ++count;
            }
        }
        return count;
    }
    void find(int i, int m, vector<bool> & visited, vector<vector<int>>& M) {
        if(visited[i]) {
            return;
        }
        visited[i] = true;
        
        for(int j = 0; j < m; ++j) {
            if(j == i) {
                continue;
            }
            if(M[i][j] == 1) {
                find(j, m, visited, M);
            }
        }
    } 
```
[Rotate String](https://www.cnblogs.com/grandyang/p/9251578.html)
```c++
    bool rotateString(string A, string B) {
        return A.size() == B.size() && (A + A).find(B) != string::npos;
    }
    // see if a string contains another one
    if (s1.find(s2) != std::string::npos) {
        std::cout << "found!" << '\n';
    }
```
[Max Width of a binary tree](https://leetcode.com/problems/maximum-width-of-binary-tree/discuss/106645/C%2B%2BJava-*-BFSDFS3liner-Clean-Code-With-Explanation)
```c++
// idea of indexing to calculate
    int widthOfBinaryTree(TreeNode* root) {
        if (!root) return 0;
        int max = 0;
        queue<pair<TreeNode*, int>> q;
        q.push(pair<TreeNode*, int>(root, 1)); // 1 represents its pos in an array
        while (!q.empty()) {
            int l = q.front().second, r = l; // right started same as left
            for (int i = 0, n = q.size(); i < n; i++) {
                TreeNode* node = q.front().first;
                r = q.front().second;
                q.pop();
                if (node->left) q.push(pair<TreeNode*, int>(node->left, r * 2));
                if (node->right) q.push(pair<TreeNode*, int>(node->right, r * 2 + 1));
            }
            max = std::max(max, r + 1 - l);
        }
        return max;
    }
```
[Sliding Window for K elements / Fruit into Baskets](https://leetcode.com/problems/fruit-into-baskets/discuss/170740/Sliding-Window-for-K-Elements)
```c++
// Find out the longest length of subarrays with at most 2 different numbers?
    int totalFruit(vector<int> &tree) {
        unordered_map<int, int> count;
        int i, j;
        for (i = 0, j = 0; j < tree.size(); ++j) {
            count[tree[j]]++;
            if (count.size() > 2) { // 2 could be k here
                if (--count[tree[i]] == 0)count.erase(tree[i]);
                i++;
            }
        }
        return j - i;
    }
    // two pointer approach
    // https://leetcode.com/problems/fruit-into-baskets/discuss/170745/Problem%3A-Longest-Subarray-With-2-Elements
    int totalFruit(vector<int> tree) {
        int res = 0, cur = 0, count_b = 0, a = 0, b = 0;
        for (int c :  tree) {
            cur = c == a || c == b ? cur + 1 : count_b + 1;
            count_b = c == b ? count_b + 1 : 1;
            if (b != c) a = b, b = c;
            res = max(res, cur);
        }
        return res;
    }
```

[Longest Common Subsequence](https://www.geeksforgeeks.org/longest-common-subsequence-dp-4/)
https://www.youtube.com/watch?v=NnD96abizww
```c++
/* Returns length of LCS for X[0..m-1], Y[0..n-1] */
int lcs( char *X, char *Y, int m, int n ) 
{ 
   int L[m+1][n+1]; 
   int i, j; 
   
   /* Following steps build L[m+1][n+1] in bottom up fashion. Note  
      that L[i][j] contains length of LCS of X[0..i-1] and Y[0..j-1] */
   for (i=0; i<=m; i++) 
   { 
     for (j=0; j<=n; j++) 
     { 
       if (i == 0 || j == 0) 
         L[i][j] = 0; 
   
       else if (X[i-1] == Y[j-1]) 
         L[i][j] = L[i-1][j-1] + 1; 
   
       else
         L[i][j] = max(L[i-1][j], L[i][j-1]); 
     } 
   } 
     
   /* L[m][n] contains length of LCS for X[0..n-1] and Y[0..m-1] */
   return L[m][n]; 
} 
```

[Longest Common Substring](https://www.geeksforgeeks.org/longest-common-substring-dp-29/)
```c++
/* Returns length of longest common substring of X[0..m-1]  
   and Y[0..n-1] */
int LCSubStr(char *X, char *Y, int m, int n) 
{ 
    // Create a table to store lengths of longest 
    // common suffixes of substrings.   Note that 
    // LCSuff[i][j] contains length of longest 
    // common suffix of X[0..i-1] and Y[0..j-1].  
  
    int LCSuff[m+1][n+1] = {0}; 
    int result = 0;  // To store length of the  
                     // longest common substring 
  
    /* Following steps build LCSuff[m+1][n+1] in 
        bottom up fashion. */
    for (int i=1; i<=m; i++) 
    { 
        for (int j=1; j<=n; j++) 
        { 
            // The first row and first column  
            // entries have no logical meaning,  
            // they are used only for simplicity  
            // of program 
            if (X[i-1] == Y[j-1]) 
            { 
                LCSuff[i][j] = LCSuff[i-1][j-1] + 1; 
                result = max(result, LCSuff[i][j]); 
            } 
            else LCSuff[i][j] = 0; 
        } 
    } 
    return result; 
} 
```

[Coin Change](https://www.lintcode.com/problem/coin-change/description)
```c++
    int coinChange(vector<int> &coins, int amount) {
        // dp
        // it's the FEWEST number of coins
        vector<int> canChange(amount + 1, INT_MAX);
        canChange[0] = 0;
        for(int i = 1; i <= amount; ++i) {
            for(int value: coins) {
                if(value <= i && canChange[i - value] != INT_MAX) {
                    canChange[i] = min(canChange[i], canChange[i - value] + 1);
                }
            }
        }
        return canChange[amount] == INT_MAX ? -1 : canChange[amount];
    }
```

[Merge Interval](https://www.lintcode.com/problem/merge-intervals/description)
```c++
bool cmp(const Interval & A, const Interval & B) {
    return A.start < B.start;
}

class Solution {
public:
    /**
     * @param intervals: interval list.
     * @return: A new interval list.
     */
    vector<Interval> merge(vector<Interval> &intervals) {
        vector<Interval> results;
        
        sort(intervals.begin(), intervals.end(), cmp);
        
        Interval last = Interval(-1, -1);
        for(Interval item: intervals) {
            if(last.end == -1) {
                last = item;
            } else if(last.end < item.start) {
                results.push_back(last);
                last = item;
            } else {
                // last.end > item.start
                last.end = max(last.end, item.end); // shallow copy
            }
        }
        if(last.end != -1) {
            results.push_back(last);
        }
        
        return results;
    }
};
```


[Kruskal's MST](https://www.geeksforgeeks.org/kruskals-minimum-spanning-tree-algorithm-greedy-algo-2/)
[Union Find](https://www.youtube.com/watch?v=7gD5liN3HCw)

[Length of Longest Substring](https://www.cnblogs.com/ariel-dreamland/p/8668286.html)
https://www.geeksforgeeks.org/length-of-the-longest-substring-without-repeating-characters/
```c++
    int lengthOfLongestSubstring(string s) {
        int m[256] = { 0 }, res = 0, left = 0;
        for (int i = 0; i < s.size(); ++i) {
            if (m[s[i]] == 0 || m[s[i]] <= left) {
                res = max(res, i - left + 1);
            } else {
                left = m[s[i]];
            }
            m[s[i]] = i + 1;
        }
        return res;
    }
```
[Longest Substring with At Most K Distinct Characters](https://www.lintcode.com/problem/longest-substring-with-at-most-k-distinct-characters/description)
```c++
    int lengthOfLongestSubstringKDistinct(string &s, int k) {
        int start = 0, maxLen = 0;
        unordered_map<char, int> charCount;
        for(int i = 0; i < s.length(); ++i) {
            ++charCount[s[i]];
            while (charCount.size() > k) {
                if(--charCount[s[start]] == 0) {
                    charCount.erase(s[start]);
                }
                ++start;
            }
            maxLen = max(maxLen, i - start + 1);
        }
        return maxLen;
    }
```

[Invert Tree Node](https://leetcode.com/problems/invert-binary-tree/submissions/)
```c++
        TreeNode* invertTree(TreeNode* root) {
//         if(!root || !root->left && !root->right) {
//             return root;
//         }
//         swap(root->left, root->right); // can also use post order or in order
        
//         invertTree(root->right);
//         invertTree(root->left);
//         return root;
        queue<TreeNode* > myStack; // stack is the same
        myStack.push(root);
        while(!myStack.empty()) {
            TreeNode * cur = myStack.front();
            myStack.pop();
            if(!cur) {
                continue;
            }
            swap(cur->left, cur->right);
            myStack.push(cur->left);
            myStack.push(cur->right);   
        }
        return root;
        
    }
```

```java
class SpiralIdeonePrint {
    int [] isPrime;
    int primeSize = 0;

    static void spiralOrderPrimes(int a[][]) 
    { 
        int i, k = 0, l = 0;
        
        int m = a.length - 1;
        int n = a[0].length - 1;
        while(k <= m && l <= n) {
            for(i = l; i <= n; ++i) {
                System.out.print(a[k][i] + " ");
            }
            ++k;
            for(i = k; i <= m; ++i) {
                System.out.print(a[i][n] + " ");
            }
            --n;
            if(k <= m){
                for(i = n; i >= l; --i) {
                    System.out.print(a[m][i] + " ");
                }
                --m;
            }
            if(l <= n){
                for(i = m; i >= k; --i) {
                    System.out.print(a[i][l] + " ");
                }
                ++l;
            }
        }
    }
```

[Pythagorean Triplet in an array](https://www.geeksforgeeks.org/find-pythagorean-triplet-in-an-unsorted-array/)
- could square and sort and then apply two sum 



[Subarray Sum Equals K](https://www.lintcode.com/problem/subarray-sum-equals-k/description?_from=ladder&&fromId=96)
```c++
    int subarraySumEqualsK(vector<int> &nums, int k) {
        unordered_map<int, int> preSum;
        preSum[0] = 1;
        int sum = 0, res = 0;
        for(int num: nums) {
            sum += num;
            res += preSum.count(sum - k);
            ++preSum[sum];
        }
        return res;
    }
```

[Find duplicates in O(n) time and O(1) extra space](https://www.geeksforgeeks.org/find-duplicates-in-on-time-and-constant-extra-space/)

[Sorted Array to Balanced BST]()
```c++
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return helper(nums, 0 , (int)nums.size() - 1);
    }
    TreeNode* helper(vector<int>& nums, int left, int right) {
        if (left > right) return NULL;
        int mid = left + (right - left) / 2;
        TreeNode *cur = new TreeNode(nums[mid]);
        cur->left = helper(nums, left, mid - 1);
        cur->right = helper(nums, mid + 1, right);
        return cur;
    }
```
[Sorted Linked List to Balanced BST](https://www.geeksforgeeks.org/sorted-linked-list-to-balanced-bst/)
```c++
     TreeNode* sortedListToBST(ListNode* head) {
        if (!head) return NULL;
        return helper(head, NULL);
    }
    TreeNode* helper(ListNode* head, ListNode* tail) {
        if (head == tail) return NULL;
        ListNode *slow = head, *fast = head;
        while (fast != tail && fast->next != tail) {
            slow = slow->next;
            fast = fast->next->next;
        }
        TreeNode *cur = new TreeNode(slow->val);
        cur->left = helper(head, slow);
        cur->right = helper(slow->next, tail);
        return cur;
    }
```
[Sort List](https://www.lintcode.com/problem/sort-list/description)
```c++
    ListNode* sortList(ListNode* head) {
        if(!head || !head->next) {
            return head;
        }
        ListNode * slow = head, * fast = head->next;
        while(fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        ListNode * headRight = slow->next;
        slow->next = nullptr;
        ListNode * left = sortList(head);
        ListNode * right = sortList(headRight);
        return merge(left, right);
    }
    
    ListNode* merge(ListNode* l1, ListNode* l2) {
        ListNode *dummy = new ListNode(-1);
        ListNode *cur = dummy;
        while (l1 && l2) {
            if (l1->val < l2->val) {
                cur->next = l1;
                l1 = l1->next;
            } else {
                cur->next = l2;
                l2 = l2->next;
            }
            cur = cur->next;
        }
        if (l1){
            cur->next = l1;
        } 
        if (l2) {
            cur->next = l2;
        }
        return dummy->next;
    }
```

[Find the smallest positive number missing from an unsorted array](https://www.geeksforgeeks.org/find-the-smallest-positive-number-missing-from-an-unsorted-array/)
    - To mark presence of an element x, we change the value at the index x to negative

[Binary Tree Maximum Path Sum](https://www.lintcode.com/problem/binary-tree-maximum-path-sum/description)
```c++
class Solution {
private:
    class ResultType {
    public:
        int singlePath, maxPath;
        ResultType(int singlePath, int maxPath): singlePath(singlePath), maxPath(maxPath) {}
    };
public:
    /**
     * @param root: The root of binary tree.
     * @return: An integer
     */
    int maxPathSum(TreeNode * root) {
        ResultType result = helper(root);
        return result.maxPath;
    }
    
    ResultType helper(TreeNode * curRoot) {
        if(!curRoot) {
            return ResultType(0, INT_MIN); // have to be INT_MIN or {-1} would return 0 which is not we want 
        }
        // Divide
        ResultType left = helper(curRoot->left);
        ResultType right = helper(curRoot->right);

        // Conquer
        int singlePath = max(left.singlePath, right.singlePath) + curRoot->val;
        singlePath = max(singlePath, 0);

        int maxPath = max(left.maxPath, right.maxPath);
        maxPath = max(maxPath, left.singlePath + right.singlePath + curRoot->val);

        return ResultType(singlePath, maxPath);
    }
};
```
[Remove Invalid Parentheses](https://www.youtube.com/watch?v=EE_9U798nvQ)
```c++
    // could use bfs to solve this
    unordered_set<string> visited;
    vector<string> removeInvalidParentheses(string s) {
        if(check(s)) {
            return {s};
        }
        vector<string> res;
        queue<string> queue;
        queue.push(s);
        visited.insert(s);
        
        bool found = false;
        while(!queue.empty() && !found) {
            int size = queue.size();
            while(--size >= 0){
                string cur = queue.front(); queue.pop();
                for(int i = 0; i < cur.length(); ++i) {
                    if(cur[i] != '(' && cur[i] != ')') {
                        continue;
                    }
                    string str = cur.substr(0, i) + cur.substr(i + 1);
                    if(!visited.count(str)) {
                        if(check(str)) {
                            res.push_back(str);
                            found = true;
                        }
                        queue.push(str);
                        visited.insert(str);
                    }
                }
            }
        }
        return res;
    }
    
    bool check(string str) {
        int cnt = 0;
        for(int i = 0; i < str.length(); ++i) {
            if(str[i] == '(') {
                ++cnt;
            } else if(str[i] == ')') {
                if(cnt-- == 0) {
                    return false;
                }
            }   
        }
        return cnt == 0;
    }
```

[Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/submissions/)
```c++
    int trap(vector<int>& heights) {
        if(heights.size() == 0) {
            return 0;
        }
        // find heightest

        int idx = findMax(heights), count = 0;
        // search from left
        int prevHighBar = 0, localCount = 0;
        
        for(int i = 0; i <= idx; ++i) {
            int height = heights[i];
            if(height >= prevHighBar) {
                count += localCount;
                localCount = 0;
                prevHighBar = height;
            } else if(height < prevHighBar) {
                localCount += prevHighBar - height;
            } 
        }
        
        prevHighBar = 0, localCount = 0;
        for(int i = heights.size() - 1; i >= idx; --i) {
            int height = heights[i];
            if(height >= prevHighBar) { // equal here is necessary
                count += localCount;
                localCount = 0;
                prevHighBar = height;
            } else if(height < prevHighBar) {
                localCount += prevHighBar - height;
            } 
        }
 
        return count;
    }
```
[next permutation](https://leetcode.com/problems/next-permutation/discuss/13867/C%2B%2B-from-Wikipedia)