## Higher-order Functions
**by Jing Wen**

A higher-order function is a function that takes one or more functions as arguments, returns a function as a result, or both. They allow you to create more generic, reusable, and composable (modular with no side effects) code. 

In Python, functions are first-class objects, which means you can pass them around like any other object, such as integers or strings.

You can define your own higher-order function by passing a function as an argument:
  ```python
from time import time

def time_it(f, x):
    start_time = time()
    ret = f(x)
    print(f'Your function took {time()-start_time} to run!')
    return ret

f = sum
x = [i for i in range(100000)]
result = time_it(f, x)
print(result)
```

---

### Questions

<details>
  <summary> <b>Question 1</b> </summary>
  
  Fix my code!
  
  ```python
def sum_until(n):
    """Returns a function that sums the first n elements of a given list.

    Arguments:
        n (int): The number of elements to include in the sum.

    Returns:
        function: A function that takes a list as input and returns the sum
                  of the first n elements in the list.
    """
    def sum_all(m)
        return ...
    return ...
  ```
  
   <details>
    <summary>Question 1 Answer</summary>
     
   ```python
def sum_until(n):
    def sum_all(m)
        return sum(m[:n])
    return sum_all
  ```
     
  </details>

</details>

<details>
  <summary> <b>Question 2</b> </summary>
  
  ```python
def a(b, y):
    return y==0 or not b(a, y-1)
  
def x(y, a):
    return a==0 or not y(x, a-1)
  ```
which expressions are `True`:
- [ ] a(x, 2)
- [ ] x(a, 2)
- [ ] a(x, 3)
- [ ] x(a, 3)
  
   <details>
    <summary>Question 2 Answer</summary>
     
  - [x] a(x, 2)
  - [x] x(a, 2)
  - [ ] a(x, 3)
  - [ ] x(a, 3)
     
  </details>

</details>

<details>
  <summary> <b>Question 3</b> </summary>
  
  Given:
  ```python
  def f1(x):
    return [x]
  
  def f2(f, y):
    return f(3) * y
  
  def f3(x, y):
    z = f1(y) + f2(f1, x)
    if not f1(len(z)//10):
      z = f2(list, f1(z))
      return z
    return f2(str, sum(z))
  ```
  which expressions are `True`:
- [ ] f2(tuple, 4) == (3, 3, 3, 3, 3)
- [ ] f2(max, 1) == 3
- [ ] f3(2, 3) == '333333333'
- [ ] f3(2, 3) == [3, 3, 3, 3, 3, 3, 3, 3, 3]
- [ ] f1(f2(int, f1(f2(f1, 1)))) == [[[3], [3], [3]]]
  
  <details>
    <summary>Question 3 Answer</summary>
    
  - [ ] f2(tuple, 4) == (3, 3, 3, 3, 3) 
    ```diff
    - error because tuple(3)
    ```
  - [ ] f2(max, 1) == 3
    ```diff
    - error because max(3)
    ```
  - [x] f3(2, 3) == '333333333'
    ```diff
    ! Note that len(f1(z//10)) always = 1 as f1 returns list of length 1, so if branch is never taken
    + f3(2, 3)
    + = f2(str, sum(z))
    + = f2(str, sum(f1(y) + f2(f1, x)))
    + = f2(str, sum(f1(3) + f2(f1, 2)))
    + = f2(str, sum([3] + f2(f1, 2)))
    + = f2(str, sum([3] + f1(3) * 2))
    + = f2(str, sum([3] + [3] * 2))
    + = f2(str, sum([3, 3, 3]))
    + = f2(str, 9)
    + = str(3) * 9
    + = 333333333
    ```
  - [ ] f3(2, 3) == [3, 3, 3, 3, 3, 3, 3, 3, 3]
    ```diff
    - the return statement is the str one
    ```
  - [x] f1(f2(int, f1(f2(f1, 1)))) == [[[3], [3], [3]]]
    
  </details>
  
</details>

---


    
Some common higher-order functions provided to you in Python are `map()`, `filter()`, and `reduce()`:

- `map()`: Applies a given function to all items in an iterable (e.g., list or tuple) and returns a map object (which can be converted to a list or other iterable).
  Example:
  
  ```python
  def square(x):
      return x**2

  numbers = [1, 2, 3, 4]
  result = map(square, numbers)
  print(list(result))  # Output: [1, 4, 9, 16]
  ```
  
- `filter()`: Filters the items in an iterable based on a function that returns a boolean (True or False), called a predicate. The function is applied to each item, and the ones that return True are retained.

  Example:
  ```python
  def is_even(x):
      return x % 2 == 0

  numbers = [1, 2, 3, 4]
  result = filter(is_even, numbers)
  print(list(result))  # Output: [2, 4]
  ```
  
- `reduce()`: Applies a function with two arguments cumulatively to the items of an iterable, reducing the iterable to a single value. The reduce() function is found in the `functools` module (**not** imported by default in python).
    ```python
  from functools import reduce # important

  def multiply(x, y):
      return x * y

  numbers = [1, 2, 3, 4]
  result = reduce(multiply, numbers)
  print(result)  # Output: 24
  ```

## Lambdas
In Python, lambda is a keyword used to create small, anonymous (unnamed) functions, also known as lambda functions. These functions can be used wherever function objects are required, such as when you want to pass a simple function as an argument to another function. 

Lambda functions are limited to a single expression and cannot contain statements or include complex logic. You can use lambda functions as arguments for higher-order functions when you don't want to define a separate, full function.

The general syntax for a lambda function is:

```python
lambda arguments: expression
```

which is similar to

```python
def <unnamed>(arguments):
  return expression
```

For example:
```python
# Define a lambda function that takes two arguments and returns their sum
add = lambda x, y: x + y

# Use the lambda function
result = add(5, 3)
print(result)  # Output: 8
```

It is important to note that lambda functions are limited in their functionality compared to regular Python functions (defined using `def`). They are mainly used for simple operations where a full function definition would be unnecessarily verbose.

You can use lambdas with higher-order functions like `map()`, `filter()`, and `reduce()`:

```python
numbers = [1, 2, 3, 4]
result = map(lambda x: x**2, numbers)
print(list(result))  # Output: [1, 4, 9, 16]
```

```python
numbers = [1, 2, 3, 4]
result = filter(lambda x: x % 2 == 0, numbers)
print(list(result))  # Output: [2, 4]
```

```python
from functools import reduce

numbers = [1, 2, 3, 4]
result = reduce(lambda x, y: x * y, numbers)
print(result)  # Output: 24
```

## Nested Lambdas
Nested lambdas refer to the use of one or more lambda functions within another lambda function. This is useful when you need to create functions that either take other functions as arguments or return functions as their result.

```python
# Define a lambda function that returns another lambda function
multiply_by = lambda x: (lambda y: x * y)

# Create a lambda function that multiplies its argument by 5
times_five = multiply_by(5)

# Use the created lambda function
result = times_five(3)
print(result)  # Output: 15
```

The breakdown of this code is as follows:

```python
times_five = multiply_by(5)
times_five = (lambda x: (lambda y: x * y))(5)
times_five = lambda y: 5 * y

result = times_five(3)
result = (lambda y: 5 * y)(3)
result = 5 * 3
result = 15
```
  
  
  
  
  
