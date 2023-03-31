
# Recursion and Iteration
**by Jing Wen**

The anatomy of a recursion solution is simple, for 99% of problems it follows roughly this format:
```python
def function_R(...):
	BASE CASE
	PROCESS CURRENT + function_R(NEXT) 
	# order and operator (in this case, +) depends on question
```
For example, to sum up all numbers to a positive integer, `n`, we first decide on a base case, where can stop looping. In the case of sum, we can decrement `n` until it reaches 0, i.e.
$$sum(3) = 3 + sum(2) = 3 + 2 + sum(1) = 3 + 2 + 1 + sum(0)$$
which when using the base case
$$sum(0) = 0$$
leads us to conclude
$$sum(3) = 3 + 2 + 1 + 0 = 6$$

Note that the base case can also be:
$$sum(1) = 1$$
or anything that we know the value of (i.e. $sum(2)=3$, $sum(3)= 6$) but if the question says that $n$ can be any positive integer, then if your base case is more than 1, it will not be captured, thus we choose either 1 or 0.

A Python implementation of this would be
```python
def sum_R(n):
	if n == 0:
		return 0
	return n + sum_R(n - 1) 
```
In this case, our `BASE CASE` is $sum(0) = 0$ as discussed above, `PROCESS CURRENT` is $n$ i.e. we dont do anything to $n$ and `NEXT` is $n-1$. 

The structure of our return function is `PROCESS CURRENT + f(NEXT)` but it could very well be `f(NEXT) + PROCESS CURRENT` for this question because sum is commutative.

If we were to modify our question and instead get the sum of squares, we have to make a modification to `PROCESS CURRENT` to square each number.
```python
def sum_of_squares_R(n):
	if n == 0:
		return 0
	return n**2 + sum_of_squares_R(n - 1) # this line changed
```
The logic of summing everything still stays the same and used the same `BASE CASE` and `NEXT`.

**More  Complex `PROCESS CURRENT`**

Power is also quite a simple `PROCESS CURRENT`, but you can do more complex things, for example to calculate the sum where only odd numbers are squared:
```python
def sum_of_squares_R(n):
	if n == 0:
		return 0
	if n % 2 == 0: # if even
		return n + sum_of_squares_R(n - 1)
	else: # if odd
		return n**2 + sum_of_squares_R(n - 1)
```
**Non-commutative Operators**

Let us now look at the case whereby addition is not commutative, strings.
`'a'+'b'='ab'`
however,
`'b'+'a'='ba'`
Thus, the order in which things are added is important. Reversing the order they are added reverses the output. 

Let us look at this function which takes in a string and returns the exact same string:
```python
def f(s):
	if len(s) == 0:
		return ''
	return s[0] + f(s[1:]) 
```

Using the non-commutativity of string addition, by reversing the order of `PROCESS CURRENT` and `f(NEXT)`, we can reverse the string:
```python
def f(s):
	if len(s) == 0:
		return ''
	return f(s[1:]) + s[0] 
```

**Other Operators**

Until now, we have been using the arithmetic operator `+`. However, there is no reason we are restricted to this. We can use boolean operators:
 ```python
def no_a_in_string(s):
	if len(s) == 0:
		return True
	return f(s[1:]) and s[0]!='a' 
```
and even functions:
 ```python
def maximum(s):
	if len(s) == 0:
		return 0
	return max(int(s[0]), f(s[1:]))
```
There can also be multiple recursion terms in a single return, like Fibonacci which has a return like ```return fibonacci(n-1) + fibonacci(n-2)```

**Slicing in `PROCESS CURRENT`**
Many string questions make us use substrings  rather than just the first character, `s[0]`. An example is reversing pairs of numbers:
 ```python
def rev(s):
	if len(s) <= 1:
		return s
	return s[1] + s[0] + rev(s[2:])
```
In the `return`, note that `PROCESS CURRENT` takes the form of `s[1] + s[0]` which is an effective swapping of the order, and `NEXT` is `s[2:]`. The algorithm is effectively saying to chop the first two off, swap them and run the same thing on the rest of the string.
```
rev('abcdef') 
= 'b' + 'a' + rev('cdef')
= 'b' + 'a' + 'd' + 'c' + rev('ef')
= 'b' + 'a' + 'd' + 'c' + 'f' + 'e'
= 'badcfe'
```
alternatively, `return s[:2][::-1] + rev(s[2:])` could be used. `[:2]` selects the first two characters and `[::-1]` reverses it.


