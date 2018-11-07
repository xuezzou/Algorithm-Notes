### Lintcode
> [Lintcode Ladder](https://www.lintcode.com/ladder/1/)

#### Content
- [Hack the Algorithm Interview](#hack-the-algorithm-interview)
- [Binary Search & log(n) Algorithm](#Binary-Search-&-logn-algorithm)
- [Two-pointer Algorithm](#two-pointer-algorithm)
- BFS & Topological Sort
- Binary Tree & Tree-based DFS
- Combination-based DFS
- Permutation-based & Graph-based DFS
- Data Structure - Stack, Queue, Hash, Heap
- Data Structure - Interval, Array, Matrix & Binary Indexed Tree
- Additional - Dynamic Programming
 
---

#### Hack the Algorithm Interview
1. [Logest Palindrome](https://www.lintcode.com/problem/longest-palindrome/description?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param s: a string which consists of lowercase or uppercase letters
     * @return: the length of the longest palindromes that can be built
     */
    int longestPalindrome(string &s) {
        if(s.length() <= 1 ) {
            return s.length();
        }
        int ans = 0;
        std::unordered_map<char, int> hash;
        for(size_t i = 0; i < s.length(); ++i) {
            ++hash[s[i]];
        }
        std::unordered_map<char, int>::iterator it;
        for(it = hash.begin(); it != hash.end(); ++it) {
            if(it->second > 1) ans+=it->second % 2 == 0 ? it->second : it->second - 1;
        }
        if(hash.size() == 1) {
            return hash.begin()->second;
        }
        return ans + 1;
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
            for(int k = 0; k < target.length(); ++k) {
                if(source[i + k] != target[k]) {
                    break;
                }
                if(k == target.length() - 1) {
                    return i;
                }
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

        int i = 0, j = s.length() - 1;
        while(i < j) {
            if(isalnum(s[i])) {
                if(isalnum(s[j])) {
                    if(tolower(s[i++])!= tolower(s[j--])) {
                        return false;
                    }
                } else{
                    --j;
                }
            } else{
                ++i;
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
        if(s.length() <= 1) {
            return s;
        }
        
        bool dp[s.length()][s.length()] = {false};
        size_t startIndex = 0, longestLen = 1;
        
        for(size_t i = 0; i < s.length(); ++i) {
            dp[i][i] = true;
            if(i != s.length() - 1) {
                dp[i][i+1] = (s[i] == s[i + 1]);
                if(dp[i][i +1]) {
                    longestLen = 2;
                    startIndex = i;
                }
            }
        }

        for(size_t i = 0; i < s.length() - 2; ++i) {
            for(size_t j = i + 2; j < s.length(); ++j) {
                dp[i][j] = (s[i] == s[j]) && dp[i + 1][ j - 1];
                if(dp[i][j]) { 
                    if(j - i + 1 > longestLen){
                        longestLen =  j - i + 1;
                        startIndex = i;
                    }
                }
            }
        }
        return s.substr(startIndex, longestLen);
    }
    ```

##### Binary Search & logN Algorithm
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
        if(nums[start] > nums[end]) {
            return nums[start];
        }
        return nums[end];
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
        
        if(k == 0) {
            return {};
        }
        // if(A.size() == 1) {
        //     return A;
        // }
        
        vector<int> results;

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
        int i = 0;
        while( i < k && start >= 0 && end < A.size()){
            if(target - A[start] > A[end] - target) {
                results.push_back(A[end++]);
            } else {
                results.push_back(A[start--]);
            }
            ++i;
        }
        while(i < k) {
            if(start >= 0) {
                results.push_back(A[start--]);
                ++i;
            } else {
                results.push_back(A[end++]);
                ++i;
            }
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
        int index = 1;
        while (reader->get(index - 1) < target) {
            index = index * 2;
        }
        
        int start = 0, end = index - 1;
        while(start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (reader->get(mid) >= target) {
                end = mid;
            }else{
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
    // iterative solution
    /**
     * @param x the base number
     * @param n the power number
     * @return the result
     */
    public double myPow(double x, int n) {
        // Write your code here
        boolean isNegative = false;
        if (n < 0) {
            x = 1 / x;
            isNegative = true;
            n = -(n + 1); // Avoid overflow when n == MIN_VALUE
        }

        double ans = 1, tmp = x;

        while (n != 0) {
            if (n % 2 == 1) {
                ans *= tmp;
            }
            tmp *= tmp;
            n /= 2;
        }
        
        if (isNegative) {
            ans *= x;
        }
        return ans;
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
            return 0;
        }
        int start = 0, end = nums.size() - 1;
        int minIndex = 0;
        while(start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[start] < nums[mid]) {
                if(nums[start] < nums[end]) {
                    return nums[start];
                } else {
                    start = mid;
                }
            } else {
                end = mid;
            }
        }
        if(nums[start] < nums[end]) {
            return nums[start];
        }
        return nums[end];
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
        if (1 == n) {
            return a % b;
        } else if (0 == n) {
            // do not use 1 instead (1 % b)! b = 1
            return 1 % b;
        } else if (0 > n) {
            return -1;
        }

        // (a * b) % p = ((a % p) * (b % p)) % p
        // use long long to prevent overflow
        long long product = fastPower(a, b, n / 2);
        product = (product * product) % b;
        if (1 == n % 2) {
            product = (product * a) % b;
        }

        // cast long long to int
        return (int) product;
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
            if(A[mid] > A[mid - 1] && A[mid] > A[mid + 1]) {
                return mid;
            } 
            if(A[mid] > A[mid - 1]) {
                start = mid;
            } else {
                end = mid;
            }
        }
        if(A[start] > A[end]) {
            return start;
        } 
        return end;
    }
    ```
10. [First Bad Version](https://www.lintcode.com/problem/first-bad-version/description?_from=ladder&&fromId=1)
    ```c++
    /**
     * @param n: An integer
     * @return: An integer which is the first bad version.
     */
    int findFirstBadVersion(int n) {
        int start = 1, end = n;
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
                if(target >= A[start] && target <= A[mid]) {
                    end = mid;
                } else {
                    start = mid;
                }
            } else {
                if(target >= A[mid] && target <= A[end]) {
                    start = mid;
                } else {
                    end = mid;
                }
            }
        } 
        if(A[start] == target) {
            return start;
        }
        if(A[end] == target) {
            return end;
        }
        return -1;
    }
    ```

#### Two-pointer Algorithm
1. []()
2. []()
3. []()
4. []()
5. []()
6. []()
7. []()