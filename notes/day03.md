# 链表part01
## 链表理论知识
链表的类型:
1. 单链表
2. 双链表
3. 循环链表

**节点定义**  
```java
public class ListNode {
    int val;
    ListNode next;      //在 ListNode 这个类里，声明了一个字段，它的类型也是 ListNode。这叫 自引用（self-reference），用来把节点串起来。
    
    public ListNode(){}     //无参构造方法（创建节点时不传值），这样写的好处：你可以先 new ListNode() 再手动给 val、next 赋值。

    public ListNode(int val) {this.val = val;}    // 节点的构造函数(有一个参数)

    public ListNode(int val, ListNode next) {    // 节点的构造函数(有两个参数)
        this.val = val;
        this.next = next;
    }
}
``` 
**题目:203.移除链表元素**  
**位置:** https://leetcode.cn/problems/remove-linked-list-elements/description/  
```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummy = new ListNode(0,head); // 虚拟头结点,不想单独处理“头结点被操作”的各种特例时就得加
                                               // 一般只要涉及修改链表都得需要加
        ListNode curr = dummy;
        while(curr!=null&&curr.next!=null){
            ListNode tmp=curr.next.next;
            if(curr.next.val==val){
                curr.next=tmp;
            }else{// 注意这个else 如果curr.next.val==val 此时不能移动指针,因为还需要判断下一个指向的元素是不是val,一旦一定就把下个元素跳过了
                curr=curr.next;
            }
            
        }
        return dummy.next;
    }
}
```
**题目: 707.设计链表**  
**位置:** https://leetcode.cn/problems/design-linked-list/description/  
```java
class Node{
        int val;
        Node next;
        public Node(){} //无参构造
        public Node(int val,Node next){ //有参构造
            this.val = val;
            this.next = next;
        }
}
class MyLinkedList {
    // 虚拟头结点
    Node dummy;    // 注意,一定得用虚拟头结点,否则会多出很多很多麻烦
    int len;
    public MyLinkedList() {
        this.dummy=new Node(0,null);
        len =0;
    }
    
    public int get(int index) {
        Node curr =dummy.next;
        if(index<0 || index>=len) return -1;
        for(int i=0;i<index;i++){
            curr=curr.next;
        }
        return curr.val;
    }
    
    public void addAtHead(int val) {
        Node head = dummy.next;
        dummy.next = new Node(val,head);
        len++;
    }
    
    public void addAtTail(int val) {
        Node curr = dummy;
        while(curr.next!=null){
            curr=curr.next;
        }
        curr.next= new Node(val,null);
        len++;
    }
    
    public void addAtIndex(int index, int val) {
        if(index<0 || index>len) {
            return;
        }
        if(index==len){
            addAtTail(val);
            return;
        }
        if(index==0){
            addAtHead(val);
            return;
        }
        Node pre = dummy;
        for(int i=0;i<index;i++){
            pre=pre.next;
        }
        Node tmp = pre.next;
        pre.next = new Node(val,tmp);
        len++;
    }
    
    public void deleteAtIndex(int index) {
        if(index<0||index>=len){
            return;
        }
        Node pre = dummy;
        for(int i=0;i<index;i++){
            pre=pre.next;
        }
        pre.next = pre.next.next;
        len--;
    }
}

```
**题目: 206.翻转链表**  
**位置:** https://leetcode.cn/problems/reverse-linked-list/description/  
```java
// 这道题目经常用作其他题目的一个方法，最好直接记住，能够不假思索写出来
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode ans = null;
        ListNode curr=head;
        while(curr!=null){
            ListNode tmp = curr.next;
            curr.next = ans;
            ans = curr;
            curr= tmp;
        }
        return ans;

    }
}
```

## little tips
- **无参构造方法**（创建节点时不传值）。这样写的好处：你可以先 new ListNode() 再手动给 val、next 赋值。
- **删除节点**ABC要删B，只要将A的指针指向C即可，如果是C++需要手动释放B的内存，而Java、Python，就有自己的内存回收机制，就不用自己手动释放
- 链表的增添和删除都是O(1)操作，查找的时间复杂度是O(n)
- 数组的增添和删除都是O(n)操作，查找的时间复杂度是O(1)
