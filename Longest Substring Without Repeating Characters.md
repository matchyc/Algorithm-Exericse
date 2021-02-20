# Longest Substring Without Repeating Characters - 最长无重复字串

## 滑动窗口 (计算机网络 ？ 2333


**LeetCode 3 最长无重复字串**

## 题目

给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。
Given a string s, find the length of the longest substring without repeating characters.

Link:[https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/]

```
示例 1:

输入: s = "abcabcbb"
输出: 3
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

示例 2:

输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
示例 3:

输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
示例 4:

输入: s = ""
输出: 0
```

## 思路1：
```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {

        int maxLen = 0;
        int nLen = 0;
        unordered_map<char,int> mp; // key-value 
        for(int i = 0; i < s.size(); ++i) {
            for(int j = i; j < s.size(); ++j) {

                if(mp.find(s[j]) == mp.end()) {
                    mp[s[j]] = j;
                    nLen++;               
                    maxLen = max(nLen, maxLen);
                }
                else {
                    nLen = 0;
                    mp.erase(mp.begin(), mp.end());
                    break;
                }
            }
        }
        return maxLen;
    }
}

```

## 思路2：
```c++

class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int maxlen = 0;
        int len = 0;
        int j = 0; // 左边窗口
        unordered_map<char,int> mp;
        unordered_map<char,int>::iterator it;
        for(int i = 0; i < s.size(); ++i) {
            it = mp.find(s[i]);
            if(it == mp.end()) {
                len += 1;
                mp.insert({s[i], i});
            }
            else if(j <= it->second){
                j = it->second + 1;
                it->second = i;
                len = i - j + 1;
            }
            else {
                ++len;
                mp[s[i]] = i;
            }
            maxlen = max(len,maxlen);
        }
        return maxlen;
    }
};
```

## 思路3：
```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        // I'm going to look up cpp reference about "set", especially its methods.
        // learning...
        // done.
        std::unordered_set<char> pack;
        int maxLen = 0;
        int l = 0, r = 0;

        for(;r < s.size();) {
            if(pack.find(s[r]) == pack.end()) {
                pack.emplace(s[r]);
            }
            else {
                while(s[l] != s[r]) {
                    pack.erase(s[l++]);
                }
                ++l;
            }
            maxLen = max(r - l + 1, maxLen);
            ++r;
        }
        return maxLen;
    }   
};

```



