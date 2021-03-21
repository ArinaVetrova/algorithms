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


- Group Anagrams
https://leetcode.com/problems/group-anagrams/

Можно сортировать каждую строку и использовать отсортированный вид для ключа в словаре. В итоге в значениях будут анаграммы.

Зная, что алфавит состоит только из 26 символов, можно ускорить решение. Будем подсчитывать в каждой строке частоту встречаемости каждой буквы, а потом из этих частот формировать ключ для словаря.
```
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string> > vRes;
        
        unordered_map<string, vector<string> > anagramMap;
        for(auto &s : strs)
        {
            auto word = s;
            sort(s.begin(), s.end());
            anagramMap[s].push_back(word);
        }
        
        for(const auto &anagram : anagramMap)
        {
            vRes.push_back(anagram.second);
        }
        return vRes;
    }
};
```

- Valid Parentheses
https://leetcode.com/problems/valid-parentheses/
Как: c помощью стека. На все открывающие скобки кладем в стек соотв. ей закрывающую и потом, когда идут закрывающие, вытаскиваем верхушку стека и сравниваем с текущей скобкой и так до конца послед-ти.

class Solution {
public:
    
    bool isValid(string s) {
        stack<char> stk;
        for (auto& c : s)
        {
            switch(c) {
                case '(': stk.push(')'); break; 
                case '[': stk.push(']'); break; 
                case '{': stk.push('}'); break; 
                default: 
                    if (stk.empty() or stk.top() != c) return false; 
                    stk.pop(); 
            }
        }
        return stk.empty();
    }
};

- Generate Parentheses
https://leetcode.com/problems/generate-parentheses/

- Рекурсивное решение
class Solution {
public:
    vector <string> answer;
    
    void generateBrackets(int left, int right, int n, string current) {
        if (right > left || left > n) {return;}        
        
        if (right == left && right == n) {
            answer.push_back(current);
            return;
        }
        
        generateBrackets(left+1, right, n, current+"(");
        generateBrackets(left, right+1, n, current+")");
        
    }
    
    vector<string> generateParenthesis(int n) {
        generateBrackets(0,0,n,"");
        return answer;
    }
};

Time Complexity : O(2^{2n}n)O(2 
2n
 n). For each of 2^{2n}2 
2n
  sequences, we need to create and validate the sequence, which takes O(n)O(n) work.

Space Complexity : O(2^{2n}n)O(2 
2n
 n). Naively, every sequence could be valid. See Approach 3 for development of a tighter asymptotic bound.
 
 - Итеративное решение
 ???
 
 - Valid Palindrome
 https://leetcode.com/problems/valid-palindrome/
 
 The idea is to start with two pointers i and j placed at the beginning and end of the string respectively.We need to move our pointers in case it is not alphanumeric but once we find both pointers at a position when both are alphanumeric we check for the palindrome condition i.e s[i]==s[j] and then again update i and j.
we keep on repeating the process until i<j.

isalnum - Макрос isalnum() возвращает ненулевое значение, если его аргумент является либо буквой ал­фавита (верхнего или нижнего регистра), либо цифрой. Если символ не является буквенно-циф­ровым, возвращается 0.

```
class Solution {
public:
    bool isPalindrome(string s) {
        int i=0,j=s.size()-1;
        while(i<j){
                 if(!isalnum(s[i]))i++;
                 if(!isalnum(s[j]))j--;
             if(isalnum(s[i])&&isalnum(s[j])){
                 if(tolower(s[i])!=tolower(s[j]))return false;
                 i++;
                 j--;
             }
        }
        return true;
    }
};
```

- Longest Palindromic Substring
https://leetcode.com/problems/longest-palindromic-substring/
Given a string s, return the longest palindromic substring in s.

In fact, we could solve it in O(n^2)O(n 
2
 ) time using only constant space.

We observe that a palindrome mirrors around its center. Therefore, a palindrome can be expanded from its center, and there are only 2n - 12n−1 such centers.

You might be asking why there are 2n - 12n−1 but not nn centers? The reason is the center of a palindrome can be in between two letters. Such palindromes have even number of letters (such as "abba") and its center are between the two 'b's.

```
 string longestPalindrome(string s) {
        if(s.length()==0) return "";
        int start = 0, end = 0;
        for(int i=0; i<s.length(); i++) {
            int len = max(expandAroundCenter(s, i, i), expandAroundCenter(s, i, i+1));
            if(len > (end-start)) {
                start = i - ((len-1)/2);
                end = i + (len/2);
            }
        }
        return s.substr(start, end-start+1);
    }
    
    int expandAroundCenter(string s, int left, int right) {
        if(s.length()==0 || left>right) return 0;
        while(left>=0 && right<s.length() && s[left]==s[right]) left--, right++;
        return right-left-1;
    }
```

- Palindromic Substrings
https://leetcode.com/problems/palindromic-substrings/

Given a string, your task is to count how many palindromic substrings in this string.

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

Решение в лоб
```
class Solution {
    bool isPalindrome(const string& s, int lo, int hi) {
        while (lo < hi) {
            if (s[lo] != s[hi])
                return false;

            ++lo;
            --hi;
        }

        return true;
    }

 public:
    int countSubstrings(string s) {
        int ans = 0;

        for (int lo = 0; lo < s.size(); ++lo)
            for (int hi = lo; hi < s.size(); ++hi)
                ans += isPalindrome(s, lo, hi);

        return ans;
    }
};
```
Time: O(N^3)
Space:  O(1)

```
class Solution {
public:
        int countSubstrings(string s) {
        if(s.length()==0) return 0;
        int res= 0;
        for(int i=0; i<s.length(); i++) {
            expandAroundCenter(s, i, i, res);
            expandAroundCenter(s, i, i+1, res);
        }
        return res;
    }
    
    int expandAroundCenter(string s, int left, int right, int& res) {
        if(s.length()==0 || left>right) return 0;
        while(left>=0 && right<s.length() && s[left]==s[right]) left--, right++, res++;
        return res;
    }
};
```
Time Complexity: O(N^2)

-Is Subsequence
https://leetcode.com/problems/is-subsequence/
Given two strings s and t, check if s is a subsequence of t.

```

sarahCODER's avatar
sarahCODER
72
Last Edit: June 9, 2020 7:03 PM

597 VIEWS

class Solution {
public:
    bool isSubsequence(string s, string t) {
        int ps=0, pt=0;
        while(ps<s.size() && pt<t.size()){
           if(s[ps]==t[pt]) ps++;
            pt++;
        }
        return ps==s.size();
    }
};
```
```
