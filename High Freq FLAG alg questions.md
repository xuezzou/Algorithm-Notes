#### Lesson 1: Interview Style of Facebook/Google etc.
- [Log Sorting]()
    - Key: sort according to multiple situations
```c++
class Solution {
public:
    /**
     * @param logs: the logs
     * @return: the log after sorting
     */
    struct node{
        string id;
        string content;
        int index;
        int type;
        friend bool operator < (node a, node b) {
            if(a.type == 2 && b.type == 2) {
                return a.index < b.index;
            }
            if(a.type == 1 && b.type == 1) {
                if(a.content == b.content) {
                    return a.id < b.id;
                }
                return a.content < b.content;
            }
            return a.type < b.type;
        }
    };
    vector<string> logSort(vector<string> &logs) {
        // Write your code here
        vector<node> p;
        vector<string> ans;
        int n = logs.size();
        for(int i = 0; i < n; i++) {
            int j;
            for(j = 0; j < logs[i].length(); j++) {
                if(logs[i][j] == ' '){
                    break;
                }
            }
            node temp;
            temp.index = i;
            temp.id = logs[i].substr(0, j);
            temp.content = logs[i].substr(j + 1);
            if(temp.content[0] >= '0' && temp.content[0] <= '9')
                temp.type = 2;
            else
                temp.type = 1;
            p.push_back(temp);
        }
        sort(p.begin(), p.end());
        for(int i = 0; i < n; i++) {
            ans.push_back(p[i].id + ' ' + p[i].content);
        }
        return ans;
    }
};
```
- [Valid Word Abbreviation]()
```c++
    bool validWordAbbreviation(string &word, string &abbr) {
        int i = 0, j = 0;
        while(i < word.length() && j < abbr.length()) {
            if(abbr[j] == '0') {
                return false;
            }
            if(isdigit(abbr[j])) {
                int start = j;
                while(j < abbr.length() && isdigit(abbr[j])) {
                    ++j;
                }
                i += stoi(abbr.substr(start, j - start));
            } else {
                if(word[i] != abbr[j]) {
                    return false;
                }
                ++i; ++j;
            }
        }
        return i == word.length() && j == abbr.length();
    }
```
- [Merge Intervals]()
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
- [Decode Ways](https://www.lintcode.com/problem/decode-ways/?_from=ladder&&fromId=14)
```c++
    int numDecodings(string &s) {
        if(s.length() == 0) {
            return 0;
        }
        // dp
        vector<int> decodeWays(s.length() + 1, 0);
        decodeWays[0] = 1;
        for(int i = 1; i <= s.length(); ++i) {
            if(s[i - 1] != '0') {
                decodeWays[i] += decodeWays[i - 1];
            }
            if(i >= 2) {
                int val = stoi(s.substr(i - 2, 2));
                if(val >= 10 && val <= 26) {
                    decodeWays[i] += decodeWays[i - 2];
                }
            }
        }
        return decodeWays[s.length()];
    }
```
- [Maximum Subtree](https://www.lintcode.com/problem/maximum-subtree/description?_from=ladder&&fromId=14)
```c++
class Solution {
public:
    /**
     * @param root: the root of binary tree
     * @return: the maximum weight node
     */
    TreeNode* maxNode = nullptr;
    int maxSum = INT_MIN;
    TreeNode * findSubtree(TreeNode * root) {
        findSubtreeHelper(root);
        return maxNode;
    }
    int findSubtreeHelper(TreeNode * root) {
        if(root == nullptr) {
            return 0;
        }
        int curSum = findSubtreeHelper(root->left) + findSubtreeHelper(root->right) + root->val;
        if(curSum > maxSum) {
            maxSum = curSum;
            maxNode = root;
        }
        return curSum; // !!!
    } 
};
```
- [Window Sum](https://www.lintcode.com/problem/window-sum/description?_from=ladder&&fromId=14)
```c++
    vector<int> winSum(vector<int> &nums, int k) {
        int firstSum = 0;
        vector<int> results;
        if(nums.size() == 0) {
            return {};
        }
        for(int i = 0;  i < nums.size() && i < k; ++i) {
            firstSum += nums[i];
        }
        
        results.push_back(firstSum);
        for(int j = k; j < nums.size(); ++j) {
            firstSum -= nums[j - k];
            firstSum += nums[j];
            results.push_back(firstSum);
        }
        return results;
    }
```

Related
[Decrease to be Palindrome](https://www.lintcode.com/problem/decrease-to-be-palindrome/description?_from=ladder&&fromId=14)
```c++
    int numberOfOperations(string &s) {
        int left = 0, right = s.length() - 1;
        int res = 0;
        while(left < right) {
            res += abs(s[left++] - s[right--]);
        }
        return res;
    }
```
[Palindromic Substrings](https://www.lintcode.com/problem/palindromic-substrings/description?_from=ladder&&fromId=14)
```c++
    int countPalindromicSubstrings(string &str) {
        int n = str.size();
        int ans = 0;
        vector<vector<bool>> dp (n, vector<bool> (n, false));
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j <= i; ++j) {
                if(str[i] == str[j] && ( i - j <= 2 || dp[j + 1][i - 1])) {
                    dp[j][i] = true;
                    ans += 1;
                } 
            }
        }
        return ans;
    }
```

#### Lesson 2: Basic Data Structure & Algorithms high frequency problems
- [Valid Number](https://www.jiuzhang.com/solution/valid-number/#tag-highlight-lang-cpp)
```c++
    vector<string> findMissingRanges(vector<int> &nums, int lower, int upper) {
        vector<string> res;
        if(nums[0] > lower) {
            res.push_back(getRange(lower, nums[0] - 1));
        }
        
        for(int i = 1; i < nums.size(); i++) {
            long diff = (long)nums[i] - (long)nums[i-1];
            if (diff > 1L) {
                res.push_back(getRange(nums[i-1]+1, nums[i]-1));
            }
        }
        if(nums[nums.size() - 1] < upper) {
            res.push_back(getRange(nums[nums.size() - 1] + 1, upper));
        }
        return res;
    }
    void getRange(vector<string> & ans, int from, int to) {
        if (from > to) {
            return;
        }
        if (from == to) {
            ans.push_back(to_string(from));
        } else {
            ans.push_back(to_string(from) + "->" + to_string(to));
        }
    }
    // Key: corner case when the array is empty and handle case like [214783674] 0 214783674 about overflow
```
2. [Valid Number]()
```java
// 符号 + 浮点数 + e + 符号 + 整数
// 在末尾 + 一个空格 没有越界情况了 相当于dummy node
    public boolean isNumber(String s) {
        // write your code here
        int i = 0;
        s = s.trim() + " "; // why +" "?
        char[] sc = s.toCharArray();
        int len = s.length() - 1;

        if (sc[i] == '+' || sc[i] == '-') {
            i++;
        }
        int nDigit = 0, nPoint = 0;
        while (Character.isDigit(sc[i]) || sc[i] == '.') {
            if (Character.isDigit(sc[i])) {
                nDigit++;
            }
            if (sc[i] == '.') {
                nPoint++;
            }
            i++;
        }
        if (nDigit <= 0 || nPoint > 1) {
            return false;
        }

        if (sc[i] == 'e') {
            i++;
            if (sc[i] == '+' || sc[i] == '-') {
                i++;
            }
            if (i == len) {
                return false;
            }
            for (; i < len; i++) {
                if (!Character.isDigit(sc[i])) {
                    return false;
                }
            }
        }
        return i == len;
    }
```
3. [Integer to Roman]()
```java
// 数据分离 & 数据转换 一类问题
// 任何数都可以转成 1 2 4 8 16 32 的和

```
4. [Pow(x, n)]()
```c++
// when n is n is min_value
    while(n != 0) {
        if(n % 2 == 1) {
            ans *= temp;
        }
        temp = temp * temp;
        n /= 2;
    }
```
5. [Find Celebrity]()
```c++
```
6. [Sparse Matrix]()
```c++
// i j k 先循环 i k 再 j
// 针对 b 的情况先把握非0情况

```
开方 二分法