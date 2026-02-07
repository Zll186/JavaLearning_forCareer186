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
//原链表删除
public ListNode removeElements(ListNode head, int val) {
    //为什么用 while 而不是 if？ 因为可能存在连续多个节点都需要删除，while 会一直删到第一个“干净”的节点为止。
    while(head!=null && head.val==val) {
        head = head.next;
    }
    ListNode curr = head;// 创建一个临时指针 curr，用来遍历链表
    while(curr!=null && curr.next !=null) {
        if(curr.next.val == val){
            curr.next = curr.next.next;
        } else {
            curr = curr.next;
        }
    }
    return head;
}
//虚拟头节点
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummy = new ListNode(0,head); //原本的 head 就变成了 dummy.next
                                               // 一般只要涉及修改链表都得需要加
        ListNode curr = dummy;// 让 cur 从虚拟头节点开始
        while(curr.next!=null){ //始终站在“待删除节点”的前一个位置
            if (cur.next.val == val) {
            cur.next = cur.next.next;
        } else {
            cur = cur.next;        
        }
    }
    return dummy.next;
   }
}
```
**题目: 707.设计链表**  
**位置:** https://leetcode.cn/problems/design-linked-list/description/  
这道题目设计链表的五个接口：
- 获取链表第index个节点的数值
- 在链表的最前面插入一个节点
- 在链表的最后面插入一个节点
- 在链表第index个节点前面插入一个节点
- 删除链表的第index个节点
**思路**虚拟头节点

```java
//单链表
class MyLinkedList {
    class ListNode {
        int val;
        ListNode next;
        ListNode(int val) {
            this.val=val;
        }
    }
    private int size; //size存储链表元素的个数
    private ListNode head;  //注意这里记录的是虚拟头结点
    
    //初始化链表
    public MyLinkedList() {
        this.size = 0;
        this.head = new ListNode(0);
    }

    //获取第index个节点的数值，注意index是从0开始的，第0个节点就是虚拟头结点
    public int get(int index) {
        //如果index非法，返回-1
        if (index < 0 || index >= size) {
            return -1;
        }
        ListNode cur = head;
        //第0个节点是虚拟头节点，所以查找第 index+1 个节点
        for (int i = 0; i <= index; i++) {
            cur = cur.next;
        }
        return cur.val;
    }
    //在链表的最前面插入一个节点
    public void addAtHead(int val) {
        ListNode newNode = new ListNode(val);
        newNode.next = head.next;
        head.next = newNode;
        size++;
        // 在链表最前面插入一个节点，等价于在第0个元素前添加
        // addAtIndex(0, val);
    }

    //在链表的最后面插入一个节点
    public void addAtTail(int val) {
        ListNode newNode = new ListNode(val);
        ListNode cur = head;
        while (cur.next != null) {
            cur = cur.next;
        }
        cur.next = newNode;
        size++;
    }

    // 在第 index 个节点之前插入一个新节点，例如index为0，那么新插入的节点为链表的新头节点。
    // 如果 index 等于链表的长度，则说明是新插入的节点为链表的尾结点
    // 如果 index 大于链表的长度，则返回空
    public void addAtIndex(int index, int val) {
        if (index < 0 || index > size) {
            return;
        }
        //找到要插入节点的前驱
        ListNode pre = head;
        for (int i = 0; i < index; i++) {
            pre = pre.next;
        }
        ListNode newNode = new ListNode(val);
        newNode.next = pre.next;
        pre.next = newNode;
        size++;
    }

    //删除链表的第index个节点
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }
        
        //因为有虚拟头节点，所以不用对index=0的情况进行特殊处理
        ListNode pre = head;
        for (int i = 0; i < index ; i++) {
            pre = pre.next;
        }
        pre.next = pre.next.next;
        size--;
    }
}

```
**题目: 206.翻转链表**  
**位置:** https://leetcode.cn/problems/reverse-linked-list/description/  
```java
// 这道题目经常用作其他题目的一个方法，最好直接记住，能够不假思索写出来
//双指针法
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode cur = head;
        ListNode temp = null;
        while (cur != null) {
            temp = cur.next;// 保存下一个节点
            cur.next = prev;
            prev = cur;
            cur = temp;
        }
        return prev;
    }
}

```

## little tips
- **无参构造方法**（创建节点时不传值）。这样写的好处：你可以先 new ListNode() 再手动给 val、next 赋值。
- **删除节点**ABC要删B，只要将A的指针指向C即可，如果是C++需要手动释放B的内存，而Java、Python，就有自己的内存回收机制，就不用自己手动释放
- 链表的增添和删除都是O(1)操作，查找的时间复杂度是O(n)
- 数组的增添和删除都是O(n)操作，查找的时间复杂度是O(1)
