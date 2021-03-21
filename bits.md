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
https://tproger.ru/problems/write-a-function-summation-of-two-numbers-without-using-the-and-other-arithmetic-operators/
```
public int getSum(int a, int b) {
    int carry;
    while (b != 0) {
        carry = a & b;
        a = a ^ b;
        b = carry << 1;
    }
    return a;
}
```

- Number of 1 Bits

```
