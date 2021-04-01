# Общие алгоритмы, которые считаю, надо знать и не влезли в другие категории

- Binary search
https://leetcode.com/problems/binary-search/submissions/
```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if (nums.empty())
            return -1;
        size_t n = nums.size(); 
        int left = 0, right = n-1;
        int pivot = 0;
        while (left<=right)
        {
            pivot = left + (right-left)/2;
            if (nums[pivot] == target)
                return pivot;
            else if (nums[pivot] < target)
                left = pivot + 1;
            else
                right = pivot - 1;
        }
        return -1;
    }
};
```
 Time: O(logN)
 Space: O(1)
 
 
