# 无重复最长字串

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例  1:

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
解释: 因为无重复字符的最长子串是  "wke"，所以其长度为 3。
  请注意，你的答案必须是 子串 的长度，"pwke"  是一个子序列，不是子串。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-substring-without-repeating-characters
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 考虑

应该是一个所谓“滑动窗口”的问题，我已经有一个题解，在该 repository 中。 I already had a solution for this question in the repository. 但是过去已久，兴致来了，再写一次。

我记得我有一个很不好的方法，每一次都要 erase 所有 unordered_map 中的键值对，并且有一个使用 set 的解法，我很乐意尝试其中一种，然后去查看另外的解法。

需要说明的是，下面的解法是思路3.

**滑动窗口**，把已有的字符放到一个set里，窗口不断向右延展，若遇到了set中有的，窗口的左边界不断向右收缩，直到删掉那个重复的字符，最大长度每一次都进行更新。其实我感觉只用在延展的地方更新（事实证明是这样的）。

```C++
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