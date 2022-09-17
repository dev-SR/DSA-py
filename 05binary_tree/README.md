# Binary Tree

- [Binary Tree](#binary-tree)
	- [Introduction](#introduction)
	- [Creating Binary Tree](#creating-binary-tree)
	- [Printing Binary Tree](#printing-binary-tree)
		- [Print All Nodes](#print-all-nodes)
			- [Print Node with  Left Child and Right Child](#print-node-with--left-child-and-right-child)
		- [Print At Depth k](#print-at-depth-k)
	- [Input of Binary Tree](#input-of-binary-tree)
	- [Number of Nodes in Binary Tree](#number-of-nodes-in-binary-tree)
	- [Height of Binary Tree](#height-of-binary-tree)
		- [isBalanced](#isbalanced)
		- [v2: getHeight+checkBalance](#v2-getheightcheckbalance)
	- [Largest Node](#largest-node)
	- [Number of Leaf Nodes](#number-of-leaf-nodes)
	- [Remove Leaf Nodes](#remove-leaf-nodes)
	- [Diameter of Binary Tree](#diameter-of-binary-tree)
	- [Level Order Traversal](#level-order-traversal)
	- [Build Tree Using inOrder And PreOrder](#build-tree-using-inorder-and-preorder)


```python
"""
cd .\05binary_tree\
jupyter nbconvert --to markdown b_tree.ipynb --output README.md
"""

```

## Introduction


A binary tree is a tree data structure in which each parent node can have `0`,`1` or at most `2` children. Each node of a binary tree consists of three items:

- data item
- address of left child
- address of right child

<div align="center">
<img src="img/tree_intro.jpg" alt="rec" width="500px">
</div>

## Creating Binary Tree


```python
class BinaryTreeNode:
	def __init__(self, data):
		self.data = data
		self.left = None
		self.right = None

	def __str__(self):
		return str(self.data)
```


```python
A = BinaryTreeNode("A")
B = BinaryTreeNode("B")
C = BinaryTreeNode("C")
D = BinaryTreeNode("D")
E = BinaryTreeNode("E")
F = BinaryTreeNode("F")

A.left = B
A.right = C
B.left = D
C.left = E
C.right = F
```

## Printing Binary Tree

### Print All Nodes

- `base case`: Check if the given node is `null`. If `null`, then `return` from the function.
- `Inductive step`: Print the data/root of the given node.
- `Hypothesis`: recursive function will print
  - left subtree
  - then, right subtree


```python
def print_tree(root):
	if root is None:
		return
	print(root.data)
	print_tree(root.left)
	print_tree(root.right)

print_tree(A)
```

    A
    B
    D
    C
    E
    F


<div align="center">
<img src="img/rtree.jpg" alt="rtree.jpg" width="800px">
</div>

Here is a simple simulation of printing a binary tree.

<div align="center">
<img src="img/print_tree_left.jpg" alt="print_tree_left.jpg" width="1000px">
</div>

#### Print Node with  Left Child and Right Child


```python
def print_tree_details(root):
	# print(l)
	if root is None:
		return
	# printing root node
	print(root.data,end="->")
	# printing left node
	if root.left != None:
		print(f"L:{root.left.data}",end=",")
	# printing right node
	if root.right != None:
		print(f"R:{root.right.data}", end="")

	print()
	print_tree_details(root.left)
	print_tree_details(root.right)

print_tree_details(A)
```

    A->L:B,R:C
    B->L:D,
    D->
    C->L:E,R:F
    E->
    F->


### Print At Depth k


```python
def printDepthK(root, k):
	if root == None:
		return
	if k == 0:
		print(root.data)
		return
	printDepthK(root.left, k - 1)
	printDepthK(root.right, k - 1)

```


```python
printDepthK(A,2)
```

    D
    E
    F



```python
def printDepthKV2(root,k, d=0):
	if root == None:
		return
	if k == d:
		print(root.data)
		return
	printDepthKV2(root.left,k, d + 1)
	printDepthKV2(root.right, k, d + 1)

```


```python
printDepthKV2(A,2)
```

    D
    E
    F


## Input of Binary Tree


```python
def takeInput():
	rootData = int(input())
	if rootData == -1:
		return None

	root = BinaryTreeNode(rootData)
	leftTree = takeInput()
	rightTree = takeInput()
	root.left = leftTree
	root.right = rightTree
	return root

```


```python
root = takeInput() # 1 2 4 -1 -1 5 -1 -1 3 -1 7 -1 -1
print_tree_details(root)

```

    1->L:2,R:3
    2->L:4,R:5
    4->
    5->
    3->R:7
    7->


## Number of Nodes in Binary Tree

- `Base Case`: `if root == None: return 0`
- `Induction Hypothesis`:
  - leftTree will give total number of nodes in leftTree = `totalNodesInLeftTree`
  - rightTree will give total number of nodes in rightTree = `totalNodesInRightTree`
- `Recursive Step`:
  - `1(root) + totalNodesInLeftTree + totalNodesInRightTree = totalNodesInTree`


```python
def numNodes(root):
	if root == None:
		return 0
	totalNodesInLeftTree = numNodes(root.left)
	totalNodesInRightTree = numNodes(root.right)
	totalNodesInTree = 1 + totalNodesInLeftTree + totalNodesInRightTree
	return totalNodesInTree
```


```python
numNodes(A)
```




    6



## Height of Binary Tree


```python
def height(root):
	if root == None:
		return 0
	leftHeight = height(root.left)
	rightHeight = height(root.right)
	return 1 + max(leftHeight, rightHeight)

```


```python
A = BinaryTreeNode("A")
B = BinaryTreeNode("B")
C = BinaryTreeNode("C")
E = BinaryTreeNode("E")
F = BinaryTreeNode("F")

A.left = B
A.right = C
C.left = E
C.right = F

height(A)
```


    3


### isBalanced


```python
def isBalanced(root):
	if root == None:
		return True
	leftHeight = height(root.left)
	rightHeight = height(root.right)
	if abs(leftHeight - rightHeight) > 1:
		return False

	# check if left and right subtree are balanced
	isLeftBalanced = isBalanced(root.left)
	isRightBalanced = isBalanced(root.right)
	if isLeftBalanced and isRightBalanced:
		return True
	else:
		return False
```


```python
A = BinaryTreeNode("A")
B = BinaryTreeNode("B")
C = BinaryTreeNode("C")
E = BinaryTreeNode("E")
F = BinaryTreeNode("F")

A.left = B
A.right = C
C.left = E
C.right = F

isBalanced(A)
```


    True



```python
A = BinaryTreeNode("A")
B = BinaryTreeNode("B")
C = BinaryTreeNode("C")
D = BinaryTreeNode("D")
E = BinaryTreeNode("E")
F = BinaryTreeNode("F")

A.left = B
A.right = C
C.left = E
C.right = F
E.left = D

isBalanced(A)
```


    False


### v2: getHeight+checkBalance


```python
def getHeightAndCheckBalanced(root):
	if root == None:
		return 0, True

	leftHeight, isLeftBalanced = getHeightAndCheckBalanced(root.left)
	rightHeight, isRightBalanced = getHeightAndCheckBalanced(root.right)

	height = 1 + max(leftHeight, rightHeight)

	if abs(leftHeight - rightHeight) > 1:
		return height, False

	if isLeftBalanced and isRightBalanced:
		return height, True
	else:
		return height, False

```


```python
A = BinaryTreeNode("A")
B = BinaryTreeNode("B")
C = BinaryTreeNode("C")
E = BinaryTreeNode("E")
F = BinaryTreeNode("F")

A.left = B
A.right = C
C.left = E
C.right = F

getHeightAndCheckBalanced(A)
```


    (3, True)



```python
A = BinaryTreeNode("A")
B = BinaryTreeNode("B")
C = BinaryTreeNode("C")
D = BinaryTreeNode("D")
E = BinaryTreeNode("E")
F = BinaryTreeNode("F")

A.left = B
A.right = C
C.left = E
C.right = F
E.left = D

getHeightAndCheckBalanced(A)
```


    (4, False)


## Largest Node

- `Base Case`: `if root == None: return -1`
- `Induction Hypothesis`:
  - leftTree will give the largest node in leftTree = `largestNodeInLeftTree`
  - rightTree will give largest node in rightTree = `largestNodeInRightTree`
- `Recursive Step`:
  - `Max(root,largestNodeInLeftTree, largestNodeInRightTree) = largestNodeInTree`


```python
A = BinaryTreeNode(10)
B = BinaryTreeNode(20)
C = BinaryTreeNode(5)
D = BinaryTreeNode(4)
E = BinaryTreeNode(3)

A.left = B
A.right = C
B.left = D
B.right = E

print_tree_details(A)
```

    10->L:20,R:5
    20->L:4,R:3
    4->
    3->
    5->



```python
def findLargestNode(root):
	if root ==None:
		return -1
	largestNodeInLeftTree = findLargestNode(root.left)
	largestNodeInRightTree = findLargestNode(root.right)
	largestNodeInTree = max(root.data, largestNodeInLeftTree, largestNodeInRightTree)
	return largestNodeInTree

```


```python
findLargestNode(A)
```




    20



## Number of Leaf Nodes

<div align="center">
<img src="img/leaf.jpg" alt="leaf.jpg" width="800px">
</div>


```python
def numLeafNodes(root):
	if root == None:
		return 0
	# both both left and right are leaf nodes are None - means they are leaf nodes
	if root.left == None and root.right == None:
		return 1

	totalLeafNodesInLeftTree = numLeafNodes(root.left)
	totalLeafNodesInRightTree = numLeafNodes(root.right)
	totalLeafNodesInTree = totalLeafNodesInLeftTree + totalLeafNodesInRightTree
	return totalLeafNodesInTree

```


```python
A = BinaryTreeNode("A")
B = BinaryTreeNode("B")
C = BinaryTreeNode("C")
D = BinaryTreeNode("D")
E = BinaryTreeNode("E")
F = BinaryTreeNode("F")

A.left = B
A.right = C
C.left = E
C.right = F

numLeafNodes(A)
```




    3



## Remove Leaf Nodes

<div align="center">
<img src="img/rml.jpg" alt="rml.jpg" width="600px">
</div>


```python
x1 = BinaryTreeNode("1")
x2 = BinaryTreeNode("2")
x3 = BinaryTreeNode("3")
x4 = BinaryTreeNode("4")
x5 = BinaryTreeNode("5")
x6 = BinaryTreeNode("6")
x7 = BinaryTreeNode("7")
x8 = BinaryTreeNode("8")
x9 = BinaryTreeNode("9")


x1.left = x2
x1.right = x3
x2.left = x4
x2.right = x5
x5.left = x6
x5.right = x7
x3.left = x8
x3.right = x9
print_tree_details(x1)
```

    1->L:2,R:3
    2->L:4,R:5
    4->
    5->L:6,R:7
    6->
    7->
    3->L:8,R:9
    8->
    9->



```python
def removeLeafNodes(root):
	if root == None:
		return None

	if root.left == None and root.right == None:
		return None

	root.left = removeLeafNodes(root.left)
	root.right = removeLeafNodes(root.right)
	return root
```


```python
root = removeLeafNodes(x1)
print_tree_details(root)
```

    1->L:2,R:3
    2->R:5
    5->
    3->


## Diameter of Binary Tree

<div align="center">
<img src="img/diameter.jpg" alt="diameter.jpg" width="1000px">
</div>

<div align="center">
<img src="img/diam1.jpg" alt="diam1.jpg" width="800px">
</div>


```python
def diameter(root):
	if root == None:
		return 0

	option1 = height(root.left) + height(root.right)
	option2 = diameter(root.left)
	option3 = diameter(root.right)

	return max(option1, max(option2, option3))
```


```python
x1 = BinaryTreeNode("1")
x2 = BinaryTreeNode("2")
x3 = BinaryTreeNode("3")
x4 = BinaryTreeNode("4")
x5 = BinaryTreeNode("5")
x6 = BinaryTreeNode("6")
x7 = BinaryTreeNode("7")

x1.left = x2
x1.right = x3
x2.left = x4
x2.right = x5
x4.left = x6
x5.right = x7

print_tree_details(x1)

```

    1->L:2,R:3
    2->L:4,R:5
    4->L:6,
    6->
    5->R:7
    7->
    3->



```python
diameter(x1)
```




    4



## Level Order Traversal

- Add the root to the queue
- current elements in the queue represents the current level elements
  - so, for every element in the queue
    - pop the element
    - append to the current level list
    - add all the children of the popped element to the queue; this will add the next level elements to the queue


<div align="center">
<img src="img/tree.jpg" alt="tree.jpg" width="400px">
</div>


```python
x1 = BinaryTreeNode("1")
x2 = BinaryTreeNode("2")
x3 = BinaryTreeNode("3")
x4 = BinaryTreeNode("4")
x5 = BinaryTreeNode("5")
x6 = BinaryTreeNode("6")
x7 = BinaryTreeNode("7")
x8 = BinaryTreeNode("8")
x9 = BinaryTreeNode("9")


x1.left = x2
x1.right = x3
x2.left = x4
x2.right = x5
x5.left = x6
x5.right = x7
x3.left = x8
x3.right = x9
print_tree_details(x1)
```

    1->L:2,R:3
    2->L:4,R:5
    4->
    5->L:6,R:7
    6->
    7->
    3->L:8,R:9
    8->
    9->



```python
from collections import deque

def get_level_order(root):
	if root == None:
		return

	q = deque()
	q.append(root)

	level_order_list = []


	while len(q) > 0:
		currentLevel = []
		# current elements in the queue represents the current level elements
		elementsInCurrentLevel = len(q)
		# so, for every element in the queue
		for i in range(elementsInCurrentLevel):
			# get current level element
			currentNode = q.popleft()
			currentLevel.append(currentNode.data)

			# add its children to the queue for next level
			if currentNode.left != None:
				q.append(currentNode.left)
			if currentNode.right != None:
				q.append(currentNode.right)
		level_order_list.append(currentLevel)

	return level_order_list

get_level_order(x1)

```




    [['1'], ['2', '3'], ['4', '5', '8', '9'], ['6', '7']]



## Build Tree Using inOrder And PreOrder

- **`root`**, `left`, `right` | `PreOrder`
- `left`, **`root`**, `right` | `inOrder`

<div align="center">
<img src="img/inpre.jpg" alt="inpre.jpg" width="700px">
</div>

we need both inOrder and preorder to build a tree.
algorithm:
- `root` is the first element in `preorder`
- find `inoder` of both `left` and `right` subtree
- find `preorder` of both `left` and `right` subtree


```python
def buildTreeFromPreIn(pre,inOrder):
	if len(pre) == 0:
		return None

	rootData = pre[0]
	root = BinaryTreeNode(rootData)
	rootIndexInOrder = -1

	for i in range(0, len(inOrder)):
		if inOrder[i] == rootData:
			rootIndexInOrder = i
			break
	if rootIndexInOrder == -1:
		return None

	leftInOrder = inOrder[0:rootIndexInOrder]
	rightInOrder = inOrder[rootIndexInOrder+1:]

	lengthOfLeftSubtree = len(leftInOrder)
	leftPreOrder = pre[1:lengthOfLeftSubtree+1]
	rightPreOrder = pre[lengthOfLeftSubtree+1:]

	leftChild = buildTreeFromPreIn(leftPreOrder, leftInOrder)
	rightChild = buildTreeFromPreIn(rightPreOrder, rightInOrder)

	root.left = leftChild
	root.right = rightChild

	return root
```


```python
pre = [1, 2, 4, 5, 3, 6, 7]
inOrder = [4, 2, 5, 1, 6, 3, 7]

root = buildTreeFromPreIn(pre, inOrder)
print_tree_details(root)
```

    1->L:2,R:3
    2->L:4,R:5
    4->
    5->
    3->L:6,R:7
    6->
    7->