## Practice: Integers
For each question, write an recursive and iterative version.

(Hard) Write a one line version that does not use recursion (making use of list/string comprehension).
### Sum to N
Given a integer $n$,
$$ \sum_{x=0}^{n}{x}$$

| Test Case | Expected Result |
|-|-|
| 100 | 5050 |
| 9999 | 49995000 |
| -100 | -5050 |

### Sum of Squares to N
Given a integer $n$,
$$ \sum_{x=0}^{n}{x^2}$$
| Test Case | Expected Result |
|-|-|
| 100 | 338350 |
| 9999 | 332833500 |
| -100 | 338350 |

### Addition
In this question, you are not allowed to use the numeric operators `+`, `-`, `*`, `/` or `**`.
You are given the functions `successor` and `negate`:
```python
def successor(n):
	return n + 1
	
def negate(n):
	return -n
```
Create a function `add` that adds two integers `A` and `B` together, i.e.
$$ add(A, B) = A + B$$
using `successor`, `negate` and without using the aforementioned operators.

<details>
  <summary>Possible Solution</summary>
  
  ```python
def successor(n):
	return n + 1
	
def negate(n):
	return -n
	
def add(a, b):
    # note that negate(successor(negate(x))) is -(-x+1) = x-1
    if b == 0:
        return a
    if b < 0:
        return add(negate(successor(negate(a))), successor(b)) # add(a-1, b+1)
    return add(successor(a), negate(successor(negate(b)))) # add(a+1, b-1)
  ```
  
</details>

### Multiplication
In this question, you are not allowed to use the numeric operators `+`, `-`, `*`, `/` or `**`.
Similarly, u create a function `mul` that multiplies `A` and `B` together, i.e. 
$$ mul(A, B) = AB$$
using `add` and without using the aforementioned operators.

<details>
  <summary>Possible Solution</summary>
  
  ```python
def successor(n):
	return n + 1
	
def negate(n):
	return -n
	
def add(a, b):
    # note that negate(successor(negate(x))) is -(-x+1) = x-1
    if b == 0:
        return a
    if b < 0:
        return add(negate(successor(negate(a))), successor(b)) # add(a-1, b+1)
    return add(successor(a), negate(successor(negate(b)))) # add(a+1, b-1)
    
def mul(a, b):
    if b == 1:
        return a
    elif b == negate(1):
        return negate(a)
    if b < 0:
        return negate( add(a, mul(a, negate(successor(b))))) # -a - mul(a, -b-1))
    return add(a, mul(a, negate(successor(negate(b))))) # a + mul(a, b-1)
  ```
  
</details>

### Power
In this question, you are not allowed to use the numeric operators `+`, `-`, `*`, `/` or `**`.
Similarly, u create a function `pow` that multiplies `A` and `B` together, i.e. 
$$ pow(A, B) = A^B$$
using `mul` and without using the aforementioned operators.
Note that `B` is always a positive integer or zero.

| Test Case | Expected Result |
|-|-|
| **(A, B)**| |
| (3, 2)| 9 |
| (3, 3)| 27 |
| (-3, 3)| -27 |
| (3, 0)| 1 |
| (0, 0)| 1 |

<details>
  <summary>Possible Solution</summary>
  
  ```python
def successor(n):
	return n + 1
	
def negate(n):
	return -n
	
def add(a, b):
    # note that negate(successor(negate(x))) is -(-x+1) = x-1
    if b == 0:
        return a
    if b < 0:
        return add(negate(successor(negate(a))), successor(b)) # add(a-1, b+1)
    return add(successor(a), negate(successor(negate(b)))) # add(a+1, b-1)
    
def mul(a, b):
    if b == 1:
        return a
    elif b == negate(1):
        return negate(a)
    if b < 0:
        return negate( add(a, mul(a, negate(successor(b))))) # -a - mul(a, -b-1))
    return add(a, mul(a, negate(successor(negate(b))))) # a + mul(a, b-1)
    
def pow(a, b):
    if b == 0:
        return 1
    return mul(a,  pow(a, negate(successor(negate(b))))) # a * pow(a, b-1)
  ```
  
