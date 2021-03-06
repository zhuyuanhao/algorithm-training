# Longest Palindromic Substring

- tags: [palindrome]

## Question

- leetcode: [Longest Palindromic Substring | LeetCode OJ](https://leetcode.com/problems/longest-palindromic-substring/)
- lintcode: [(200) Longest Palindromic Substring](http://www.lintcode.com/en/problem/longest-palindromic-substring/)

```
Given a string S, find the longest palindromic substring in S.
You may assume that the maximum length of S is 1000,
and there exists one unique longest palindromic substring.

Example
Given the string = "abcdzdcab", return "cdzdc".
Challenge
O(n2) time is acceptable. Can you do it in O(n) time.
```

## 题解1 - 穷竭搜索

最简单的方案，穷举所有可能的子串，判断子串是否为回文，使用一变量记录最大回文长度，若新的回文超过之前的最大回文长度则更新标记变量并记录当前回文的起止索引，最后返回最长回文子串。

### Python

```python
class Solution:
    # @param {string} s input string
    # @return {string} the longest palindromic substring
    def longestPalindrome(self, s):
        if not s:
            return ""

        n = len(s)
        longest, left, right = 0, 0, 0
        for i in xrange(0, n):
            for j in xrange(i + 1, n + 1):
                substr = s[i:j]
                if self.isPalindrome(substr) and len(substr) > longest:
                    longest = len(substr)
                    left, right = i, j
        # construct longest substr
        result = s[left:right]
        return result

    def isPalindrome(self, s):
        if not s:
            return False
        # reverse compare
        return s == s[::-1]
```

### C++

```c++
class Solution {
public:
    /**
     * @param s input string
     * @return the longest palindromic substring
     */
    string longestPalindrome(string& s) {
        string result;
        if (s.empty()) return s;

        int n = s.size();
        int longest = 0, left = 0, right = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j <= n; ++j) {
                string substr = s.substr(i, j - i);
                if (isPalindrome(substr) && substr.size() > longest) {
                    longest = j - i;
                    left = i;
                    right = j;
                }
            }
        }

        result = s.substr(left, right - left);
        return result;
    }

private:
    bool isPalindrome(string &s) {
        int n = s.size();
        for (int i = 0; i < n; ++i) {
            if (s[i] != s[n - i - 1]) return false;
        }
        return true;
    }
};
```

### Java

```java
public class Solution {
    /**
     * @param s input string
     * @return the longest palindromic substring
     */
    public String longestPalindrome(String s) {
        String result = new String();
        if (s == null || s.isEmpty()) return result;

        int n = s.length();
        int longest = 0, left = 0, right = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j <= n; j++) {
                String substr = s.substring(i, j);
                if (isPalindrome(substr) && substr.length() > longest) {
                    longest = substr.length();
                    left = i;
                    right = j;
                }
            }
        }

        result = s.substring(left, right);
        return result;
    }

    private boolean isPalindrome(String s) {
        if (s == null || s.isEmpty()) return false;

        int n = s.length();
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) != s.charAt(n - i - 1)) return false;
        }

        return true;
    }
}
```

### 源码分析

使用`left`, `right`作为子串的起止索引，用于最后构造返回结果，避免中间构造字符串以减少开销。

### 复杂度分析

穷举所有的子串，$$O(C_n^2) = O(n^2)$$, 每次判断字符串是否为回文，复杂度为 $$O(n)$$, 故总的时间复杂度为 $$O(n^3)$$. 故大数据集下可能 TLE. 使用了`substr`作为临时子串，空间复杂度为 $$O(n)$$.

## 题解2
### C++
```c++
string palindrome(string& s, int l, int r) {
	while (l>=0 && r<s.size() && s[l]==s[r]) l--, r++;
	return s.substr(l+1, r-l-1);
}

string longestPalindrome(string s) {
	if (s.empty()) return s;

	string res;
	for (int i=0; i<s.size(); i++) {
		string t;
		t = palindrome(s, i, i);
		if (t.size() > res.size()) res = t;
	   
		t = palindrome(s, i, i+1);
		if (t.size() > res.size()) res = t;   
	}
	return res;
}
```

### 源码分析
假定扫描的每个字母是回文的中间位置（需要处理奇偶两种情况），从该位置向两头搜索寻找最大回文长度

### 复杂度分析
时间复杂度降到O(n^2)了

## 题解3
另外还有一个O（n）的解法，具体参考下面的链接
http://articles.leetcode.com/2011/11/longest-palindromic-substring-part-ii.html

## Reference

- [Longest Palindromic Substring Part I | LeetCode](http://articles.leetcode.com/2011/11/longest-palindromic-substring-part-i.html)
- [Longest Palindromic Substring Part II | LeetCode](http://articles.leetcode.com/2011/11/longest-palindromic-substring-part-ii.html)
