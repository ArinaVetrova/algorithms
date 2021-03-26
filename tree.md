- TREE

-Same tree
https://leetcode.com/problems/same-tree/
Проверить, одинаковы деревья или нет

Recursive:
```
  class Solution {
  public:
      bool isSameTree(TreeNode* p, TreeNode* q) {
          if(p==nullptr && q==nullptr) return true;
          if(p==nullptr || q==nullptr) return false;
          if(q->val!=p->val) return false;
          return isSameTree(p->left,q->left) && isSameTree(p->right,q->right);
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