</details>

### Fibonacci
Given a positive integer $n$, find the $n$th term of the Fibonacci sequence i.e.
$$ F_n = F_{n-1}+ F_{n-2}$$
where
$$F_0 = 0$$
and
 $$F_1 = 1$$
| Test Case | Expected Result |
|-|-|
| 10| 55 |
| 18| 2584 |

### Check Power
Given a positive integer $n$ and an integer $x$, create a function `isPower(x,n)` that checks if `x` is a power of `n`.
| Test Case | Expected Result | Remarks |
|-|-|-|
| **(x, n)**| |
| (1, 2)| True| $2^0=1$|
| (16, 2)| True | $2^4=16$|
| (81, -3) | True | $(-3)^4=81$|
| (82, -3) | False| There is no $n$ where $(-3)^n=82$|
| (100, 5) | False| There is no $n$ where $5^n=100$|
	
<details>
  <summary>Possible Solution</summary>
  
  ```python
def isPower(x,n):
    i = 1 # n^0 = i = 1
    while i < x: # upper bound is x as i is strictly increasing, so when i is more than x we can give up
        i *= n # increase n's exponent by 1 i.e. multiply by n
        if i == x: # if match, stop early and return true
            return True
    return False # can't find
  ```
One-liner:
```python
isPower = lambda x, n: x in [n**exp for exp in range(x)] # technically range(logn(x)) but x is always larger than logn(x)
```
  
</details>

### Prime (No need recursion)
Given a positive integer $n$, create a function `isPrime(n)` that checks if `n` is prime.
| Test Case | Expected Result |
|-|-|
| 1 | False |
| 2 | True |
| 4 | False |
| 73 | True|
| 7919 | True |
| 7920 | False |
	
<details>
  <summary>Possible Solution</summary>
	
```python
def isPrime(n):
    for i in range(2, n//2+1):
        if n%i==0:
            return False
    return True
```
One-liner:
```python
# Naive method
isPrime = lambda n : not any([n%i==0 for i in range(2, n//2+1)])

# Sieve of Eratosthenes
isPrime = lambda n : n not in {i*j for i in range(2,n) for j in range(2,n//i+1)}
```
  
</details>

### Super Digit
We define super digit of an integer  $x$ using the following rules:
-   If  has only  digit, then its super digit is $x$.
-   Otherwise, the super digit of $x$ is equal to the super digit of the sum of the digits of $x$. 

For example, super digit of  $9875$ will be calculated as:
$$super(9875) =  super(9+8+7+5) = super(29)$$
$$super(29) = super(2+9) = super(11)$$
$$super(11) = super(1+1) = super(2) = 2$$

| Test Case | Expected Result |
|-|-|
| 9875 | 2 |
| 9873 | 9 |
| 148148148  | 3|

### Number of Ones (Hard)
Given an integer `n`, count the total number of digit  `1`  appearing in all positive integers <= `n`. i.e.
$$ ones(13) = 6 $$
as
**1**, 2, 3, 4, 5, 6, 7, 8, 9, **1**0, **11**, **1**2, **1**3
`Number of 1s = 6`

| Test Case | Expected Result |
|-|-|
| 13| 6 |
| 100| 21 |
| 1000 | 301|
| 31415 | 22904 |
| 314159 | 260456 |
| 3141592 | 2918706 |
| 3141592653589793 | 5746138735375548 |

## Practice: Strings
For each question, write an recursive and iterative version.

(Hard) Write a one line version that does not use recursion (making use of list/string comprehension).
### Stutter
For each character in an input string `s`, repeat each character 3 times.
| Test Case | Expected Result |
|-|-|
| `hello` | `hhheeellllllooo` |
| `test`| `ttteeesssttt` |
| `abc` | `aaabbbccc`|

