### Introduction
1. [Log Sorting](https://www.lintcode.com/problem/log-sorting/description?_from=ladder&&fromId=14)
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
2. [Valid Word Abbreviation](https://www.lintcode.com/problem/valid-word-abbreviation/description?_from=ladder&&fromId=14)
    - take care of edge cases such as empty
    - note the bound when we have a smaller while loop inside a bigger while loop
```c++
    bool validWordAbbreviation(string &word, string &abbr) {
        int i = 0, j = 0;
        while(i < word.length() && j < abbr.length()) {
            // can't start with 0
            if(abbr[j] == '0') {
                return false;
            }
            if(isdigit(abbr[j])) {
                int start = j;
                while(j < abbr.length() && isdigit(abbr[j])) { // important that j < abbr.length()
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
3. [Merge Intervals](https://www.lintcode.com/problem/merge-intervals/description?_from=ladder&&fromId=14)
    - O(nlgn) from sorting
    - similar question: insert interval, insert and merge
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
        for(Interval each: intervals) {
            if(last.start == -1) {
                last = each;
                continue;
            }
            if(last.end < each.start) {
                results.push_back(last);
                last = each;
            } else {
                last.end = max(last.end, each.end);
            }
        }
        if(last.start != -1) {
            results.push_back(last);
        }
        
        return results;
    }
};
```
4. [Decode Ways](https://www.lintcode.com/problem/decode-ways/?_from=ladder&&fromId=14)
    - could be solved by recursion but cost a lot, nearly 2^n (choose or not choose)
    - solved by dynamic programing like the dp problem of climbing stairs
        - note the initialization that `f[0] = 1`
        - `f[n] = f[n - 1]` if `s[n] != 0`
        - further `+ f[ n - 2]` if `s[n - 1: n ]` is in 10 - 26
        - O(n) complexity
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
#### **Related Problems**
- [Largest Number](https://www.lintcode.com/problem/largest-number/description?_from=ladder&&fromId=14)
```c++
bool cmp(string & A, string & B) {
    return A + B > B + A;
}
class Solution {
public:
    string largestNumber(vector<int> &nums) {
        vector<string> stringNums(nums.size());
        for(int i = 0; i < nums.size(); ++i) {
            stringNums[i] = to_string(nums[i]);
        }
        sort(stringNums.begin(), stringNums.end(), cmp);
        string res = "";
        for(string eachStr: stringNums) {
            res += eachStr;
        }
        int index = 0;
        while (index < res.length() && res[index] == '0') {
            index++;
        }
        if (index == res.length()) {
            return "0";
        }
        return res;
    }
};
```
- [Subarray product less than k](https://www.lintcode.com/problem/subarray-product-less-than-k/description?_from=ladder&&fromId=14)
```c++
    int numSubarrayProductLessThanK(vector<int> &nums, int k) {
        if(nums.size() == 0 || k <= 1) {
            return 0;
        }
        int left = 0, product = 1, res = 0;
        for(int right = 0; right < nums.size(); ++right) {
            product *= nums[right];
            while(product >= k) {
                product /= nums[left++];
            }
            res += right - left + 1;
        }
        return res;
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
[Palindrome Pairs](https://www.lintcode.com/problem/palindrome-pairs/)
```c++
    vector<vector<int>> palindromePairs(vector<string> &words) {
        unordered_map<string, int> hash;
        for(int i = 0; i < words.size(); ++i) {
            hash[words[i]] = i;
        }
        
        vector<vector<int>> res;
        for(int i = 0; i < words.size(); ++i) {
            for(int j = 0; j <= words[i].size(); ++j) {
                string left = words[i].substr(0, j);
                string right = words[i].substr(j);
                string tmp;
                if(valid(left)) {
                    tmp = right;
                    reverse(tmp.begin(), tmp.end());
                    if(hash.count(tmp) && hash[tmp] != i) {
                        res.push_back({hash[tmp], i});
                    }
                }
                if(!right.empty() && valid(right)) {
                    tmp = left;
                    reverse(tmp.begin(), tmp.end());
                    if(hash.count(tmp) && hash[tmp] != i) {
                        res.push_back({i, hash[tmp]});
                    }
                }
            }
        }
        
        return res;
    }
```

### Corner Cases 
- [Missing Range](https://www.lintcode.com/problem/missing-ranges/?_from=ladder&&fromId=14)
    - Key: corner case when the array is empty and handle case like [214783674] 0 214783674 about overflow
```c++
    vector<string> findMissingRanges(vector<int> &nums, int lower, int upper) {
        vector<string> res;
        if (nums.size() == 0) {
            getRange(res, lower, upper);
            return res;
        }
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
```
2. [Valid Number](https://www.lintcode.com/problem/valid-number/description?_from=ladder&&fromId=14)
    - pattern: `+/-` + `number/float number` + `e` + `+/-` + `integer`
    - add a space at the end of the input to handle out of bound (act like a *dummy* node)
```c++

    bool isNumber(string &s) {
        int l = 0, r = s.length() - 1;
        while (l <= r && isspace(s[l])) {
            l++;
        }
        while (l <= r && isspace(s[r])) {
            r--;
        }
        if (l > r) {
            return false;
        }
        s = s.substr(l, r - l + 1);
        s += ' '; l = 0; r = s.length() - 1;
        // add a space to handle edge cases
        if(s[l] == '+' || s[l] == '-') {
            ++l;
        }
        
        int nDigit = 0, nPoint = 0;
        while(isdigit(s[l]) || s[l] == '.') {
            if(isdigit(s[l])) {
                ++nDigit;
            }
            if(s[l] == '.') {
                ++nPoint;
            }
            ++l;
        }
        if(nDigit <= 0 || nPoint > 1) {
            return false;
        }
        if(s[l] == 'e') {
            ++l;
            if(s[l] == '+' || s[l] == '-') {
                ++l;
            }
            if(l == r) {
                return false;
            }
            for(; l < r; ++l) {
                if(!isdigit(s[l])) {
                    return false;
                }
            }
        }
        return l == r;
    }
```
3. [Integer to Roman](https://www.lintcode.com/problem/integer-to-roman/description?_from=ladder&&fromId=14)
    - separation of digits and conversion between different bits representation
    - any number can be viewed as sum of 1 2 4 8 16 32 ...
```c++
    string intToRoman(int n) {
        string ans;
        string Roman[] = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
        int value[] = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        for (int i = 0; i < 13; i++) { 
            while(n >= value[i]) {
                ans += Roman[i];
                n -= value[i];
            }
        } 
        return ans;
    }
    string intToRoman(int n) {
        string M[] = {"", "M", "MM", "MMM"};
        string C[] = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
        string X[] = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
        string I[] = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};
        return M[n / 1000] + C[(n / 100) % 10] + X[(n / 10) % 10] + I[n % 10];
    }
```
4. [Pow(x, n)](https://www.lintcode.com/problem/powx-n/description?_from=ladder&&fromId=14)
```c++
double myPow(double x, long n) {
    if(n < 0) {
        x = 1 / x;
        n = -n;
    }
    double result = 1;
    while(n != 0) {
        if(n % 2 == 1) {
            ans *= temp;
        }
        temp = temp * temp;
        n /= 2;
    }
    return ans;
```
5. [Find Celebrity](https://www.lintcode.com/problem/find-the-celebrity/description?_from=ladder&&fromId=14)
    - a celebrity: everyone knows him, but he knows nobody
    - like a competition, every time we bring one as candidate, and each `knows(ans, i)` can eliminate one's possibility as being the candidate
        - if the candidate know a people, then that means it is not the celebrity (if ans know i, then ans can't be the candidate)
        - otherwise, we keep the candidate (if ans don't know i, then i can't be th candidate)
```c++
    int findCelebrity(int n) {
        int ans = 0;
        for (int i = 1; i < n; i++) {
            if (knows(ans, i)) {
                ans = i;
            }
        }
        for (int i = 0; i < n; i++) {
            if (i < ans && knows(ans, i)) {
                return -1;
            }
            if (ans != i && !knows(i, ans)) {
                return -1;
            }
        }
        return ans;
    }
    // time complexity O(n)
```
6. [Sparse Matrix](https://www.lintcode.com/problem/sparse-matrix-multiplication/description?_from=ladder&&fromId=14)
```c++
    // i j k is the same as i k j
    // as for B, we could make a vector to note down the non-zero values
    vector<vector<int>> multiply(vector<vector<int>> &A, vector<vector<int>> &B) {
        vector<vector<int>> res(A.size(), vector<int>(B[0].size(), 0));
        // note down B's value previously
        vector<vector<pair<int, int>>> bVals(B.size());
        
        for(int i = 0; i < B.size(); ++i) {
            for(int j = 0; j < B[0].size(); ++j) {
                if(B[i][j] != 0) {
                    bVals[i].push_back({j, B[i][j]});
                }
            }
        }
        
        for(int i = 0; i < A.size(); ++i) {
            for(int k = 0; k < A[0].size(); ++k) {
                if(A[i][k] == 0) {
                    continue;
                }
                // without considering B's case
                // for(int j = 0; j < B[0].size(); ++j) {
                //     res[i][j] += A[i][k] * B[k][j];
                // }
                for(int j = 0; j < bVals[k].size(); ++j) {
                    int col = bVals[k][j].first;
                    int val = bVals[k][j].second;
                    res[i][col] += A[i][k] * val;
                }
            }
        }
        return res;
    }
```
#### **Related Problems**
- [String to Integer](https://www.lintcode.com/problem/string-to-integer-atoi/description?_from=ladder&&fromId=14)
```c++
    // this question is for corner cases 
    // such as floating points and negative number
    int atoi(string &str) {
        long ans = 0;
        int sign = 1;
        int i = 0;
        while(str[i] == ' ') {
            ++i;
        }
        if(str[i] == '+') {
            ++i;
        } else
        if(str[i] == '-') {
            sign = -1;
            ++i;
        }
        for(; i < str.length(); ++i) {
             if(str[i] >= '0' && str[i] <= '9'){
                ans = ans * 10 + (str[i] - '0') ;
                if(ans > INT_MAX) {
                    break;
                }
            } else if(str[i] != ' ') {
                break;
            }
        }
        if (ans * sign >= INT_MAX) {
            return INT_MAX;
        }
        if (ans * sign <= INT_MIN) {
            return INT_MIN;
        }
        return sign * (int)ans;
    }
```
- [Roman to Integer](https://www.lintcode.com/problem/roman-to-integer/description?_from=ladder&&fromId=14)
```c++
    int romanToInt(string &s) {
        int ans = 0;
        for(int i = 0; i < s.length(); ++i) {
            ans += toInt(s[i]);
            std::cout<<toInt(s[i]);
            if(i > 0 && toInt(s[i - 1]) < toInt(s[i])) {
                ans -= toInt(s[i - 1]) * 2;
            }
        }
        return ans;
    }
    
    int toInt(char s) {
        switch(s) {
            case 'I': return 1;
            case 'V': return 5;
            case 'X': return 10;
            case 'L': return 50;
            case 'C': return 100;
            case 'D': return 500;
            case 'M': return 1000;
            default:  return 0;
        }
    }
```
- [Min Stack](https://www.lintcode.com/problem/min-stack/description?_from=ladder&&fromId=14)
```c++
class MinStack {
public:
    stack<int> myStack, minStack;
    MinStack() {
        // do intialization if necessary
    }

    /*
     * @param number: An integer
     * @return: nothing
     */
    void push(int number) {
        myStack.push(number);
        if(minStack.empty() || number <= minStack.top()) {
            minStack.push(number);
        }
    }

    /*
     * @return: An integer
     */
    int pop() {
        int top = myStack.top();
        if(top == minStack.top()) {
            minStack.pop();
        }
        myStack.pop();
        return top;
    }

    /*
     * @return: An integer
     */
    int min() {
        return minStack.top();
    }
};
```

### Hashing of chars
1.[Find All Anagrams in a string](https://www.lintcode.com/problem/find-all-anagrams-in-a-string/?_from=ladder&&fromId=14)
```c++
    vector<int> findAnagrams(string &s, string &p) {
        if(s.length() < p.length()) {
            return {};
        }
        vector<int> charCount(256, 0);
        for(char ch: p) {
            ++charCount[ch];
        }
        int start = 0, end = 0, matched = 0, sLen = s.length(), pLen = p.length();
        vector<int> res;
        while(end < sLen) {
            if(charCount[s[end]] >= 1) {
                ++matched;
            }
            --charCount[s[end++]];
            if(matched == pLen) {
                res.push_back(start);
            }
            if(end - start == pLen) {
                if(charCount[s[start]] >= 0) {
                    --matched;
                }
                ++charCount[s[start++]];
            }
        }
        return res;
    }
        
    vector<int> findAnagrams(string &s, string &p) {   
        // another version use delta (difference)
        vector<int> det(256, 0);
        vector<int> ans;
        
        for(int i = 0; i < p.length(); ++i) {
            ++det[s[i]];
            --det[p[i]];
        }
        
        int abSum = 0;
        for(int item: det) {
            abSum += abs(item);
        }

        if(abSum == 0) {
            ans.push_back(0);
        }

        for(int i = p.length(); i < s.length(); ++i) {
            int r = s[i];
            int l = s[i - p.length()];
            
            abSum -= (abs(det[r]) + abs(det[l]));
            ++det[r]; --det[l];
            abSum += (abs(det[r]) + abs(det[l]));
            if(abSum == 0) {
                ans.push_back(i - p.length() + 1);
            }
        }
        return ans;
    }
```
2. [Valid Word Abbreviation](https://www.lintcode.com/problem/unique-word-abbreviation/description)
```c++
class ValidWordAbbr {
public:
    unordered_map<string,int> map;
    unordered_map<string,int> times; 
    /*
    * @param dictionary: a list of words
    */ValidWordAbbr(vector<string> dictionary) {
        for(string word: dictionary) {
            ++times[word];
            if(word.length() <= 2) {
                ++map[word];
            } else {
                ++map[word[0] + to_string(word.length() - 2) + word.back()];
            }
        }
    }

    /*
     * @param word: a string
     * @return: true if its abbreviation is unique or false
     */
    bool isUnique(string &word) {
        if(word.length() <= 2) {
            return times[word] == map[word];
        } else {
            return times[word] == map[word[0] + to_string(word.length() - 2) + word.back()];
        }
    }
};
```
3. [Load Balancer](https://www.lintcode.com/problem/load-balancer/description?_from=ladder&&fromId=14)
    - `server_id` is distinct
```c++
class LoadBalancer {
private:
    map<int, int> dict;
    vector<int> servers;
public:
    LoadBalancer() {
    }

    /*
     * @param server_id: add a new server to the cluster
     * @return: nothing
     */
    void add(int server_id) {
        if(!dict.count(server_id)) {
            dict[server_id] = servers.size();
            servers.push_back(server_id);
        }
    }

    /*
     * @param server_id: server_id remove a bad server from the cluster
     * @return: nothing
     */
    void remove(int server_id) {
        if(dict.count(server_id)) {
            int index = dict[server_id];
            servers[index] = servers.back();
            dict[servers[index]] = index;
            dict.erase(server_id);
            servers.pop_back();
        }
    }

    /*
     * @return: pick a server in the cluster randomly with equal probability
     */
    int pick() {
        return servers[rand() % servers.size()];
    }
};
```
4. [Longest Consecutive Subsequence](https://www.lintcode.com/problem/longest-consecutive-sequence/description?_from=ladder&&fromId=14) 
    - thinking out of soring
    - O(n) cause each element is being assessed only once
    - Hash: O(1) check/delete element
```c++
    int longestConsecutive(vector<int> &nums) {
        unordered_set<int> map(nums.begin(), nums.end());

        int longestLen = 0;
        for(int num: nums) {
            if(map.count(num)) {
                map.erase(num);
                int prev = num - 1, next = num + 1;
                while(map.count(prev)) {
                    map.erase(prev);
                   --prev; 
                }
                while(map.count(next)) {
                    map.erase(next);
                    ++next;
                }
                longestLen = max(longestLen, next - prev - 1);
            }
        }
        return longestLen;
    }
```
5. [Word Abbreviation](https://www.lintcode.com/problem/word-abbreviation/description?_from=ladder&&fromId=14)
    - time complexity: avg len of word * n
```c++
    vector<string> wordsAbbreviation(vector<string> &dict) {
        int len = dict.size();
        vector<string> ans(len);
        // vector<int> prefix(len);
        unordered_map<string, int> count;
    
        for (int i = 0; i < len; i++) {
            // prefix[i] = 1;
            ans[i] = getAbbr(dict[i], 1);
            count[ans[i]]++;
        }
        int round = 1;
        while (true) {
            ++round;
            bool unique = true;
            for (int i = 0; i < len; i++) {f
                if (count[ans[i]] > 1) {
                //   prefix[i]++;
                    ans[i] = getAbbr(dict[i], round);
                    count[ans[i]]++;
                    unique = false;
                    }
                }
            if (unique) {
                break;
            }
        }
        return ans;
    }
  
    string getAbbr(string s, int p) {
        if (p >= s.length() - 2) {
          return s;
        }
        string ans;
        ans = s.substr(0, p) + to_string(s.length() - 1 - p) + s.back();
        return ans;
    }
```
6. [Longest Substring without Repeating Characters](https://www.lintcode.com/problem/longest-substring-without-repeating-characters/description?_from=ladder&&fromId=14)
```c++
    int lengthOfLongestSubstring(string &s) {
        vector<int> lastIndex(256, -1);
        int left = 0, longestLen = 0;
        for(int right = 0; right < s.length(); ++right) {
            if(lastIndex[s[right]] == -1 || left > lastIndex[s[right]]) {
                longestLen = max(longestLen, right - left + 1);
            } else {
                left = lastIndex[s[right]] + 1;
            }
            lastIndex[s[right]] = right;
        }
        return longestLen;
    }
```
#### **Related**
- [Group Anagram](https://www.lintcode.com/problem/group-anagrams/description?_from=ladder&&fromId=14)
    - think about use what to be the key of the hash
```c++
    vector<vector<string>> groupAnagrams(vector<string> &strs) {
        unordered_map<string, multiset<string>> res;
        for(string word: strs){
            string key = word;
            sort(key);
            res[key].insert(word);
        }
        vector<vector<string>> anagrams;
        for(auto mp: res) {
            anagrams.push_back(vector<string>(mp.second.begin(), mp.second.end()));
        }
        return anagrams;
    }
    void sort(string & word) {
        vector<int> count(26, 0);
        for(char ch: word) {
            ++count[ch - 'a'];
        }
        word = "";
        for(int i = 0; i < 26; ++i) {
            word += string(count[i], i + 'a');
        }
    }
```
- [Valid Soduku](https://www.lintcode.com/problem/valid-sudoku/description?_from=ladder&&fromId=14)
```c++
    bool isValidSudoku(vector<vector<char>> &board) {
        // check row
        vector<bool> visited(10, false);
        
        for(int i = 0; i < board.size(); ++i) {
            clear(visited);
            for(int j = 0; j < board[0].size(); ++j) {
                if(!check(visited, board[i][j])) {
                    return false;
                }
            }
        }
        // check col
        for(int i = 0; i < board.size(); ++i) {
            clear(visited);
            for(int j = 0; j < board[0].size(); ++j) {
                if(!check(visited, board[j][i])) {
                    return false;
                }
            }
        }
        // check sub
        for(int i = 0; i < board.size(); i += 3) {
            for(int j = 0; j < board[0].size(); j += 3) {
                clear(visited);
                for(int k = 0; k < board.size(); ++k) {
                    if(!check(visited, board[i + k / 3][j + k % 3])) {
                        return false;
                    }
                }
            }
        }
        return true;
        
    }
    bool check(vector<bool> & visited, char ch) {
        if(ch == '.') {
            return true;
        }
        int digit = ch - '0';
        if(visited[digit]) {
            return false;
        }
        visited[digit] = true;
        return true;
    }
    
    void clear(vector<bool> & visited) {
        for(int i = 0; i < visited.size(); ++i) {
            visited[i] = false;
        }
    }
```

### BFS 
1. [Walls and Gates](https://www.lintcode.com/problem/walls-and-gates/description?_from=ladder&&fromId=14)
```c++
    void wallsAndGates(vector<vector<int>> &rooms) {
        queue<vector<int>> queue;
        vector<vector<int>> dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        for(int i = 0; i < rooms.size(); ++i) {
            for(int j = 0; j < rooms[0].size(); ++j) {
                if(rooms[i][j] == 0) {
                    queue.push({i, j});
                }
            }
        }
        while(!queue.empty()) {
            vector<int> cur = queue.front();
            queue.pop();
            for(int i = 0; i < dirs.size(); ++i) {
                int nx = cur[0] + dirs[i][0];
                int ny = cur[1] + dirs[i][1];
                if(nx >= 0 && nx < rooms.size() && ny >= 0 && ny < rooms[0].size() && rooms[nx][ny] == INT_MAX) {
                    queue.push({nx, ny});
                    rooms[nx][ny] = rooms[cur[0]][cur[1]] + 1;
                }
            }
        }
    }
```
2. [Zombie in Matrix](https://www.lintcode.com/problem/zombie-in-matrix/description?_from=ladder&&fromId=14)
    - initialize a queue with **multiple** entry pts
```c++
    int zombie(vector<vector<int>> &grid) {
        queue<vector<int>> queue;
        vector<vector<int>> dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        for(int i = 0; i < grid.size(); ++i) {
            for(int j = 0; j < grid[0].size(); ++j) {
                if(grid[i][j] == 1) {
                    queue.push({i, j}); 
                }
            }
        }
        int ans = -1;
        while(!queue.empty()) {
            ++ans;
            int size = queue.size();
            while(size-- >0) {
                vector<int> cur = queue.front(); queue.pop();
                for(int i = 0; i < dirs.size(); ++i) {
                    int nx = cur[0] + dirs[i][0];
                    int ny = cur[1] + dirs[i][1];
                    if(nx >= 0 && nx < grid.size() && ny >= 0 && ny < grid[0].size() && grid[nx][ny] == 0) {
                        queue.push({nx, ny});
                        grid[nx][ny] = -1; // mark visited
                    }
                }
            }
        }
        for (int i = 0; i < grid.size(); i++) {
            for (int j = 0; j < grid[0].size(); j++) {
                if (grid[i][j] == 0) {
                    return -1;
                }
            }
        }
        return ans;
    }
```
3. [Surrounded Regions](https://www.lintcode.com/problem/surrounded-regions/description?_from=ladder&&fromId=14)
    - search from the four bounder
```c++
    void surroundedRegions(vector<vector<char>> &board) {
        // visited four side
        if(board.size() == 0 || board[0].size() == 0) {
            return;
        }
        for(int i = 0; i < board.size(); ++i) {
            bfs(board, i, 0);
            bfs(board, i, board[0].size() - 1);
        }
        for(int i = 0; i < board[0].size(); ++i) {
            bfs(board, 0, i);
            bfs(board, board.size() - 1, i);
        }
        
        for(int i = 0; i < board.size(); ++i) {
            for(int j = 0; j < board[0].size(); ++j) {
                if(board[i][j] == 'O') {
                    board[i][j] = 'X';
                } else if(board[i][j] == 'W') {
                    board[i][j] = 'O';
                }
            }
        }
    }
    void bfs(vector<vector<char>> & board, int x, int y) {
        if(board[x][y] != 'O') {
            return;
        }
        board[x][y] = 'W';
        queue<vector<int>> queue;
        queue.push({x, y});
        // visited
        vector<int> dx = { 0, 1, 0, -1 };
        vector<int> dy = { 1, 0, -1, 0 };
        while(!queue.empty()) {
            vector<int> cur = queue.front();
            queue.pop();
            for(int i = 0; i < dx.size(); ++i) {
                int nx = cur[0] + dx[i];
                int ny = cur[1] + dy[i];
                if (0 <= nx && nx < board.size() && 0 <= ny && ny < board[0].size() && board[nx][ny] == 'O') {
                    board[nx][ny] = 'W'; // 'W' -> Water
                    queue.push({nx, ny});
                }
            }
        }
    }
```
4. [Open the Lock](https://www.lintcode.com/problem/open-the-lock/description?_from=ladder&&fromId=14)
```c++
    int openLock(vector<string> &deadends, string &target) {
        unordered_set<string> deadSet(deadends.begin(), deadends.end());
        unordered_set<string> visited;
        if(deadSet.count(target) || deadSet.count("0000")) {
            return -1;
        }
        if(target == "0000") {
            return 0;
        }
        int ans = 0;
        queue<string> queue;
        queue.push("0000");
        visited.insert("0000");
        while(!queue.empty()) {
            ++ans;
            int size = queue.size();
            while(size-- >0) {
                string cur = queue.front(); queue.pop();
                for(int k = 0; k < cur.length(); ++k) {
                    string next = cur;
                    next[k] = (cur[k] - '0' + 1) % 10 + '0';
                    if(target == next) {
                        return ans;
                    }
                    if(!deadSet.count(next) && !visited.count(next)) {
                        queue.push(next);
                        visited.insert(next);
                    }
                    
                    next[k] = (cur[k] - '0' - 1 + 10) % 10 + '0';
                    if(target == next) {
                        return ans;
                    }
                    if(!deadSet.count(next) && !visited.count(next)) {
                        queue.push(next);
                        visited.insert(next);
                    }
                }
            }
        }
        return -1;
    }
```
5. [Sliding Puzzle II](https://www.lintcode.com/problem/sliding-puzzle-ii/description?_from=ladder&&fromId=14)
```c++
    int minMoveStep(vector<vector<int>> &init_state, vector<vector<int>> &final_state) {
        // note down the state as string
        unordered_set<string> visited;
        string init = matrix2Str(init_state);
        string final = matrix2Str(final_state);
        if(init == final){
            return 0;
        }
        queue<string> queue;
        queue.push(init);
        visited.insert(init);
        int step = 0;
        int size;
        while(!queue.empty()) {
            ++step;
            size = queue.size();
            while(size-- > 0) {
                string cur = queue.front(); queue.pop();
                std::cout<<cur<<" ";

                vector<string> nextStates = getNext(cur, visited);
                for(string next: nextStates) {
                    if(next == final) {
                        return step;
                    }
                    queue.push(next);
                }
            }
        }
        return -1;
    }
    
    vector<string> getNext(string & state, unordered_set<string> & visited) {
        vector<string> res;
        vector<vector<int>> dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        int zeroX, zeroY;
        for(int i = 0; i < state.length(); ++i) {
            if(state[i] == '0') {
                zeroX = i / 3;
                zeroY = i % 3;
                break;
            }
        }
        // visited corresponding cells
        string tmp;
        for(int i = 0; i < dirs.size(); ++i) {
            tmp = state;
            int nextX = zeroX + dirs[i][0];
            int nextY = zeroY + dirs[i][1];
            // judge if it is in bound
            if(nextX >= 0 && nextX < 3 && nextY >= 0 && nextY < 3){
                swap(tmp[zeroX * 3 + zeroY], tmp[nextX * 3 + nextY]);
                if(!visited.count(tmp)) {
                    res.push_back(tmp);
                    visited.insert(tmp);
                }
            }
        }
        return res;
    }
    
    string matrix2Str(vector<vector<int>> & state) {
        string str = "";
        for(int i = 0; i < state.size(); ++i) {
            for(int j = 0; j < state[0].size(); ++j) {
                str += to_string(state[i][j]);
            }
        }
        return str;
    }
```
#### **Related Problems**
- [Pacific Atlanta Water Flow](https://www.lintcode.com/problem/pacific-atlantic-water-flow/description?_from=ladder&&fromId=14)
```c++
    vector<vector<int>> pacificAtlantic(vector<vector<int>> &matrix) {
        vector<vector<int>> res;
        if(matrix.size() == 0 || matrix[0].size() == 0) {
            return res;
        }
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<bool>> ocean1(m, vector<bool>(n, false));
        vector<vector<bool>> ocean2(m, vector<bool>(n, false));
        vector<vector<bool>> visited(m, vector<bool>(n, false)); // no used
        // bfs
        queue<vector<int>> queue1, queue2;
        vector<vector<int>> dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        for(int i = 0; i < m; ++i) {
            queue1.push({i, 0});
            queue2.push({i, n - 1});
        }
        for(int j = 0; j < n; ++j) {
            if(j != 0)
                queue1.push({0, j});
            if(j != n - 1)
                queue2.push({m - 1, j});
        }
        
        while(!queue1.empty()) {
            vector<int> cur = queue1.front();
            queue1.pop();
            ocean1[cur[0]][cur[1]] = true;
            for(int i = 0; i < dirs.size(); ++i) {
                int nextX = cur[0] + dirs[i][0], nextY = cur[1] + dirs[i][1];
                if(nextX >= 0 && nextX < m && nextY >= 0 && nextY < n && matrix[nextX][nextY] >=matrix[cur[0]][cur[1]] && !ocean1[nextX][nextY]){
                        queue1.push({nextX, nextY});
                }
            }
        }
        while(!queue2.empty()) {
            vector<int> cur = queue2.front();
            queue2.pop();
            ocean2[cur[0]][cur[1]] = true;
            for(int i = 0; i < dirs.size(); ++i) {
                int nextX = cur[0] + dirs[i][0], nextY = cur[1] + dirs[i][1];
                if(nextX >= 0 && nextX < m && nextY >= 0 && nextY < n && matrix[nextX][nextY] >=matrix[cur[0]][cur[1]] && !ocean2[nextX][nextY]){
                        queue2.push({nextX, nextY});
                }
            }
        }
        
        for(int i = 0; i < m; ++i) {
            for(int j = 0; j < n; ++j) {
                if(ocean1[i][j] && ocean2[i][j]) {
                    res.push_back({i, j});
                }
            }
        }
        return res;
    }
```
- [Build Post Office II](https://www.lintcode.com/problem/build-post-office-ii/?_from=ladder&&fromId=14)
```c++
    // https://www.cnblogs.com/aprilyang/p/6507928.html
    
    vector<vector<int>> dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    int m, n;
    int shortestDistance(vector<vector<int>> &grid) {
        // since we have less house we would expand from the house
        if(grid.size() == 0 || grid[0].size() == 0) {
            return 0;
        }
        m = grid.size();
        n = grid[0].size();
        
        // add houses onto the queue 
        queue<vector<int>> queue;
        // two matrix one is for step and one is to note down how many times the empty place is visited
        vector<vector<int>> visitedTimes(m, vector<int>(n, 0));
        vector<vector<int>> steps(m, vector<int>(n, 0));
        
        int numHouses = 0;
        for(int i = 0; i < m; ++i) {
            for(int j = 0; j < n; ++j) {
                if(grid[i][j] == 1) {
                    bfs(i, j, grid, visitedTimes, steps);
                    ++numHouses;
                }
            }
        }
        // get the ans
        int min = INT_MAX;
        for(int i = 0; i < m; ++i) {
            for(int j = 0; j < n; ++j) {
                if(grid[i][j] == 0) {
                    if(visitedTimes[i][j] == numHouses) {
                        if(steps[i][j] != 0 && steps[i][j] < min) {
                            min = steps[i][j];
                        }
                    }
                }
            }
        }
        return min == INT_MAX ? -1: min;
    }
    
    void bfs(int x, int y, vector<vector<int>> & grid, vector<vector<int>> & visitedTimes, vector<vector<int>> & steps) {
        queue<vector<int>> queue;
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        queue.push({x, y});
        int step = 0;
        while(!queue.empty()) {
            ++step;
            int size = queue.size();
            while(size-- >0) {
                vector<int> cur = queue.front(); queue.pop();
                for(int i = 0; i < dirs.size(); ++i) {
                    int nextX = cur[0] + dirs[i][0];
                    int nextY = cur[1] + dirs[i][1];
                    if(nextX < 0 || nextY < 0 || nextX >= m || nextY >= n) {
                        continue;
                    }
                    if(grid[nextX][nextY] != 0 || visited[nextX][nextY]) {
                        continue;
                    }
                    queue.push({nextX, nextY});
                    visited[nextX][nextY] = true;
                    visitedTimes[nextX][nextY] += 1;
                    steps[nextX][nextY] += step;
                }
            }
        }
    }
```
- [Binary Tree Vertical Order Traversal](https://www.lintcode.com/problem/binary-tree-vertical-order-traversal/)
```c++
    vector<vector<int>> verticalOrder(TreeNode * root) {
        if(!root) {
            return {};
        }
        map<int, vector<int>> hash;
        queue<pair<TreeNode *, int>> queue;
        queue.push(make_pair(root, 0));

        while(!queue.empty()) {
            pair<TreeNode *, int> cur = queue.front(); queue.pop();
            hash[cur.second].push_back(cur.first->val);
            if(cur.first->left) {
                queue.push(make_pair(cur.first->left, cur.second - 1));
            }
            if(cur.first->right) {
                queue.push(make_pair(cur.first->right, cur.second + 1));
            }
        }
        
        vector<vector<int>> res;
        for(auto it: hash) {
            res.push_back(it.second);
        }
        return res;
    }
```

### Tree
1. [Convert BST to Greater Tree](https://www.lintcode.com/problem/convert-bst-to-greater-tree/?_from=ladder&&fromId=14)
    - key: break bigger task into smaller task
```c++
    int sum = 0;
    void dfs(TreeNode* cur) {
        if (!cur) {
            return;
        }
        dfs(cur->right);     
        sum += cur->val;      
        cur->val = sum;
        dfs(cur->left);
    }
  
    TreeNode* convertBST(TreeNode* root) {
        dfs(root);
        return root;
    }
```
2.[In-order Successor in BST]()
    - did it in recursion instead of iteration
```c++
    TreeNode* inorderSuccessor(TreeNode * root, TreeNode * p) {
        if (!root || !p) {
            return nullptr;
        }

        if (root->val <= p->val) {
            return inorderSuccessor(root->right, p); // successor in the right
        } else {
            // if left node exist return it else return the root
            TreeNode* left = inorderSuccessor(root->left, p);
            return (left) ? left : root;
        }
    }
```
3. [Binary Tree Upside Down](https://www.lintcode.com/problem/binary-tree-upside-down/description)
```c++
    //  https://www.cnblogs.com/grandyang/p/5172838.html
    TreeNode * upsideDownBinaryTree(TreeNode * root) {
        // recursive solution
        if(!root) {
            return root;
        }
        return dfs(root);
        
        // iterative sols
        TreeNode * cur = root, * pre = nullptr, * next = nullptr, * tmp = nullptr;
        while (cur) {
            next = cur->left;
            cur->left = tmp;
            tmp = cur->right;
            cur->right = pre;
            pre = cur; cur = next;
        }
        return pre;
    }
    
    TreeNode* dfs(TreeNode* cur) {
        if(!cur->left) {
            return cur;
        }
        TreeNode * newRoot = dfs(cur->left);
        cur->left->right = cur;
        cur->left->left = cur->right;
        cur->left = nullptr; // important
        cur->right = nullptr; // important
        return newRoot;
    }
```
4. [Find Leaves of Binary Tree](https://www.lintcode.com/problem/find-leaves-of-binary-tree/description?_from=ladder&&fromId=14)
```c++
    unordered_map<int, vector<int>> depths;
    vector<vector<int>> findLeaves(TreeNode * root) {
        int maxDepth = helper(root);
        vector<vector<int>> res(maxDepth);
        for(int i = 1; i <= maxDepth ; ++i) {
            res[i - 1] = (depths[i]);
        }
        return res;
    }
    int helper(TreeNode* cur) {
        if(!cur) {
            return 0;
        }
        int curDepth = max(helper(cur->left), helper(cur->right)) + 1;
        depths[curDepth].push_back(cur->val);
        return curDepth;
    }
```
5. [Word Break ii](https://www.lintcode.com/problem/word-break-ii/)
```c++
    vector<string> wordBreak(string s, unordered_set<string> & dict) {
        map<string, vector<string>> done;
        done[""] = {""};
        return dfs(s, dict, done);
    }

    vector<string> dfs(string s, unordered_set<string> & dict, map<string, vector<string>> & done) {
        if (done.count(s)) {
            return done[s];
        }
        vector<string> ans;

        for (int len = 1; len <= s.length(); len++) {  //将s 分割成s1 s2  其中s1长度为len
            string s1 = s.substr(0, len);
            string s2 = s.substr(len);

            if (dict.count(s1)) {
                vector<string> s2_res = dfs(s2, dict, done);
                for (string item : s2_res) {
                    if (item == "") {
                        ans.push_back(s1);
                    } else {
                        ans.push_back(s1 + " " + item);
                    }
                }
            }
        }
        done[s] = ans;
        return ans;
    }
```

### DFS
1. [Word Squares](https://www.lintcode.com/problem/word-squares/description?_from=ladder&&fromId=14)
    - key: hash/Trie note down the prefix of words
    - only need to search from specific prefix words
```c++
    // time complexity: words.length()^l but 2 optimization make constant factors better
    void initHashPrefix(unordered_map<string, vector<string>> & hash, vector<string> & words) {
        for(string word: words) {
            hash[""].push_back(word); // !!!
            string prefix = "";
            for(char ch: word) {
                prefix += ch;
                hash[prefix].push_back(word);
            }
        }
    }
    
    void dfs(int index, int len, vector<string> & subset, vector<vector<string>> & results, unordered_map<string, vector<string>> & hash) {
        if(index == len) {
            results.push_back(subset);
            return; // important here!!!
        }
        // optimization: get the corresponding prefix
        string prefix = "";
        for(string prevWord: subset) {
            prefix += prevWord[index];
        }
        
        for(string item: hash[prefix]) {
            // !!! optimization: check prefix for the following levels
            if(!checkPrefix(index, len, item, subset, hash)) {
                continue;
            }
            subset.push_back(item);
            dfs(index + 1, len, subset, results, hash);
            subset.pop_back();
        }
    }

    bool checkPrefix(int index, int len, string nextWord, vector<string> & subset, unordered_map<string, vector<string>> & hash) {
        for(int i = index + 1; i < len; ++i) { // col eg i = [2, 3] 2: le 3: la
            string prefix = "";
            for(string item: subset) {
                prefix += item[i];
            }
            prefix += nextWord[i];
            if(!hash.count(prefix)) {
                return false;
            }
        }
        return true;
    }
    
    vector<vector<string>> wordSquares(vector<string> &words) {
        vector<vector<string>> results;
        if(words.size() == 0) {
            return results;
        }
        int len = words[0].length();
        if(len == 0) {
            return results;
        }
        
        unordered_map<string, vector<string>> hashPrefix;
        initHashPrefix(hashPrefix, words);
        
        vector<string> subset;
        dfs(0, len, subset, results, hashPrefix);
        return results;
    }
```
2. [Factorization](https://www.lintcode.com/problem/factorization/description?_from=ladder&&fromId=14)
    - optimization of `i <= sqrt(remain)`
```c++
class Solution{
public:
    // time complexity **exponential**
    // solved by dfs
    
    vector<int> subset;
    vector<vector<int>> result;
    
    void dfs(int start, int remain) {
        if(remain == 1) {
            if(subset.size() > 1) {
                result.push_back(subset);
            }
            return; // important
        }
        
        // i don't need to traverse to remain
        // i <= remain/i since start <= remain 
        // thus i <= sqrt(remain)
        
        // only optimize to break for loop 
        // but note the case when i == remain 
        // then next level would have remain == 1
        
        for(int i = start; i <= sqrt(remain); ++i) {
            if(remain % i == 0) {
                // i is a factor of remain
                subset.push_back(i);
                dfs(i, remain / i);
                subset.pop_back();
            }
        }
        
        // i == remain
        subset.push_back(remain);
        dfs(remain, 1);
        subset.pop_back();
    }
    
    vector<vector<int>> getFactors(int n) {
        dfs(2, n);
        return result;
    }
};
```
3. [Expression Add operator](https://www.lintcode.com/problem/expression-add-operators/description?_from=ladder&&fromId=14)
    - *: sum = sum - lastFactor + lastFactor * number
        - lastF = lastF * number
    - +/-: sum = sum +/- number
        - lastF = number
```c++
    vector<string> result;
    
    void dfs(string & num, string curStr, int index, long long curVal, long long lastF, int target) {
        if(index == num.length()){
            if(curVal == target) {
                result.push_back(curStr);
            }
            return;
        }
        for(int i = index; i < num.length(); ++i) {
            long long x = stol(num.substr(index, i - index + 1));
            if(index == 0) {
                dfs(num, "" + to_string(x), i + 1, x, x, target);
            } else {
                dfs(num, curStr + "*" + to_string(x), i + 1, curVal - lastF + lastF * x, lastF * x, target);
                dfs(num, curStr + "+" + to_string(x), i + 1,  curVal + x, x, target);
                dfs(num, curStr + "-" + to_string(x), i + 1, curVal - x, -x, target);
            }
            if(x == 0) {
                std::cout<<curStr<<endl;
                break;
            }
        }
    }
    vector<string> addOperators(string &num, int target) {
        dfs(num, "", 0, 0, 0, target);
        return result;
    }
```
4. [Word Ladder ii](https://www.lintcode.com/problem/word-ladder-ii/)
```c++
class Solution{
public:
    map<string, vector<string>> graph;
    vector<vector<string>> ans;
    map<string, int> lb;
    
    void dfs(int limit, int x, string word, string end, vector<string> & path) {
        if (x == limit + 1) {
            if (word == end) {                  //reach the end
                ans.push_back(path);        //save the path
            }
            return;
        }
        
        if (x - 1 + lb[word] > limit) {     //如果当前单词的变换次数加上不同的字母数超出限制就退出
            return;
        }
        
        for (string next : graph[word]) {
            path.push_back(next);
            dfs(limit, x + 1, next, end, path);  
            path.pop_back();        
        }
        
        if (ans.empty()) {
            lb[word] = max(lb[word], limit - (x - 1) + 1);
        }
    }
    
    vector<string> getNext(string word, unordered_set<string> & dict) {
        vector<string> ret ;
        
        for (int i = 0; i < word.length(); i++) {   //枚举当前单词替换位置
            
            for (char c = 'a'; c <= 'z'; c++) {    
                string next = word;
                //枚举当前可替换字母
                next[i] = c;
                if (dict.count(next) && next != word) {   //如果替换字母后的单词在dict中
                    ret.push_back(next);                                 //加入ret中
                }
            }
        }
        return ret;
    }
    
    int getDiff(string a, string b) {           //比较两个字符串不同的字母个数
        int ret = 0;
        for (int i = 0; i < a.length(); i++) {
            if (a[i] != b[i]) {
                ret++;
            }
        }
        return ret;
    } 
    
    vector<vector<string>> findLadders(string &start, string &end, unordered_set<string> &dict) {
        
        dict.insert(start);
        dict.insert(end);
        for (string word : dict) {
            graph[word] = getNext(word, dict); // save the next possible transfomations
            lb[word] = getDiff(word, end);
        }
        
        int limit = 0;
        vector<string> path;
        path.push_back(start);
        
        while (ans.empty()) {
            dfs(limit, 1, start, end, path);    // begin to search
            limit++;                            // each time we expand our limit  of the depth
        }
        return ans;
    }
};
```
5. [Sudoku Solver](https://www.lintcode.com/problem/sudoku-solver/description?_from=ladder&&fromId=14)
```c++
    void solveSudoku(vector<vector<int>> &board) {
        solve(board);
    }
    
    bool solve(vector<vector<int>> &board) {
        for(int i = 0; i < 9; i++) {
            for(int j = 0; j < 9; j++){
                if(board[i][j] != 0){
                    continue;
                }
                for(int k = 1; k <= 9; ++k) {
                    board[i][j] = k;
                    if(isValid(i, j, board) && solve(board)) {
                        return true;
                    }
                    board[i][j] = 0;
                }
                return false;
            }
        }
        return true;
    }
    
    bool isValid(int i, int j, vector<vector<int>> & board) {
        // same row
        unordered_set<int> visited;
        for(int k = 0; k < 9; ++k) {
            if(visited.count(board[i][k])) {
                return false;
            }
            if((board[i][k]) != 0){
                visited.insert(board[i][k]);
            }
        }
        
        visited.clear();
        std::cout<<visited.size();
        std::cout<<visited.size();
        for(int k = 0; k < 9; ++k) {
            if(visited.count(board[k][j])) {
                return false;
            }
            if(board[k][j] != 0)
            visited.insert(board[k][j]);
        }
        
        visited.clear();
        for (int m = 0; m < 3; m++) {
            for (int n = 0; n < 3; n++){
                int x = i / 3 * 3 + m, y = j / 3 * 3 + n;
                if (visited.count(board[x][y])) {
                    return false;
                }
                if(board[x][y] != 0)
                visited.insert(board[x][y]);
            } 
        }
        return true;
    }
```


### More 
#### **Union Find** Algorithm
-  [Minimum Spanning Tree](https://www.lintcode.com/problem/minimum-spanning-tree/description?_from=ladder&&fromId=14)
```c++
// /**
//  * Definition for a Connection.
//  * class Connection {
//  * public:
//  *   string city1, city2;
//  *   int cost;
//  *   Connection(string& city1, string& city2, int cost) {
//  *       this->city1 = city1;
//  *       this->city2 = city2;
//  *       this->cost = cost;
//  * }
//  */

bool cmp(const Connection& c1, const Connection& c2) {
    if (c1.cost != c2.cost)
        return c1.cost < c2.cost;

    if (c1.city1 != c2.city1)
        return c1.city1 < c2.city1;

    return c1.city2 < c2.city2;
}


class UnionFind {
public:
    vector<int> f;
    UnionFind(int num) {
        f.resize(num);
        fill(f.begin(), f.end(), -1);
    }
    void union2(int num1, int num2) {
        num1 = find(num1);
        num2 = find(num2);
        if(f[num1] < f[num2]) {
            f[num1] += f[num2];
            f[num2] = num1;
        } else {
            f[num2] += f[num1];
            f[num1] = num2;
        }
    }
    int find(int num) {
        if(f[num] < 0) {
            return num;
        }
        f[num] = find(f[num]);
        return f[num];
    }
};
 
class Solution {
public:
    /**
     * @param connections given a list of connections include two cities and cost
     * @return a list of connections from results
     */
    vector<Connection> lowestCost(vector<Connection>& connections) {
        vector<Connection> res;
        sort(connections.begin(), connections.end(), cmp);
        unordered_map<string, int> stringToID2;
        int numCity = 0;
        for(Connection item: connections) {
            if(!stringToID2.count(item.city1)){
                stringToID2[item.city1]= numCity++;
            }
            if(!stringToID2.count(item.city2)){
                stringToID2[item.city2]= numCity++;
            }
        }
        
        UnionFind ufs(numCity); // important  + 1 here!!! or set numCity begin w 0
        for(Connection item: connections) {
            int city1 = stringToID2[item.city1];
            int city2 = stringToID2[item.city2];
            if(ufs.find(city1) == ufs.find(city2)) {
                continue;
            }
            ufs.union2(city1, city2);
            res.push_back(item);
            
        }

        if(res.size() == numCity - 1) {
            return res;
        } else {
            return {};
        }
    }
};
```
- [Number of Islands II](https://www.lintcode.com/problem/number-of-islands-ii/description?_from=ladder&&fromId=14)
```c++
/**
 * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */
class UnionFind{
public:
    vector<int> f;
    UnionFind(int num) {
        f.resize(num);
        fill(f.begin(), f.end(), -1);
    }
    int find(int num) {
        if(f[num] < 0) {
            return num;
        }
        f[num] = find(f[num]);
        return f[num];
    }
    void union2(int num1, int num2) {
        num1 = find(num1);
        num2 = find(num2);
        if(num1 < num2) {
            f[num1] += f[num2];
            f[num2] = num1;
        } else {
            f[num2] += f[num1];
            f[num1] = num2;
        }
    }
};

class Solution {
public:
    /**
     * @param n: An integer
     * @param m: An integer
     * @param operators: an array of point
     * @return: an integer array
     */
         
    int convert2ID(int x, int y, int n) {
        return x * n + y;
    }
    
    vector<vector<int>> dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    vector<int> numIslands2(int m, int n, vector<Point> &operators) {
        if(n == 0 || m == 0 || operators.size() == 0) {
            return {};
        }
        vector<int> res;
        vector<vector<int>> grid(m, vector<int>(n, 0));
        UnionFind ufs(m * n);
        int count = 0;
        for(Point pt: operators) {
            if(grid[pt.x][pt.y] == 1) {
                res.push_back(count); // still want to push the count if already visited
                continue;
            }
            // union its four points around if they are 1 
            // all are same then no change in num of islands
            // isolated + 1
            // 1 different find no change two different find count -1 3 different find count - 2 4 different find count - 3
            grid[pt.x][pt.y] = 1;
            ++count;
            int curNum = convert2ID(pt.x, pt.y, n);
            for(int i = 0; i < dirs.size(); ++i) {
                int nX = pt.x + dirs[i][0]; 
                int nY = pt.y + dirs[i][1];
                if(nX < 0 || nY < 0 || nX >= m || nY >= n || grid[nX][nY] == 0) {
                    continue;
                }
                int nXID = convert2ID(nX, nY, n);
                if(ufs.find(curNum) != ufs.find(nXID)){
                    ufs.union2(curNum, nXID);
                    --count;
                }
            }
            res.push_back(count);
        }
        return res;
    }

};
```
#### **Segment Tree**
- [Interval Sum](https://www.lintcode.com/problem/interval-sum/) 
- [Segment Tree II](https://www.lintcode.com/problem/interval-sum-ii/description)
```c++
class SegmentTree{
public:
    SegmentTree * left;
    SegmentTree * right;
    int start; 
    int end;
    long long sum;
    
    SegmentTree(int start, int end, long long sum): left(nullptr), right(nullptr), start(start), end(end), sum(sum) {}
};

class Solution {
public:
    /* you may need to use some attributes here */

    /*
    * @param A: An integer array
    */
    SegmentTree * root;
    
    SegmentTree * build(int start, int end, vector<int> & arr) {
        if(start > end) {
            return nullptr;
        }
        SegmentTree * node = new SegmentTree(start, end, (long long)arr[start]);

        if(start == end) {
            return node;
        }
        int mid = start + (end - start) / 2;
        node->left = build(start, mid, arr);
        node->right = build(mid + 1, end, arr);
        node->sum = node->left->sum + node->right->sum;
        return node;
    }
    
    long long query(SegmentTree *root, int start, int end) {
        if(start <= root->start && root->end <= end) {
            return root->sum;
        }
        if (root->left->end >= end) {
            return query(root->left, start, end);
        }
        if (root->right->start <= start) {
            return query(root->right, start, end);
        }
        long long leftsum = query(root->left, start, root->left->end);
        long long rightsum = query(root->right, root->right->start, end);
        return leftsum + rightsum;
    }
    
    void update(SegmentTree *root, int index, int value) {
        if(root->start == index && root->end == index) { // find
            root->sum = (long long)value;
            return;
        }
        
        int mid = (root->start + root->end) / 2;
        if(root->start <= index && index <= mid) {
            update(root->left, index, value);
        }else if(mid < index && index <= root->end) {
            update(root->right, index, value);
        }
        // update
        root->sum = root->left->sum + root->right->sum;
    }
    
    Solution(vector<int> A) {
        root = build(0, A.size() - 1, A);
    }

    /*
     * @param start: An integer
     * @param end: An integer
     * @return: The sum from start to end
     */
    long long query(int start, int end) {
        return query(root, start, end);
    }

    /*
     * @param index: An integer
     * @param value: An integer
     * @return: nothing
     */
    void modify(int index, int value) {
        update(root, index, value);
    }
};
```