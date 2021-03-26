- TREE
```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
 ```

- Maximum Depth of Binary Tree
https://leetcode.com/problems/maximum-depth-of-binary-tree/

Recursive:
```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        return !root ? 0 : max(maxDepth(root->right), maxDepth(root->left))+1;
    }
};
```

- Same tree
https://leetcode.com/problems/same-tree/
Проверить, одинаковы деревья или нет

Recursive:
```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (!root)
            return 0;
        else
            return 1+max(maxDepth(root->left), maxDepth(root->right));
    }
};
```

Iterative:  - имитация рекурсии с помощью стека
```
class Solution {
  public:
      bool isSameTree(TreeNode* p, TreeNode* q) {
          stack<TreeNode *> st;
          st.push(p);
          st.push(q);
          while (st.size()!=0){
              TreeNode * q2=st.top();
              st.pop();
              TreeNode * q1=st.top();
              st.pop();
              if (q1==nullptr && q2==nullptr) continue;
              if (q1==nullptr || q2==nullptr) return false;
              if (q1->val!=q2->val) return false;
              st.push(q1->left);
              st.push(q2->left);
              st.push(q1->right);
              st.push(q2->right);
          }
          return true;
      }
  };
```