### No Consecutive Duplicates
Given an input string `s`, remove any consecutive duplicate characters.
| Test Case | Expected Result |
|-|-|
| `hello` | `helo` |
| `tessst`| `test` |
| `abc` | `abc`|

###  Reverse String
Given an input string `s`, reverse a string without using `s[::-1]`.
| Test Case | Expected Result |
|-|-|
| `hello` | `olleh` |
| `abc`| `cba` |
| `racecar` | `racecar`|

###  Reverse in Blocks of Two
Given an input string `s`, reverse every 2 characters. 
| Test Case | Expected Result |
|-|-|
| `ababab` | `bababa` |
| `abcdef`| `badcfe`|
| `abcdefg` | `badcfeg`|

###  Reverse in Blocks of n
Given an input string `s` and a positive integer `n`, reverse every n characters or part thereof. 
| Test Case | Expected Result |
|-|-|
| **(s, n)**| |
| (`ababab`, 2) | `bababa` |
| (`abcdef`, 2)| `badcfe`|
| (`abcdefg`, 2) | `badcfeg`|
| (`ababab`, 3) | `ababab` |
| (`abcdefgh`, 3)| `cbafedhg`|
| (`abcdefg`, 4) | `dcbagfe`|

### Run-Length Decoding
Given an input string `s`, decode it based on run-length encoding.  For example, `w4a3d1e1x6` is decoded to `wwwwaaadexxxxxx`
												     
<details>
  <summary>Possible Solution</summary>
  
  ```python
def rld(s):
    if s == '':
        return ''
    return s[0] * int(s[1]) + rld(s[2:])
  ```
  
</details>

### Run-Length Encoding (No need recursion)
Given an input string `s`, decode it based on run-length encoding.  For example, `wwwwaaadexxxxxx` is encoded to `w4a3d1e1x6`

<details>
  <summary>Possible Solution</summary>
  
  ```python
def rle(s):
    out = ''
    cur = s[0]
    count = 1
    for c in s[1:]:
        if c == cur:
            count += 1
        else:
            out += cur + str(count)
            cur = c
            count = 1
    return out + cur + str(count)
  ```
  
</details>

### Caesar Cipher
Implement a Caesar cipher to encode and decode a given input string `s` using a provided integer key `k`. The Caesar cipher is a substitution cipher where each letter in the plaintext is shifted a certain number of places down the alphabet using the key.

| Test Case | Expected Result |
|-|-|
| **(s, k)**| |
| **Encode** | |
| (`"hello"`, 3) | `"khoor"` |
| (`"world"`, 5)| `"btwqi"`|
| (`"caesar"`, 1) | `"dbftbs"`|
| (`"shift"`, 10) | `"kpdwlo"`|
| (`"python"`, 13)`| `"clguba"`|
| **Decode** | |
| (`"khoor"`, 3) | `"hello"` |
| (`"btwqi"`, 5)| `"world"`|
| (`"dbftbs"`, 1) | `"caesar"`|
| (`"kpdwlo"`, 10) | `"shift"`|
| (`"clguba"`, 13)`| `"python"`|

### Permutations of a String
Generate all possible permutations of a given input string `s`. The output should be a list of strings, each being a unique permutation of the input string. The order of the strings in the output list does not matter.
| Test Case | Expected Result |
|-|-|
| `ab` | `["ab", "ba"]` |
| `abc`| `["abc", "acb", "bac", "bca", "cab", "cba"]`|
| `aba` | `["aba", "aab", "baa"]`|

### Longest Common Subsequence
Given two strings `s1` and `s2`, find the length of the longest common subsequence (LCS). A subsequence is a sequence of characters that appears in the same order in both strings, not necessarily consecutive. The LCS is the longest subsequence that appears in both strings.

| Test Case | Expected Result |
|-|-|
| **(s1, s2)**| |
| (`"abcde"`, `"ace"`) | `3` |
| (`"abc"`, `"abc"`) | `3` |
| (`"abc"`, `"def"`) | `0` |
| (`"abcdgh"`, `"aedfhr"`) | `4` |
| (`"aggtab"`, `"gxtxayb"`) | `4` |



