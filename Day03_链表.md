#  代码随想录算法训练营

## Day 3 ｜203. 移除链表元素, 707. 设计链表，206.反转链表

### 203. 移除链表元素
_给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点._

**思路：**
1. 删除链表中的元素后如何 返回新的头节点？ 利用虚拟头节点。

``` go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func removeElements(head *ListNode, val int) *ListNode {
    cur:= head
    dummyNode := &ListNode{
        Next:cur,
    }
    pre := dummyNode

    for cur != nil{
        next := cur.Next
        if cur.Val == val{
            pre.Next = next
            cur = next
        }else{
            pre = cur
            cur = next
        }
    }
    return dummyNode.Next
}
```

### 707. 设计链表
_你可以选择使用单链表或者双链表，设计并实现自己的链表。
单链表中的节点应该具备两个属性：val 和 next 。val 是当前节点的值，next 是指向下一个节点的指针/引用。
如果是双向链表，则还需要属性 prev 以指示链表中的上一个节点。假设链表中的所有节点下标从 0 开始。
实现 MyLinkedList 类：
MyLinkedList() 初始化 MyLinkedList 对象。
1.int get(int index) 获取链表中下标为 index 的节点的值。如果下标无效，则返回 -1 。
2. void addAtHead(int val) 将一个值为 val 的节点插入到链表中第一个元素之前。在插入完成后，新节点会成为链表的第一个节点。
3. void addAtTail(int val) 将一个值为 val 的节点追加到链表中作为链表的最后一个元素。
4. void addAtIndex(int index, int val) 将一个值为 val 的节点插入到链表中下标为 index 的节点之前。如果 index 等于链表的长度，那么该节点会被追加到链表的末尾。如果 index 比长度更大，该节点将 不会插入 到链表中。
5. void deleteAtIndex(int index) 如果下标有效，则删除链表中下标为 index 的节点。_

**思路：**
1. 利用虚拟头节点实现，对链表实现 get/delete/insert/addHead/addTail 操作。
2. 难点就是**仔细**写好每个func

``` go
type MyLinkedList struct {
	Head *Node
}

type Node struct {
	Val  int
	Next *Node
}

func Constructor() MyLinkedList {
	return MyLinkedList{}
}

func (this *MyLinkedList) Get(index int) int {
	i := 0
	cur := this.Head
	for cur != nil {
		if i == index {
			return cur.Val
		}
		i++
		cur = cur.Next
	}
	return -1
}

func (this *MyLinkedList) AddAtHead(val int) {
	newHead := &Node{
		Val:  val,
		Next: this.Head,
	}

	this.Head = newHead
	return
}

func (this *MyLinkedList) AddAtTail(val int) {
	cur := this.Head
	if cur == nil {
		this.Head = &Node{
			Val: val,
		}
		return
	}

	for cur.Next != nil {
		cur = cur.Next
	}

	cur.Next = &Node{
		Val: val,
	}
	return
}

func (this *MyLinkedList) AddAtIndex(index int, val int) {
	cur := this.Head
	dummyNode := &Node{
		Next: cur,
	}
	pre := dummyNode
	i := 0
	for cur != nil {
		if index == i {
			pre.Next = &Node{
				Val:  val,
				Next: cur,
			}
			this.Head = dummyNode.Next
			return
		}
		pre = cur
		cur = cur.Next
		i++
	}

	if i == index {
		pre.Next = &Node{
			Val: val,
		}
		this.Head = dummyNode.Next
	}
	return
}

func (this *MyLinkedList) DeleteAtIndex(index int) {
	cur := this.Head
	dummyNode := &Node{
		Next: cur,
	}
	pre := dummyNode
	i := 0
	for cur != nil {
		if i == index {
			pre.Next = cur.Next
			cur = cur.Next
			this.Head = dummyNode.Next
			return
		}
		pre = cur
		cur = cur.Next
		i++
	}
	return
}
```

### 206.反转链表
_给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。_

**思路：**
1. 每次反转 pre->cur 的指针，注意 for循环的终止条件。

``` go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseList(head *ListNode) *ListNode {
    var pre *ListNode
    cur := head
    for cur != nil{
        next := cur.Next
        cur.Next = pre
        pre = cur
        cur = next
    }

    return pre
}
```
