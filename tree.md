- TREE
Принцип, как решать задачи на деревья рекурсивно:
	https://leetcode.com/explore/learn/card/data-structure-tree/17/solve-problems-recursively/534/

- Принцип BFS (обхода в ширину) с помощью очереди
	https://leetcode.com/explore/learn/card/queue-stack/231/practical-application-queue/1372/
	
- Принцип DFS (Обход в глубину) с помощью стека
	https://leetcode.com/explore/learn/card/queue-stack/232/practical-application-stack/1377/
	Шаблон рекурсии:
	https://leetcode.com/explore/learn/card/queue-stack/232/practical-application-stack/1384/
	Шаблон итеративный со стеком:
	https://leetcode.com/explore/learn/card/queue-stack/232/practical-application-stack/1385/

- Принцип, как решать задачи на деревья итеративно с помощью очереди:
	https://leetcode.com/explore/learn/card/data-structure-tree/134/traverse-a-tree/990/

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

Iterative:
```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        int depth = 0;
        if (!root) return depth;
        queue<TreeNode*> level;
        level.push(root);
        while (!level.empty()) {
            depth++;
            int n = level.size();
            for (int i = 0; i < n; i++) {
                TreeNode* node = level.front();
                level.pop();
                if (node -> left) level.push(node -> left);
                if (node -> right) level.push(node -> right);
            }
        }
        return depth; 
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

- Binary Tree Level Order Traversal
https://leetcode.com/problems/binary-tree-level-order-traversal/

Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]

```
class Solution {
public:
vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if(root==NULL) return ans;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty())
        {
            vector<int> level;
            int n=q.size();
            for(int i=0;i<n;i++)
            {
            TreeNode* current = q.front();
            level.push_back(current->val);
            if(current->left!=NULL)
                q.push(current->left);
            if(current->right!=NULL)
                q.push(current->right);
            q.pop();
            }
            ans.push_back(level);
        }
       return ans; 
    }
};
```

- Tree Paths
https://leetcode.com/problems/binary-tree-paths/

```
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> result;
        insertPaths(root, "", &result);
        return result;
    }
private:
	// simple recursive pre-order traversal
    void insertPaths(TreeNode* node, string str, vector<string>* res) {
        if (!node) return;  // base-case
        
        str += to_string(node->val);
        if (!node->left && !node->right) {
			// if the current node is a leaf, add string to result
            res->emplace_back(str);
        }
        
        insertPaths(node->left, str + "->", res);
        insertPaths(node->right, str + "->", res);
    }
};
```

- Найти все пути дерева
(не на leetcode, но точно понадобится для других задач)

Рекурсивно:

```
void collectAllPaths(TreeNode* root, std::vector<TreeNode*>& path, std::vector<std::vector<TreeNode*>>& res)
{
    if (!root)
        return;

    path.push_back(root);
    if (!root->leftChild && !root->rightChild)
        res.push_back(path);

    if (root->leftChild)
        collectAllPaths(root->leftChild, path, res);

    if (root->rightChild)
        collectAllPaths(root->rightChild, path, res);

    path.pop_back();
 }
 ```
 
- Binary Tree Level Order Traversal
https://leetcode.com/problems/binary-tree-level-order-traversal/

Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

```
class Solution {
public:
vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if(root==NULL) return ans;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty())
        {
            vector<int> level;
            int n=q.size();
            for(int i=0;i<n;i++)
            {
            TreeNode* current = q.front();
            level.push_back(current->val);
            if(current->left!=NULL)
                q.push(current->left);
            if(current->right!=NULL)
                q.push(current->right);
            q.pop();
            }
            ans.push_back(level);
        }
       return ans; 
    }
};
```	

- Subtree of Another Tree
https://leetcode.com/problems/subtree-of-another-tree/

Given two non-empty binary trees s and t, check whether tree t has exactly the same structure and node values with a subtree of s. 
A subtree of s is a tree consists of a node in s and all of this node's descendants. The tree s could also be considered as a subtree of itself.
 s:
     3
    / \
   4   5
  / \
 1   2
 
 t:
   4 
  / \
 1   2
 
 Return true, because t has the same structure and node values with a subtree of s.
 
```
class Solution {
public:
    bool ans=false;
    
    bool sametree(TreeNode* s, TreeNode* t)
    {
        if(s==NULL&&t==NULL) return true;
        if(s==NULL&&t!=NULL) return false;
        if(s!=NULL&&t==NULL) return false;

        
        if(s->val==t->val && sametree(s->left,t->left) && sametree(s->right,t->right))
           return true;
           
           return false;

    }
    
    void preorder(TreeNode* s, TreeNode* t)
    {
        if(s==NULL)
            return;
        
        if(s->val==t->val)
        {
        bool val=sametree(s,t);
        if(val==true)
            ans=true;   
        }
        preorder(s->left,t);
        preorder(s->right,t);
    }
    
    
    bool isSubtree(TreeNode* s, TreeNode* t) {
        
        if(s==NULL&&t==NULL) return true;
        if(s==NULL&&t!=NULL) return false;
        if(s!=NULL&&t==NULL) return false;

        
       preorder(s,t); 
        
        return ans;
    }
};
```

- Construct Binary Tree from Preorder and Inorder Traversal
https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/

Given two integer arrays preorder and inorder where preorder is the preorder traversal of a binary tree and inorder is the inorder traversal of the same tree, construct and return the binary tree.

 
```

```
