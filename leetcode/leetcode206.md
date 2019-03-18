出现在:腾讯,字节跳动等

反转一个单链表。    练习地址:https://leetcode-cn.com/problems/reverse-linked-list/

迭代和递归两种方式,都建议画图理解,高频率,必须会的

首先迭代:
理解好辅助头结点的作用以及变化
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null)
            return head;
        //设一个辅助头结点
        ListNode newNode = new ListNode(-1);
        while(head != null){
            ListNode next = head.next;
            //将未反转头结点指向已反转完成部分的头结点,此时未反转头结点称为已反转头结点
            head.next = newNode.next;
            //让辅助头结点指向新的已反转头结点
            newNode.next = head;
            head = next;
        }
        return newNode.next;
    }
}
```

递归方式:
理解好,递归结束时状态,不要忘了每次递归把head.next = null
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null)
            return head;
        //返回反转完成部分的头结点
        ListNode headNode = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return headNode;
    }
}
```
