出现在:字节跳动

给定一个二叉树，返回它的 前序 遍历。
给定一个二叉树，返回它的 后序 遍历。
给定一个二叉树，返回它的 中序 遍历。
都采用非递归方式
练习地址:https://leetcode-cn.com/problems/binary-tree-preorder-traversal/

放在一起是因为他们太过于相似,也都是使用栈求解

代码实现:
1,前序遍历
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
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null)
            return res;
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode cur = stack.pop();
            res.add(cur.val);
            if(cur.right != null)
                stack.push(cur.right);
            if(cur.left != null)
                stack.push(cur.left);
        }
        return res;
    }
}
```

2,后序遍历
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
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null)
            return res;
        Stack<TreeNode> stack = new Stack<>();
        Set<TreeNode> visited = new HashSet<>();
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode node = stack.peek();
            visited.add(node);
            if(node.right != null && !visited.contains(node.right)){
                stack.push(node.right);
                visited.add(node.right);
                if(node.left == null) continue;
            }
            if(node.left != null && !visited.contains(node.left)){
                stack.push(node.left);
                visited.add(node.left);
                continue;
            }
            res.add(node.val);
            stack.pop();
        }
        return res;
    }
}
```

3,中序遍历
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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root;
        while(cur != null || !stack.isEmpty()){
            if(cur != null){
                stack.push(cur);
                cur = cur.left;
            }else{
                cur = stack.pop();
                res.add(cur.val);
                cur = cur.right;
            }
        }
        return res;
    }
}
```
