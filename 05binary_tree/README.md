- [Binary Tree](#binary-tree)
	- [Introduction](#introduction)
	- [Creating Binary Tree](#creating-binary-tree)
	- [Printing Binary Tree](#printing-binary-tree)

# Binary Tree


```python
"""
jupyter nbconvert --to markdown b_tree.ipynb --output README.md

<div align="center">
<img src="img/reversing_stack.jpg" alt="rec" width="900px">
</div>
"""

```

## Introduction


A binary tree is a tree data structure in which each parent node can have `0`,`1` or at most `2` children. Each node of a binary tree consists of three items:

- data item
- address of left child
- address of right child

<div align="center">
<img src="img/tree_intro.jpg" alt="rec" width="700px">
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
btn1 = BinaryTreeNode(1)
btn2 = BinaryTreeNode(2)
btn3 = BinaryTreeNode(3)
```


```python
btn1.left = btn2
btn1.right = btn3
```

## Printing Binary Tree


```python
def print_tree(root):
	if root is None:
		return
	print(root.data)
	print_tree(root.left)
	print_tree(root.right)

print_tree(btn1)
```

    1
    2
    3



```python
def print_tree_details(root):
	if root is None:
		return
	# printing root node
	print(" "*10,end="")
	print(root.data)
	print(" "*9, end="")
	print("⬋⬊")
	# printing left node
	if root.left != None:
		print(" "*8, end="")
		print(root.left.data,end="")
	else:
		print(" "*8, end="")
		print("✖", end="")
	# printing right node
	if root.right != None:
		print(" "*3, end="")
		print(root.right.data)
	else:
		print(" "*2, end="")
		print("✖")

	print_tree_details(root.left)
	print_tree_details(root.right)
	print()


btn = BinaryTreeNode("A")
btn1 = BinaryTreeNode("B")
btn2 = BinaryTreeNode("C")
btn.left = btn1
btn.right = btn2
btn3 = BinaryTreeNode("D")
btn1.left = btn3
btn4 = BinaryTreeNode("E")
btn5 = BinaryTreeNode("F")
btn2.left = btn4
btn2.right = btn5
print_tree_details(btn)

```

              A
             ⬋⬊
            B   C
              B
             ⬋⬊
            D  ✖
              D
             ⬋⬊
            ✖  ✖


              C
             ⬋⬊
            E   F
              E
             ⬋⬊
            ✖  ✖

              F
             ⬋⬊
            ✖  ✖




