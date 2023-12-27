# 代码随想录算法训练营
## Day 4 ｜24. 两两交换链表中的节点 , 19.删除链表的倒数第N个节点，面试题 02.07. 链表相交, 142.环形链表II  
### 24. 两两交换链表中的节点
**给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。**
#### **思路：**
1. 用虚拟头节点来标记交换后的头节点
2. 画图来确定交换的顺序，以及循环的条件
   <img width="786" alt="image" src="https://github.com/snail-miao/Algorithms/assets/43983739/5b666702-df4d-4d68-ad31-419b84126959">

``` go
func swapPairs(head *ListNode) *ListNode {
    if head == nil{
        return nil
    }
    cur1:= head
    dummyNode := &ListNode{
        Next:cur1,
    }
    prev := dummyNode
    for prev.Next != nil && prev.Next.Next != nil {
        cur1 := prev.Next
        cur2 := prev.Next.Next
        next := cur2.Next
        prev.Next = cur2
        cur2.Next = cur1
        cur1.Next = next
        prev = cur1
    }
    return dummyNode.Next
}
```

### 19.删除链表的倒数第N个节点
**给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。**
#### 思路：
1. 虚拟头节点来标记删除后的头节点
2. 快慢指针：快指针先走N步，然后慢指针和快指针同时走，走到最后的时候，慢指针就是倒数第N+1个节点了，删除其后面的一个节点即可。
``` go
func removeNthFromEnd(head *ListNode, n int) *ListNode {
    cur := head
    dummyNode := &ListNode{
        Next:cur,
    }
    prev := dummyNode
    q := head
    for i:=0;i<n;i++{
        if q == nil{
            return dummyNode.Next
        }
        q = q.Next
    }

    for q != nil{
        q= q.Next
        prev = prev.Next
        cur = cur.Next
    }

    next := cur.Next
    prev.Next = next
    return dummyNode.Next
}
```

### 面试题 02.07. 链表相交
**给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。**
#### 思路：
1. 求出两个链表的长度，并求出两个链表长度的差值，然后让curA移动到，和curB 末尾对齐的位置。
```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    cur1 := headA
    lenA:= 0
    for cur1 != nil{
        lenA ++
        cur1 = cur1.Next
    }

    cur2 := headB
    lenB := 0
    for cur2 != nil{
        lenB ++
        cur2 = cur2.Next
    }

    curA := headA
    curB := headB
    if lenA >=lenB{
        k := lenA -lenB
        for i:=0;i<k;i++{
            curA = curA.Next
        }
    }else{
        k := lenB - lenA
        for i := 0; i<k ;i++{
            curB = curB.Next
        }
    }

    for curA != nil{
        if curA == curB{
            return curA
        } 
        curA = curA.Next
        curB = curB.Next
    }
    return nil
}
```
### 142.环形链表II  （这题没做出来）
给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

不允许修改 链表。

#### 思路：
1. 判断链表是否环，如果有环，如何找到这个环的入口
2. 用快慢指针判断是否有环：2倍速，和1倍速指针，fast指针一定先进入环中，如果fast指针和slow指针相遇的话，一定是在环中相遇，这是毋庸置疑的。因为fast是走两步，slow是走一步，其实相对于slow来说，fast是一个节点一个节点的靠近slow的，所以fast一定可以和slow重合
3. 如何找到这个环的入口：
   <img width="1004" alt="image" src="https://github.com/snail-miao/Algorithms/assets/43983739/07fd9059-c532-447d-8b1d-059e9a077b2f">

``` go
func detectCycle(head *ListNode) *ListNode {
	q := head
	p := head
	var point *ListNode
	for p != nil && q != nil && q.Next != nil {
		p = p.Next
		q = q.Next.Next
		if p == q {
			point = p
			break
		}
	}

	s := head
	for s != nil && point != nil {
		if s == point {
			return s
		}
		s = s.Next
		point = point.Next
	}
	return nil
}
```