### Balanced Parentheses
Determine if a given input string `s` contains only balanced parentheses.
| Test Case | Expected Result |
|-|-|
| `(1)` | True |
| `(1-2)(3-4)` | True |
| `((1-2)-3)(4+5+6)`| True|
| `)1(` | False|
| `(((1)` | False|
| `((1)-2)` | True |

### Longest Palindromic Substring (Hard)
Find the longest palindromic substring within a given input string `s`. If there are multiple palindromic substrings with the same length, return the first one.
| Test Case | Expected Result |
|-|-|
| `a` | `a` |
| `ab` | `ab` |
| `aba`| `aba`|
| `abacdfgdcabba` | `abba`|

## Practice: Lists

### Remove
Given a list `nums` and a value `val`, remove all occurrences of `val` in the list in-place, and return the new length of the list.

| Test Case | Expected Result |
|-|-|
| **(nums, val)**| |
| (`[3, 2, 2, 3]`, 3) | `2` |
| (`[0, 1, 2, 2, 3, 0, 4, 2]`, 2) | `5` |
| (`[1, 1, 1]`, 1) | `0` |
| (`[1, 2, 3, 4]`, 5) | `4` |

### Merge Two Sorted Lists
Given two sorted lists `list1` and `list2`, merge them into a single sorted list.

| Test Case | Expected Result |
|-|-|
| **(list1, list2)**| |
| (`[1, 3, 5]`, `[2, 4, 6]`) | `[1, 2, 3, 4, 5, 6]` |
| (`[1, 4, 7]`, `[1, 3, 7]`) | `[1, 1, 3, 4, 7, 7]` |
| (`[]`, `[1, 2, 3]`) | `[1, 2, 3]` |
| (`[4, 5, 6]`, `[]`) | `[4, 5, 6]` |

### Sum of Nested Lists
Given a nested list of integers, calculate the sum of all integers. The list can be arbitrarily nested.
| Test Case | Expected Result |
|-|-|
| `[[1, 2], [3, [4, 5]]]` | `15` |
| `[[1, 2, 3], [4], [5, 6]]`| `21`|
| `[1, [2, [3, [4, [5]]]]]` | `15`|
| `[1, [2], [3, 4, [5, 6]]]` | `21` |
| `[[1], [2, 3, [4]], [5, [6, 7, [8]]]]`| `36`|
| `[[], [1], [2, 3], [4, [5, 6]]]` | `21`|

### Find Maximum Value in Nested Lists
Given a nested list of integers, find the maximum value. The list can be arbitrarily nested.

| Test Case | Expected Result |
|-|-|
| **nested_list**| |
| `[[1, 2], [3, [4, 5]]]` | `5` |
| `[[1, 2, 3], [4], [5, 6]]`| `6`|
| `[1, [2, [3, [4, [5]]]]]` | `5`|
| `[1, [2], [3, 4, [5, 6]]]` | `6` |
| `[[1], [2, 3, [4]], [5, [6, 7, [8]]]]`| `8`|
| `[[], [1], [2, 3], [4, [5, 6]]]` | `6`|

## Practice: Extra Recursion
### Even-Odd Numbers
Write two mutually recursive functions, `is_even` and `is_odd`, that determine whether a given non-negative integer is even or odd. The `is_even` function should call `is_odd` and vice versa. The base case is when the number is zero, in which case it is even.
| Test Case | Expected Result |
| --- | --- |
| `is_even(0)` | `True` |
| `is_odd(0)` | `False` |
| `is_even(5)` | `False` |
| `is_odd(5)` | `True` |
| `is_even(10)` | `True` |
| `is_odd(11)` | `True` |

<details>
  <summary>Possible Solution</summary>
  
  ```python
	def is_odd(n):
	    if n == 0:
		return False
	    return is_even(n-1)

	def is_even(n):
	    if n == 0:
		return True
	    return is_odd(n-1)
  ```
  
</details>
