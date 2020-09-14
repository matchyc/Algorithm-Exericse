# 关于二叉树的前、中、后、层序遍历
**leetcode 94 二叉树的中序遍历为例子**  
**题目**  
Link [二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)  
> 94. 二叉树的中序遍历 
>给定一个二叉树，返回它的中序 遍历.
>示例:
>输入: [1,null,2,3]
>输出: [1,3,2]  
进阶: 递归算法很简单，你可以通过迭代算法完成吗？

---
递归算法不再讨论，在此叙述非递归算法
## 中序遍历（本题）
1. 核心思想:  
中序遍历的本质仅仅是将根节点放在中间位置遍历。有一句话非常的精髓，它竟在非递归遍历和递归遍历中都能使用：**每一个节点都可以是根节点**。选定一棵树，一路下到最左端，那么最左下方的这个节点就是第一个被遍历的节点，之后会是哪一个节点？之后必然是这一个节点的右边的节点。*试想一路下到最左端的情景，是否有可能其右边还有分支？*然后继续遍历其右子树，形成了有某种递归意义的迭代处理。而那些经过的、还未被遍历的节点，放在栈中保存是一个非常好的选择（**遍历后直接出栈实现了丢弃**）
2. 代码实现：  
    2.1  实现1
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        if(root == nullptr)
            return ans;
        stack<TreeNode*> s;
        s.push(root);
        TreeNode* t = s.top();
        while(!s.empty() || t) {
            while(t) {
                if(t != root)
                s.push(t);
                t = t->left;
            }
            t = s.top();
            s.pop();
            ans.push_back(t->val);
            t = t->right;
        }
        return ans;
    }
};
```  
这种方法在while循环中嵌套了一个while循环来进行指针向左下方移动的操作，在t为空的时候读取了栈顶**栈顶是最后一个t不为空的指针指向的节点**，读取该节点后写入遍历数组，然后走向其右子树，对应我前面所解释的。  
***重点需要注意的是外层while的判断条件中具有 || t这个条件***，||t保证了在栈为空（比如最根的根节点被pop出来时栈就为空了）

    2.2 实现2
```C++  
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        if(root == nullptr)
            return ans;
        stack<TreeNode*> s;
        s.push(root);
        TreeNode* t = s.top();
        while(!s.empty() || t) {
            if(t) {
                if(t != root)
                s.push(t);
                t = t->left;
            }
            else {
                t = s.top();
                s.pop();
                ans.push_back(t->val);
                t = t->right;
            }
        }
        return ans;
    }
};  
```
少用了一个while，其实本质是多了一个else让外层while完成指针左下端移动的功能。

//todo:前序遍历、层序遍历（标记层号）、后序遍历

