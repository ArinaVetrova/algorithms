# Strings
-Longest Substring Without Repeating Characters
https://leetcode.com/problems/longest-substring-without-repeating-characters/
Given a string s, find the length of the longest substring without repeating characters.

Sliding window
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

- Longest Repeating Character Replacement
https://leetcode.com/problems/longest-repeating-character-replacement/

Подсчитать количество вхождений наиболее часто встречающегося символа в окне, вычесть эту величину из размера окна. Тем самым мы найдем наименьшее количество замен, которые необходимы, чтобы в данном окне все символы были одинаковыми.

class Solution {
public:
    int characterReplacement(string s, int k) {
        int right = 0, left = 0, maxLetter = 0;
        int alphabet[26] = {0};
        while(right < s.length()) {
            alphabet[s[right] - 'A']++;
            maxLetter = max(maxLetter, alphabet[s[right] - 'A']);
            right++;
            
            if(right - left - maxLetter > k) {
                alphabet[s[left] - 'A']--;
                left++;
            }
        }
        return right - left;
    }
};

Если вдруг replaceCount > k, то необходимо сдвинуть окно вправо и уменьшить count. Например, есть строка ABAACA. Окно есть ABAAC, мы его сдвигаем и уменьшаем count, чтобы получить окно BAACA.

