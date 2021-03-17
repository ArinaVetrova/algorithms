# Strings
-Longest Substring Without Repeating Characters
https://leetcode.com/problems/longest-substring-without-repeating-characters/
Given a string s, find the length of the longest substring without repeating characters.
```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_set <char> uset;
        int size=s.size();
        int start=0, end=0, res=0;
        while (start<size && end<size) {
            if (uset.find(s[end])==uset.end()) {
                uset.insert(s[end]);
                end++;
                res=max (res, end-start);
            }
            else {
                uset.erase(s[start]);
                start++;
            }
        }
    return res;
    }
};
```
