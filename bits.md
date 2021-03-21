- Single Number
https://leetcode.com/problems/single-number/
Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.
Concept

If we take XOR of zero and some bit, it will return that bit
a⊕0=a
If we take XOR of two same bits, it will return 0
a⊕a=0
 b = a⊕b⊕a=(a⊕a)⊕b=0⊕b=b
So we can XOR all bits together to find the unique number.
```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int a = 0;
        for (int i: nums)
            a^=i;
        return a;
    }
};
```
Time complexity : O(n)

Space complexity : O(1)

- Sum of Two Integers
https://leetcode.com/problems/sum-of-two-integers/

Given two integers a and b, return the sum of the two integers without using the operators + and -.
Объяснение, как работает сложение на уровне битовых операций:
https://www.geeksforgeeks.org/add-two-numbers-without-using-arithmetic-operators/

Итеративное
```
public int getSum(int a, int b) {
    int carry; //carry - это перенос остатка на уровен выше
    while (b != 0) {
        carry = a & b;
        a = a ^ b;
        b = carry << 1;
    }
    return a;
}
```
Рекурсивное
```
int Add(int x, int y)
{
    if (y == 0)
        return x;
    else
        return Add( x ^ y, (x & y) << 1);
}
```

- Number of 1 Bits
https://leetcode.com/problems/number-of-1-bits/
Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).
```
int hammingWeight(int n) {
    int count = 0;
    for (int i = 0; i < 32; i++) {
        count += (n & (1 << i)) != 0 ? 1 : 0;
    }
    return count;
}
```

- Counting Bits
https://leetcode.com/problems/counting-bits/

Given an integer num, return an array of the number of 1's in the binary representation of every number in the range [0, num]

Input: num = 5
Output: [0,1,1,2,1,2]
Explanation:
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101

```
vector<int> countBits(int num) {
        vector<int> ans;
        for(int i=0;i<=num;i++){
            int curr_num=i;
            int count=0;
            while(curr_num){
			//take and bitwise with one to check if last bit is one or not
                if(curr_num & 1)
                    count++;
			//now right shift current number by one position to form a new number 
			// and do this until your current number becomes 0
                curr_num=curr_num>>1;
            }
            ans.push_back(count);
        }
        return ans;
    }
```

- Missing Number
https://leetcode.com/problems/missing-number/

Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

Intuition

If nums were in order, it would be easy to see which number is missing.

Algorithm

??? У этой задачи есть более удобные решения, принципиально решать через непонятные манипуляции битами?

- Reverse Bits
https://leetcode.com/problems/reverse-bits/

Reverse bits of a given 32 bits unsigned integer.

```
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        uint32_t ans = 0;
        for (int i = 0; i < 32; ++i)
            if (n & (1 << i))   ans += (1 << (31-i));
        return ans;
    }
};
```
Quick Explanation
Step 1. Initialise a variable ans=0
Step 2. Run a loop from 0 to 31
Step 3. For each i in loop check whether the i th bit of n is 1 or 0
Step 4. If i th bit is 1 then add power(1,31-i) to the ans
Step 5. Return the ans.
