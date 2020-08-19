# 剑指 Offer 28. 对称的二叉树（简单）

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

    1
   / \
  2   2
 / \ / \
3  4 4  3
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

    1
   / \
  2   2
   \   \
   3    3

示例 1：

    输入：root = [1,2,2,3,4,4,3]
    输出：true

示例 2：

    输入：root = [1,2,2,null,3,null,3]
    输出：false

限制：

0 <= 节点个数 <= 1000

### 递归
```c++
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
    bool isSymmetric(TreeNode* root) {
        if(root == NULL){return true;}
        return helper(root->left, root->right);
    }

    bool helper(TreeNode* a, TreeNode* b){
        if(a == NULL && b == NULL){return true;}
        if(a == NULL || b == NULL){return false;}
        return a->val == b->val && helper(a->left, b->right) && helper(a->right, b->left);
    }
};
```

### 队列
```c++
public boolean isSymmetric(TreeNode root) {
    //队列
    Queue<TreeNode> queue = new LinkedList<>();
    if (root == null)
        return true;
    //左子节点和右子节点同时入队
    queue.add(root.left);
    queue.add(root.right);
    //如果队列不为空就继续循环
    while (!queue.isEmpty()) {
        //每两个出队
        TreeNode left = queue.poll(), right = queue.poll();
        //如果都为空继续循环
        if (left == null && right == null)
            continue;
        //如果一个为空一个不为空，说明不是对称的，直接返回false
        if (left == null ^ right == null)
            return false;
        //如果这两个值不相同，也不是对称的，直接返回false
        if (left.val != right.val)
            return false;
        //这里要记住入队的顺序，他会每两个两个的出队。
        //左子节点的左子节点和右子节点的右子节点同时
        //入队，因为他俩会同时比较。
        //左子节点的右子节点和右子节点的左子节点同时入队，
        //因为他俩会同时比较
        queue.add(left.left);
        queue.add(right.right);
        queue.add(left.right);
        queue.add(right.left);
    }
    return true;
}
```