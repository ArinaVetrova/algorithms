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

- Invert Binary Tree
https://leetcode.com/problems/invert-binary-tree/

Given the root of a binary tree, invert the tree, and return its root.
Recirsive:
Сама решила с первой попытки ^^
```
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (!root)
            return nullptr;
        TreeNode* left = root->left;
        TreeNode* right = root->right;
        root->left = right;
        root->right = left;
        
        invertTree(left);
        invertTree(right);
        
        return root;
    }
};
```

Iterative:
через стек
```
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (!root) return root;
        stack<TreeNode*> s;
        s.push(root);
        while (!s.empty()) {
            TreeNode* node = s.top();
            s.pop();
            if (node) {
                s.push(node->left);
                s.push(node->right);
                swap(node->left, node->right);
            }
        }
        return root;
    }
};
```

- Path Sum
https://leetcode.com/problems/path-sum/

Given the root of a binary tree and an integer targetSum, return true if the tree has a root-to-leaf path such that adding up all the values along the path equals targetSum.

```
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if (!root)
            return false;
        targetSum -= root->val;
        if (targetSum == 0 && !root->left && !root->right)
            return true;
           
        return (hasPathSum(root->left, targetSum) || hasPathSum(root->right, targetSum));
        
    }
};
```


