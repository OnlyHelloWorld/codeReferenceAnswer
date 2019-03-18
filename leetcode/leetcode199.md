出现在:腾讯,字节跳动

给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。面试官说法是:打印二叉树每层最右边的节点
练习地址:https://leetcode-cn.com/problems/binary-tree-right-side-view/

思路:使用队列进行层次遍历,标记当前层节点数目和下一层节点数目  弹出一个节点则当前层节点数目减1,进入一个子节点,下一层数目加1,当前层数目为0时
则此次弹出的为最右边的节点,更新当前层数目为下一层,下一层数目为0.

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
    public List<Integer> rightSideView(TreeNode root) {
        //存放结果集
        List<Integer> res = new LinkedList<>();
        if(root == null)
            return res;
        Queue<TreeNode> queue = new LinkedList<>();
        int cur = 1;              //指当前层数节点个数
        int next = 0;             //指下一层节点个数
        TreeNode node;
        queue.offer(root);
        while(!queue.isEmpty()){
            node = queue.poll();
            cur--;
            //添加非空子节点,并将下一层节点数加1            
            if(node.left != null){
                queue.offer(node.left);
                next++;
            } 
            if(node.right != null){
                queue.offer(node.right);
                next++;
            }
            //cur等于0说明这次弹出的是当前层最右的节点,加入结果集,更新cur和next
            if(cur == 0){
                res.add(node.val);
                cur = next;
                next = 0;
            }
        }
        return res;
    }
}
```

字节跳动考察的是打印二叉树最左边的节点,我们只需要添加子节点是先添加右节点即可

代码实现(未经过太多实例测试):
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
    public List<Integer> rightSideView(TreeNode root) {
        //存放结果集
        List<Integer> res = new LinkedList<>();
        if(root == null)
            return res;
        Queue<TreeNode> queue = new LinkedList<>();
        int cur = 1;              //指当前层数节点个数
        int next = 0;             //指下一层节点个数
        TreeNode node;
        queue.offer(root);
        while(!queue.isEmpty()){
            node = queue.poll();
            cur--;
            //添加非空子节点,并将下一层节点数加1            
            if(node.right != null){
                queue.offer(node.right);
                next++;
            } 
            if(node.left != null){
                queue.offer(node.left);
                next++;
            }
            //cur等于0说明这次弹出的是当前层最右的节点,加入结果集,更新cur和next
            if(cur == 0){
                res.add(node.val);
                cur = next;
                next = 0;
            }
        }
        return res;
    }
}
```
