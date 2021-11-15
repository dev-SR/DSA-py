- [Linked List](#linked-list)
	- [Node Creation](#node-creation)
	- [Linked List Creation v1](#linked-list-creation-v1)

# Linked List

<div align="center">
<img src="img/ll_1.jpg" alt="rec" width="800px">
</div>

## Node Creation

```python
class Node:

	def __init__(self,data):
		self.data=data
		self.next=None


a= Node(1)
b= Node(2)
print(a.next, "<--- `a.next`")

#    ┏━━━┓      ┏━━━┓
#    ┃ a ┃----> ┃ b ┃
#    ┗━━━┛      ┗━━━┛
a.next=b


print(a.next,"<--- `a.next`, after `a.next=b`")
print(b, "<--- also the address `b`")
print(b.data)
print(a.next.data)

```

    None <--- `a.next`
    <__main__.Node object at 0x0000016B6BB289D0> <--- `a.next`, after `a.next=b`
    <__main__.Node object at 0x0000016B6BB289D0> <--- also the address `b`
    2
    2


## Linked List Creation v1

<div align="center">
<img src="img/linked_list_creation_v1.jpg" alt="rec" width="900px" >
</div>
<!-- width="800px" -->


```python
class Node:

	def __init__(self, data):
		self.data = data
		self.next = None

def takeInput():
	inputList= [int(ele) for ele in input().split()] # 1 2 3 -1 ----->  [1,2,3,-1]

	head = None
	for currentData in inputList:
		if currentData == -1:
			break

		newNode = Node(currentData)
		# fist node
		if head is None:
			head = newNode
		else:
			currentNode = head # do not change the value of head
			# go to last node
			while currentNode.next is not None:
				currentNode = currentNode.next
			# assign new node to last node's next
			currentNode.next = newNode

	return head

def printList(head):
	while head is not None:
		print(str(head.data)+"->", end="")
		head = head.next
	print("None")
```


```python
head = takeInput() # 1 2 3 -1
printList(head)
```

    1->2->3->None

