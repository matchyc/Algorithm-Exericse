# 关于二叉树的前、中、后、层序遍历

**leetcode 94 二叉树的中序遍历为例子**  
**题目**  
Link [二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

> 94. 二叉树的中序遍历
>     给定一个二叉树，返回它的中序 遍历.
>     示例:
>     输入: [1,null,2,3]
>     输出: [1,3,2]  
>     进阶: 递归算法很简单，你可以通过迭代算法完成吗？

---

递归算法不再讨论，在此叙述非递归算法

## 中序遍历（本题）

1. 核心思想:  
   中序遍历的本质仅仅是将根节点放在中间位置遍历。有一句话非常的精髓，它竟在非递归遍历和递归遍历中都能使用：**每一个节点都可以是根节点**。选定一棵树，一路下到最左端，那么最左下方的这个节点就是第一个被遍历的节点，之后会是哪一个节点？之后必然是这一个节点的右边的节点。*试想一路下到最左端的情景，是否有可能其右边还有分支？*然后继续遍历其右子树，形成了有某种递归意义的迭代处理。而那些经过的、还未被遍历的节点，放在栈中保存是一个非常好的选择（**遍历后直接出栈实现了丢弃**）
2. 代码实现：  
   2.1 实现 1

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

这种方法在 while 循环中嵌套了一个 while 循环来进行指针向左下方移动的操作，在 t 为空的时候读取了栈顶**栈顶是最后一个 t 不为空的指针指向的节点**，读取该节点后写入遍历数组，然后走向其右子树，对应我前面所解释的。  
**_重点需要注意的是外层 while 的判断条件中具有 || t 这个条件_**，||t 保证了在栈为空（比如最根的根节点被 pop 出来时栈就为空了）后，也可以继续 while 循环。

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

少用了一个 while，其实本质是多了一个 else 让外层 while 完成指针左下端移动的功能。

//todo:前序遍历、层序遍历（标记层号）、后序遍历

## 前序遍历

前序遍历的顺序是： 根 - 左 - 右

我们的思想，首先一路向左下（↙），那么，我们总是遇到根节点然后再遇到其左子树。完全符合前序遍历的顺序。顺序压栈。
当走到最下面时，要做的就是看看离目前访问的节点有没有右子树，如果有，我们就把右子树当做一个新的树继续访问，如果没有，那么意味着 **以当前节点为父节点的，左右子树，以及当前节点，都已经被遍历了** 。 这个节点就会被弹出栈外，我们得到栈中次顶层的节点。 **需要注意的是，栈中的节点都是访问过的，我们把它们压入栈中，只是为了在我们返回时，得到他们的右子树而已**，所以说我们每得到一个栈中的节点，我们就需要去寻找他们的右子树~~~

```C++

class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if(!root)
            return vector<int>(0);

        vector<int> ans; // 答案
        stack<TreeNode*> s; // 解释中提到的栈，保存访问过的节点。
        TreeNode* t = root;

        // 同理， while中的 t 保证了当根节点出栈时，右子树还没有被遍历的情况，让右子树也可以被继续遍历。
        while(t || !s.empty()) {
            // while 一路向下，左下，↙。 在向下的过程中就访问了。
            while(t) {
                s.push(t);
                ans.push_back(t->val);
                t = t->left;
            }
            //下到最低的时候，有人会问，发生肾魔事了？
            //根据之前的解释，我们就拿到，每一个栈里面的节点，去得到他们的右子树。
            t = s.top();
            s.pop();
            t = t->right;
            //如果这个节点没有右子树呢？？？
            //那上面的while循环就不会起作用了，继续拿下一个栈里面的节点。
            //只要拿到了右子树，就重复大while的过程，右子树，也是树！
        }
        return ans;
    }
};


```


## 后序遍历

```c++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node->val);
            if (node->left) st.push(node->left); // 相对于前序遍历，这更改一下入栈顺序 （空节点不入栈）
            if (node->right) st.push(node->right); // 空节点不入栈
        }
        reverse(result.begin(), result.end()); // 将结果反转之后就是左右中的顺序了
        return result;
    }
};
```