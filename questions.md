[Count Number of Unival Subtrees](https://www.geeksforgeeks.org/find-count-of-singly-subtrees/)
```c++
// This function increments count by number of single  
// valued subtrees under root. It returns true if subtree  
// under root is Singly, else false. 
bool countSingleRec(Node* root, int &count) 
{ 
    // Return false to indicate NULL 
    if (root == NULL) 
       return true; 
  
    // Recursively count in left and right subtrees also 
    bool left = countSingleRec(root->left, count); 
    bool right = countSingleRec(root->right, count); 
  
    // If any of the subtrees is not singly, then this 
    // cannot be singly. 
    if (left == false || right == false) 
       return false; 
  
    // If left subtree is singly and non-empty, but data 
    // doesn't match 
    if (root->left && root->data != root->left->data) 
        return false; 
  
    // Same for right subtree 
    if (root->right && root->data != root->right->data) 
        return false; 
  
    // If none of the above conditions is true, then 
    // tree rooted under root is single valued, increment 
    // count and return true. 
    count++; 
    return true; 
} 
```

[Largest BST Subtree](https://www.lintcode.com/problem/largest-bst-subtree/description?_from=ladder&&fromId=104)
```c++
class Result {
public:
    int size;
    int lower;
    int upper;
    Result(int size, int lower, int upper):size(size), lower(lower), upper(upper) {}
};

class Solution {
public:
    int ans = 0;
    int largestBSTSubtree(TreeNode * root) {
        if(!root) {
            return 0;
        }
        traverse(root);
        return ans;
    }
    
    Result traverse(TreeNode * root) {
        if(!root) {
            return Result(0, INT_MAX, INT_MIN);
        }
        
        Result left = traverse(root->left);
        Result right = traverse(root->right);
        
        if(left.size == -1 || right.size == -1 || root->val <= left.upper || root->val >= right.lower) {
            return Result(-1, 0, 0);
        }
        int size = left.size + right.size + 1;
        ans = max(ans, size);
        return Result(size, min(left.lower, root->val), max(right.upper, root->val));
    }
};
```

[Binary Tree Maximum Path Sum](https://www.lintcode.com/problem/binary-tree-maximum-path-sum/description?_from=ladder&&fromId=21)
```c++
/**
 * Definition of TreeNode:
 * class TreeNode {
 * public:
 *     int val;
 *     TreeNode *left, *right;
 *     TreeNode(int val) {
 *         this->val = val;
 *         this->left = this->right = NULL;
 *     }
 * }
 */

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
        
        ResultType left = helper(curRoot->left);
        ResultType right = helper(curRoot->right);
        
        int singlePath = max(left.singlePath, right.singlePath) + curRoot->val; // put curRoor->val outside and compare it to 0
        singlePath = max(0, singlePath); 
        
        int maxPath = max(left.maxPath, right.maxPath);
        maxPath = max(maxPath, left.singlePath + right.singlePath + curRoot->val);
        return ResultType(singlePath, maxPath);
    }
};
```

[Minimum Swaps required to group all 1’s together](https://www.geeksforgeeks.org/minimum-swaps-required-group-1s-together/)
```c++
// Function to find minimum swaps 
// to group all 1's together 
int minSwaps(int arr[], int n) 
{ 
  
int numberOfOnes = 0; 
  
// find total number of all 1's in the array 
for (int i = 0; i < n; i++) { 
    if (arr[i] == 1) 
    numberOfOnes++; 
} 
  
// length of subarray to check for 
int x = numberOfOnes; 
  
int count_ones = 0, maxOnes; 
  
// Find 1's for first subarray of length x 
for(int i = 0; i < x; i++){ 
    if(arr[i] == 1) 
    count_ones++; 
} 
      
maxOnes = count_ones; 
      
// using sliding window technique to find 
// max number of ones in subarray of length x 
for (int i = 1; i <= n-x; i++) { 
      
    // first remove leading element and check 
    // if it is equal to 1 then decreament the  
    // value of count_ones by 1 
    if (arr[i-1] == 1)  
    count_ones--; 
      
    // Now add trailing element and check 
    // if it is equal to 1 Then increament  
    // the value of count_ones by 1 
    if(arr[i+x-1] == 1) 
    count_ones++; 
      
    if (maxOnes < count_ones) 
    maxOnes = count_ones; 
} 
  
// calculate number of zeros in subarray 
// of length x with maximum number of 1's 
int numberOfZeroes = x - maxOnes; 
  
return numberOfZeroes; 
} 
```

[Regular Expression Mapping](https://leetcode.com/problems/regular-expression-matching/discuss/5651/Easy-DP-Java-Solution-with-detailed-Explanation)

[Rotate Matrix](https://www.lintcode.com/problem/rotate-image/note/193237)
```c++
    void rotate(vector<vector<int>> &matrix) {
        int n = matrix.size();
        if(n == 0) {
            return;
        }
        // first think hard before implementation
        for(int i = 0; i < n; ++i) { //!!! important to not rotate twice!!!
            for(int j = 0; j < i; ++j) {
                swap(matrix[i][j], matrix[j][i]);
            }
        }
        
        for(int i = 0; i < n; ++i) {
            // rotate each row
            for(int j = 0; j < n / 2; ++j) {
                swap(matrix[i][j], matrix[i][n - 1- j]);
            }
        }
    }
```

[Remove K digits](https://www.lintcode.com/problem/remove-k-digits/description?_from=ladder&&fromId=88)

[Gas Station](https://www.lintcode.com/problem/gas-station/note/192467)
Obverse before brute force coding 

[shortest unsorted continuous subarray](https://leetcode.com/problems/shortest-unsorted-continuous-subarray/solution/)
[](https://leetcode.com/problems/shortest-unsorted-continuous-subarray/discuss/103066/Ideas-behind-the-O(n)-two-pass-and-one-pass-solutions)

[Construct a Binary Search Tree from given postorder]()

[]()
[2 Keys Keyboard](https://leetcode.com/problems/2-keys-keyboard/submissions/)

[Random pick w/ weight](https://leetcode.com/problems/random-pick-with-weight/)
```c++
class Solution {
public:
    vector<int> wSums;
    
    Solution(vector<int> w) {
        for(int i=1; i<w.size(); ++i)
            w[i] += w[i-1];
        wSums = w;
    }
    
    int pickIndex() {
        int len = wSums.size();
        int idx = rand() % (wSums[len-1]) + 1;
        int left = 0, right = len - 1;
        // search position 
        while(left + 1 < right){
            int mid = left + (right - left)/2;
            if(wSums[mid] == idx)
                return mid;
            else if(wSums[mid] < idx)
                left = mid;
            else
                right = mid;
        }
        if (wSums[left] >= idx) {
            return left;
        } else {
            return right;
        }
    }
};

```

[Meeting Room schedule](cnblogs.com/grandyang/p/5244720.html)
```c++
    int minMeetingRooms(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [](const vector<int>& a, const vector<int>& b){ return a[0] < b[0]; });
        priority_queue<int, vector<int>, greater<int>> q;
        for (auto interval : intervals) {
            if (!q.empty() && q.top() <= interval[0]) q.pop();
            q.push(interval[1]);
        }
        return q.size();
    }

    // another method
    // add all starts and ends to two separate arrays and count (available rooms)
```

[Find Triplet](https://www.geeksforgeeks.org/find-a-sorted-subsequence-of-size-3-in-linear-time/)
<!-- https://leetcode.com/problems/increasing-triplet-subsequence/ -->

[maximum sum Circular subarray](https://www.techiedelight.com/maximum-sum-circular-subarray/)

[Convert BST to sorted doubly linked list](https://www.lintcode.com/problem/convert-binary-search-tree-to-sorted-doubly-linked-list/description)

[Google Interview Problems: Ratio finder](https://medium.com/@alexgolec/google-interview-problems-ratio-finder-d7aa8bf201e3)
[Google Interview Problems: Knights Dialer](https://medium.com/hackernoon/google-interview-questions-deconstructed-the-knights-dialer-f780d516f029)

#### System Design
- approach
    - **Scope** it well
    - ask **clarification**
    - **outline** use case
    - create high level design (e.g. queue mechanism, web server) first
    - consider scalability later

- Load Balancing (considered to be added to any system)
    - balanced the load and spreads the traffic
    - a single system that receives request and routes to other machines
        - much less computation power than main application
        - choose one of multiple endpoints
        - may have caching layer
    - approach: send to the least loaded
    - concern: single point of failure 
    - DNS - a special load balancer map from domain name to IP address
    - scale horizontally - introduce complexity
    - Similar/Alternative: use multiple micro-services

- Caching
    - key-value store (dict)
    - before load balancing
    - a server (not a cache on client side)
    - take load off database
    - risk: not persistent (disk - persistent)
    - rewrite and update database
    - keep updating (lots of writes - handy)
    - tmp data store (can lose)
    - isn't right information anymore
    - keep cache coherent
    - validation on the cache
        - alg: write-thru cache, change data on cache first and then database
            - write slow
        - change cache and then at a time all push to database
            - if cache dies
        - keep limit, key-value
            - LRU, FIFOs
    - maincache in memory - fast
    - tradeoff
        - update caching (old data)
    - validation (full)
    - scale horizontally vs vertically
    - remove dependency
    - dependency injection

- CDN (Content Delivery Network)
    - choose resources based on server in different locations
    - need static files
    - pull model
        - slow first rendering in that location but fast afterwards
    - push model
        - even fast in first rendering

- SQL NoSQL
    - ACID (Atomicity, Consistency, Isolation, Durability)
        - 'transaction' such as bank transactions from one account to another
    - CAP theorem - consistency and availability 
    - SQL 
        - structured scheme
        - being transactional
        - range queries
        - bank *transactions* (one account from another account)
            - not as fast
    - NoSQL
        - based on key value (document model like MongoDBs)
        - not persistent (delayed)
        - easily scale up (memory based)
        - flexibility
    - graph data base (linked linkedin connections)
    - consideration: 
        - schema consistent / changing 
        - how to store
        - ranging
        - scalability 
        - how reliable
        - (designed based on clarification question)
    - consistency (write and read it the same value) / (SQL)
        - wait consistency
        - eventual consistency
        - strong consistency
    - availability 

- API Design
    - interface web server and front-end
    - clear and concise about end-points
    - `get` and `post`, query parameter, api key for authentication, or other filter arguments 
    - *pagination* of posts, solved by cursor (id of last item)
    - abstract, multiple requests instead of a heavy request, allow people to work thru interfaces
    - important: scale 
    - json err payload

- algorithms
    - stack & queue(use linked list cause we want to enqueue O(1))

- **Bit manipulation**
    - operations
        - AND `&` OR `|` 
        - XOR `^`(1 when values are different) 
        - RSHIFT `>>` LSHIFT `<<`
    - example: 1101 (13) -> toggle the third one
        - shift right a single number twice to 100 and XOR the numbers
        - become 1001
    - *toggle* the second bits 000 `^` 010 = 010, 010 `^` 010 =  000 (scenario: lights on)
        + use `&` or `|` check if it is on or off
    - count one / parity count
    ```c++
        int num_bits = 0;
        while(n) {
            num_bits += n & 1;
            n >> 1; // shift right discard the rightmost bit
        }

       // or 
       while(n) {
            n = n&(n-1); 
            // n&(n-1) let n's binary representation's right most 1 become 0
            num_bits++;
        } 

        n > 0 && ((n & (n - 1)) == 0 ) // judge if a representation is only has one 1 or it can be represented by 2^n
    ```
    - find a single one in array (all oth elements appears twice)
    ```c++
    int result = 0;
    for(int n : nums) {
        result ^= n;
    }
    // again XOR, toggle
    return result;
    ```
    ```c++
    // num2Bits
    result = ""
    while(n) {
        result += to_string(n&1) + result;
        result >> 1;
    }

    // bits2Num
    for(char bit: bits) {
        n << 1; // n *= 2
        if(bit == '1') {
            n += 1; // 
        }
    }
    ```

- binary search in an array comes from binary search tree
- build heap and get top k O(n)(build) + O(klgn)(get top k)

generate ()
valid () 

