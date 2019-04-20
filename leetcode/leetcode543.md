出现在:字节跳动

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过根结点。
练习地址:https://leetcode-cn.com/problems/diameter-of-binary-tree/

思路:首先树的最值问题我们应该想到递归方法.我们需要定义一个结果值保持当前最大值并在递归中持续更新,因此我们定义为类的属性比较合适.递归出口为root==null,并且返回0.递归返回为左右子树深度最大值+1.

注意:最长路径不一定经过根节点,因此我们递归过程中实际上是求每个节点的左右子树最大深度之和(当然也包括根节点)

代码实现:
```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    int max = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        maxDepth(root);
        return max;
    }
    private int maxDepth(TreeNode root) {
        if (root == null) return 0;
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        max = Math.max(max, left + right);
        return Math.max(left, right) + 1;
    }
	
}
```
