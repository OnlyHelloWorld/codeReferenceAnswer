出现在:腾讯

链表两数相加    练习地址:https://leetcode-cn.com/problems/add-two-numbers/submissions/

需要注意:
1,用int表示是否进位,技能起标志作用,又能参与计算

2,别忘了最后两数相加如果进位则新增一个节点值为1的节点放到最后

这道题leetcode给出的解答是:

![image](https://github.com/OnlyHelloWorld/codeReferenceAnswer/blob/master/images/leetcode02.png)

具体代码实现:
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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
//------------------------------------------------------------------------
        //这段代码没必要写,但是这种验证以后要经常注意,写了也不错
        if(l1 == null && l2 == null)
            return null;
//------------------------------------------------------------------------
        ListNode res = new ListNode(-1);
        ListNode cur = res;
        int carry = 0;
        while(l1 != null || l2 != null){
            int x = l1 != null ? l1.val : 0;
            int y = l2 != null ? l2.val : 0;
            //carry等于0或1都可以直接参与运算
            int sum = x + y + carry;
            carry = sum/10;
            cur.next = new ListNode(sum%10);
            cur = cur.next;
            if(l1 != null) l1 = l1.next;
            if(l2 != null) l2 = l2.next;
        }
        //注意最后两数相加是否进位
        if(carry > 0)
            cur.next = new ListNode(1);
        return res.next;
        
    }
}
```
