# 链表part02
**题目: 24.两两交换连链表中的节点**  
**位置:** https://leetcode.cn/problems/swap-nodes-in-pairs/  
```java
public ListNode swapPairs(ListNode head) {
    if (head == null || head.next == null) return head;

    ListNode first = head;      // 老大
    ListNode second = head.next; // 老二

    // 先让后面的人换，换完的结果给 res
    ListNode res = swapPairs(second.next);

    // 开始交换当前这一对
    second.next = first;
    first.next = res;

    return second; // 返回交换后的新头
}
```
**题目: 19.删除链表的倒数第N个结点**  
**位置:** https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/  
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // 删除倒数第n个,需要获取其前一个位置,并且倒数第n个还可能是头节点
        ListNode dummy = new ListNode(0,head);
        ListNode fast = dummy;
        ListNode slow = dummy;
        for(int i=0;i<n;i++){
            fast=fast.next;
        }
        while(fast.next!=null){
            slow = slow.next;
            fast = fast.next;
        }
        slow.next = slow.next.next;
        return dummy.next;
    }
}
```
**题目: 02.07.链表相交**  
**位置:** https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/description/  
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode p1 = headA;
        ListNode p2 = headB;
        while(p1!=p2){
            if(p1==null){
                p1=headB;
            }else{
                p1=p1.next;
            }


            if(p2==null){
                p2 = headA;
            }else{
                p2=p2.next;
            }
        }
        return p1;
    }
}
```
**题目: 142.环形链表**  
**位置:** https://leetcode.cn/problems/linked-list-cycle-ii/description/  
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while(fast!=null&&fast.next!=null){// 关键点1. 通过fast是否等于null来判断是否有环,而不是fast!=slow来判断
                                           // fast!=slow 对于头节点不符合
                                           // 如果单独处理一次? 又有可能存在null
            slow=slow.next;
            fast = fast.next.next;
            if(fast==slow){
                slow = head;
                while(fast!=slow){
                    fast=fast.next;
                    slow=slow.next;
                }
                return slow;
            }
        }
        return null;
    }
}
```
