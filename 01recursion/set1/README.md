- [Problem Set 1](#problem-set-1)
	- [Factorial](#factorial)
	- [Sum of first n Integer](#sum-of-first-n-integer)
	- [Print of first n Integer](#print-of-first-n-integer)
	- [Fibonacci Number](#fibonacci-number)
	- [Sum of Array](#sum-of-array)
	- [Check Number in Array](#check-number-in-array)
	- [Check If List Is Sorted Or Not](#check-if-list-is-sorted-or-not)
	- [First Index Of Number in an Array](#first-index-of-number-in-an-array)
	- [Last Index Of Number in an Array](#last-index-of-number-in-an-array)

# Problem Set 1

## Factorial

Using Mathematical Induction:

-  Base Case `f(0) = 1` is true.
-  Assuming `f(n-1)` will give me proper ans
-  Then, `n * f(n - 1)` will also be True.


```python
def fact(n):
	if n == 0:
		return 1
	smallOutput = fact(n-1) # calling phase
	return n * smallOutput # op in returning phase

fact(5)
```




    120



## Sum of first n Integer

Using Mathematical Induction:

-  Base Case `f(0) = 1` is true.
-  Assuming `f(n-1)` will give me proper ans
-  Then, `n * f(n - 1)` will also be True.


```python
def sum_n(n):
	if n == 0:
		return 0
	smallOutput = sum_n(n-1) # calling phase
	output = smallOutput + n   # op in returning phase
	return output  # op in returning phase

print(sum_n(5))

```

    15


## Print of first n Integer

Using Mathematical Induction:

-  Base Case `f(0)` is true.
-  Assuming `f(n-1)` will print up to `1......n-1`
-  If `f(n-1)` prints upto `n-1` ,then We just need to print `n`


```python
def print_1_to_n(n):
	if n == 0:
		return
	print_1_to_n(n-1)  # calling phase
	print(n)  # op in returning phase
	return # op in returning phase


print_1_to_n(5)

```

    1
    2
    3
    4
    5



```python
def print_1_to_n(n):
	if n == 0:
		return
	print(n)  # calling phase
	print_1_to_n(n-1)# calling phase
	return  # op in returning phase


print_1_to_n(5)

```

    5
    4
    3
    2
    1


## Fibonacci Number

**Extended PMI** says....

Assume `f(1)` is true , Assume `f(2)` is true -> `f(3)` .... -> `f(k)` is True.
Then Using all of this we can proof if `f(k+1)` is `True` or `False`

So, we can use `f(1)` to proof `f(2)`, then using `f(2)` we can proof `f(3)`...and so on.
Therefore, we can also use both `f(1) and f(2)` to proof `f(3)` , and `f(1),f(2),f(3)` to proof `f(4)` and so on.




```python
def fib(n):
	if n == 0 or n == 1:
		return n
	fib_n_1 = fib(n-1) # calling phase
	fib_n_2 = fib(n-2)  # calling phase
	output = fib_n_1 + fib_n_2  # op in returning phase
	return output  # op in returning phase

print(fib(6)) # 1,1,2,3,5, 8
```

    8


## Sum of Array

- Base Case: If List is of size `0`, then `sum=0`;
- Induction Hypothesis: Summation of List of size `l-1`, (`[1:]`) is `smallSum`,
- Induction Step/Recursive Calculation:
  - `a[0] + smallSum`



```python
def sum_of_array(a):
	l = len(a)
	if l == 0:
		return 0

	smallerList = a[1:]
	smallerSum = sum_of_array(smallerList)
	return a[0] + smallerSum

print(sum_of_array([2,2,2,1,1]))

```

    8


## Check Number in Array

- Base Case: List is of size `0` or `1` => `return False`
- Induction Hypothesis: `x` is present in List of size `l-1`
- Induction Step:
  - if `a[0]!=x`
  - ask recursion to find `x` in the `smallerList`


```python
def checkNumberInArray(x,a):
	l = len(a)
	if l == 0:
		return False

	if x == a[0]:
		return True

	smallerList = a[1:]
	presentInSmallerList = checkNumberInArray(x,smallerList)

	if presentInSmallerList:
		return True
	else:
		return False



print(checkNumberInArray(3,[1,2,3,4,5]))
print(checkNumberInArray(6, [1, 2, 3, 4, 5]))

```

    True
    False


## Check If List Is Sorted Or Not

- Base Case: List is of size `0` or `1`
- Induction Hypothesis:  It will works with `l-1` sized List.
- Induction Step:
  - check if `[0] > [1]`; `rerun false` ~ list is not sorted
  - else check for smallerList starting from `[1:]`

```
    False<==> f([1,2,1,5,4])
                ↓
              1>2(x).....do nothing
                ↓
        False <==>  f([2,1,5,4])
                      ↓
                    2>1 return false
                      ↓
                ☝  False

```

```
    TRUE<==>  f([1,2,3])
                ↓
              1>2(x).....do nothing
                ↓
        TRUE <==>  f([2,3])
                      ↓
                    2>3(x).....do nothing
                      ↓
          TRUE <==>  f([3])
                        ↓
                      len = 1
                        ↓
                  ☝  TRUE

```


```python
def isSorted(a):
	l = len(a)
	if l == 0 or l == 1: # base case
		return True

	# Induction step
	if a[0] > a[1]:
		return False

	smallerList = a[1:]
	isSmallerSorted = isSorted(smallerList)
	if isSmallerSorted:
		return True
	else:
		return False


print(isSorted([4,2,3,1,5]))
print(isSorted([1,2,3,4,5]))
```

    False
    True


But current solution is making two many Copies in the memory each time of calling the function. So below code is memory efficient:

- Induction Hypothesis: It will works with `l-1` sized List.
- Induction Step:
  - check `si == l-1 or si == l ` `rerun false` ; *# si = start index*
  - else check for smallerList starting from `[si+1]` index

```
    False<==> f([1,2,1,5,4],si=0)
                ↓
              a[si]>a[si+1].....do nothing
                ↓
                si+1
                ↓
      False <==> f([2,1,5,4],1)
                      ↓
                    a[si]>a[si+1] return false
                      ↓
                ☝  False

```

```
    TRUE<==>  f([1,2,3],0)
                ↓
              a[si]>a[si+1].....do nothing
                    ↓
                    si+1
                    ↓
        TRUE <==>  f([2,3],1)
                      ↓
                    2>3(x).....do nothing
                      ↓
          TRUE <==>  f([3],2)
                        ↓
                      si = l
                        ↓
                  ☝  TRUE

```


```python
def isSortedBetter(a,si):# si = start index
	l = len(a)
	if si == l-1 or si == l:
		return True

	if a[si] > a[si+1]:
		return False

	isSmallerSorted = isSortedBetter(a,si+1)
	return isSmallerSorted

print(isSortedBetter([1,2,3,4,5],0))
print(isSortedBetter([1, 2, 4, 3, 5], 0))

```

    True
    False


## First Index Of Number in an Array

- Base Case: List is of size `0` or `1`
- Induction Hypothesis:  It will works with `l-1` sized List.
- Induction Step:
  - check `a[0]===x`; `rerun 0` ~ found
  - else check in smallerList starting from `[1:]`



```python
def firstIndex(a,x):
	l = len(a)
	if l == 0:
		return -1

	if a[0] == x:
		return 0

	smallerList = a[1:]
	foundInSmallerList = firstIndex(smallerList,x)
	if foundInSmallerList== -1:
		return -1
	else:
		return foundInSmallerList+ 1

print(firstIndex([1,2,3,4,5],3))
print(firstIndex([1, 2, 3, 4, 5], 10))

```

    2
    -1



```python
def firstIndexBetter(a,x,si):
	l = len(a)
	if si == l:
		return -1

	if a[si] == x:
		return si

	foundInSmallerList = firstIndexBetter(a,x,si+1)
	return foundInSmallerList

print(firstIndexBetter([1,2,3,4,5],3,0))
print(firstIndexBetter([1, 2, 3, 4, 5], 10, 0))

```

    2
    -1


## Last Index Of Number in an Array

```sh
    3 <==> f([1,3,4,3],3)
                ↓
        2,2+1 <==> f([3,4,3],3)
                      ↓
			1,1+1 <==> f([4,3],3)
						↓
				0,0+1 <==> f([3],3)
							↓
			   -1, a[0]==3 <==> f([],3)
								↓
						  return -1

                ☝  False

```



```python
def lastIndex(a,x):
	l = len(a)
	if l == 0:
		return -1

	smallerList = a[1:]
	foundInSmallerList = lastIndex(smallerList,x) # Calling phase
	# op in returning phase
	if foundInSmallerList != -1:
		return foundInSmallerList+ 1
	else:
		if a[0] == x:
			return 0
		else:
			return -1

print(lastIndex([1,3,4,3],3))
```

    3



```python
def lastIndexBetter(a,x,si):
	l = len(a)
	if si == l:
		return -1

	foundInSmallerList = lastIndexBetter(a,x,si+1) # Calling phase
	# op in returning phase
	if foundInSmallerList != -1:
		return foundInSmallerList
	else:
		if a[si] == x:
			return si
		else:
			return -1

a= [1,3,4,3]
x=3
print(lastIndexBetter(a,x,0))
```

    3

