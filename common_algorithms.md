# Общие алгоритмы, которые считаю, надо знать и не влезли в другие категории

- Binary search
https://leetcode.com/problems/binary-search/submissions/

Initialise left and right pointers : left = 0, right = n - 1.  
    While left <= right :  
        Compare middle element of the array nums[pivot] to the target value target.  
        If the middle element is the target target = nums[pivot] : return pivot.  
        If the target is not yet found :  
        If target < nums[pivot], continue the search on the left right = pivot - 1.  
        Else continue the search on the right left = pivot + 1.  

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
 
 
