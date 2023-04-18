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

Note that lambda functions are limited in their functionality compared to regular Python functions (defined using `def`). They are mainly used for simple operations where a full function definition would be unnecessarily verbose.

Even though lambdas may seem named when you assign them to variables, they are still unnamed.
Demonstration:
```python
sum2 = lambda x: sum(x)
sum3 = sum
print(sum)  # <built-in function sum>
print(sum2) # <function <lambda> at 0x7f49c2a8bb00>
print(sum3) # <built-in function sum>
```

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
  
---

### Questions

 <details>
  <summary> <b>Question 1</b> </summary>
  
  Evaluate:
  ```python
(lambda a, b, c: (a * b) + c)(3, 4, 5)
  ```
  
   <details>
    <summary>Question 1 Answer</summary>
  The given expression can be evaluated step-by-step:

  1. The outer lambda function has three arguments `a`, `b`, and `c`. It calculates the product of `a` and `b`, then adds `c` to the result.
  2. `a` is `3`.
  3. `b` is `4`.
  4. `c` is `5`.
  5. The lambda function calculates the product of `a` and `b`, which is `3 * 4`.
  6. The result is `12`.
  7. The lambda function adds `c` to the result, which is `12 + 5`.
  8. The final result is `17`.

  So, the evaluated expression is `17`.
     
  </details>
</details>



 <details>
  <summary> <b>Question 2</b> </summary>
  
  Evaluate:
  ```python
(lambda x, y, z: x(y) + z)(lambda x: x ** 2, 3, 4)
  ```
  
   <details>
    <summary>Question 2 Answer</summary>
  The given expression can be evaluated step-by-step:

  1. The outer lambda function has three arguments `x`, `y`, and `z`. It applies `x` to `y` and then adds `z` to the result.
  2. `x` is a lambda function: `(lambda a: a ** 2)`. This function takes a single argument `a` and returns its square.
  3. `y` is `3`.
  4. `z` is `4`.
  5. The lambda function applies `x` to `y`, which is `x(3)`.
  6. The result is `3 ** 2`, which is `9`.
  7. The lambda function adds `z` to the result, which is `9 + 4`.
  8. The final result is `13`.

  So, the evaluated expression is `13`.
     
  </details>
</details>




 <details>
  <summary> <b>Question 3</b> </summary>
  
  Evaluate:
  ```python
(lambda p, q, r: p(q) * r)(lambda r: r - 1, 6, 2)
  ```
  
   <details>
    <summary>Question 3 Answer</summary>
The given expression can be evaluated step-by-step:

1. The outer lambda function has three arguments `p`, `q`, and `r`. It applies `p` to `q` and then multiplies the result by `r`.
2. `p` is a lambda function: `(lambda b: b - 1)`. This function takes a single argument `b` and returns the value of `b` decremented by `1`.
3. `q` is `6`.
4. `r` is `2`.
5. The lambda function applies `p` to `q`, which is `p(6)`.
6. The result is `6 - 1`, which is `5`.
7. The lambda function multiplies the result by `r`, which is `5 * 2`.
8. The final result is `10`.

So, the evaluated expression is `10`.
     
  </details>
</details>




<details>
  <summary> <b>Question 4</b> </summary>
  
  Evaluate:
  ```python
  (lambda a, b, c: a(b) * a(c))((lambda a: a * 3), 2, 4)
  ```
  
   <details>
    <summary>Question 4 Answer</summary>
     
  1. The outer lambda function has three arguments `a`, `b`, and `c`. It applies `a` to `b` and `c` and then multiplies the results.
  2. `a` is another lambda function: `(lambda x: x * 3)`. This function takes a single argument `x` and returns its triple.
  3. `b` is `2` and `c` is `4`.
  4. Applying `a` to `b` and `c` gives us `a(2)` and `a(4)`, which are `2 * 3` and `4 * 3`, respectively.
  5. The result is `6 * 12`, which is `72`.

  So, the evaluated expression is `72`.
     
  </details>

</details>

 <details>
  <summary> <b>Question 5</b> </summary>
  
  Evaluate:
  ```python
(lambda a, b, c, d: (a(b), c(d)))(lambda a: a // 2, 8, lambda a: a % 3, 11)
  ```
  
   <details>
    <summary>Question 5 Answer</summary>
     
  1. The outer lambda function has four arguments `a`, `b`, `c`, and `d`. It applies `a` to `b` and `c` to `d`, and returns the results as a tuple.
  2. `a` is a lambda function: `(lambda x: x // 2)`. This function takes a single argument `x` and returns the integer division of `x` by 2.
  3. `b` is `8`.
  4. `c` is a lambda function: `(lambda y: y % 3)`. This function takes a single argument `y` and returns the remainder of `y` divided by 3.
  5. `d` is `11`.
  6. Applying `a` to `b` gives us `a(8)`, which is `8 // 2`.
  7. The result is `4`.
  8. Applying `c` to `d` gives us `c(11)`, which is `11 % 3`.
  9. The result is `2`.
  10. The tuple (4, 2) is returned.
     
  So, the evaluated expression is `(4, 2)`.
     
  </details>

</details>
  
 </details>



 <details>
  <summary> <b>Question 6</b> </summary>
  
  Evaluate:
  ```python
  (lambda f, g, h, x: f(g(x), h(x)))(lambda f, g: f * g, lambda h: h + 2, lambda f: f ** 2, 3)
  ```
  
   <details>
    <summary>Question 6 Answer</summary>

  1. The outer lambda function has four arguments `f`, `g`, `h`, and `x`. It applies `g` and `h` to `x` and then applies `f` to the results of those applications.
  2. `f` is a lambda function: `(lambda x, y: x * y)`. This function takes two arguments `x` and `y` and returns their product.
  3. `g` is a lambda function: `(lambda x: x + 2)`. This function takes a single argument `x` and returns `x` plus 2.
  4. `h` is a lambda function: `(lambda x: x ** 2)`. This function takes a single argument `x` and returns the square of `x`.
  5. `x` is `3`.
  6. Applying `g` and `h` to `x` gives us `g(3)` and `h(3)`, which are `3 + 2` and `3 ** 2`, respectively.
  7. The result is `5` and `9`.
  8. Applying `f` to the results of `g(3)` and `h(3)` gives us `f(5, 9)`, which is `5 * 9`.
  9. The result is `45`.

  So, the evaluated expression is `45`.
     
  </details>
</details>
  
  






 <details>
  <summary> <b>Question 7</b> </summary>
  
  Evaluate:
  ```python
  (lambda f, lst: list(map(f, lst)))(lambda f: f ** 2, [1, 2, 3, 4])
  ```
  
   <details>
    <summary>Question 7 Answer</summary>


  1. The outer lambda function has two arguments `f` and `lst`. It applies `f` to each element of `lst` using the `map` function and returns the result as a list.
  2. `f` is a lambda function: `(lambda x: x ** 2)`. This function takes a single argument `x` and returns the square of `x`.
  3. `lst` is `[1, 2, 3, 4]`.
  4. Applying `map` to `f` and `lst` gives us a new iterable with the squares of the elements in `lst`.
  5. The result is `[1, 4, 9, 16]`.

  So, the evaluated expression is `[1, 4, 9, 16]`.
     
  </details>
</details>



 <details>
  <summary> <b>Question 8</b> </summary>
  
  Evaluate:
  ```python
  (lambda f, lst: list(filter(f, lst)))(lambda x: x % 2 == 0, [1, 2, 3, 4, 5, 6])
  ```
  
   <details>
    <summary>Question 8 Answer</summary>
     
  The given expression can be evaluated step-by-step:

  1. The outer lambda function has two arguments `f` and `lst`. It filters `lst` using the `filter` function with `f` as the filtering function and returns the result as a list.
  2. `f` is a lambda function: `(lambda x: x % 2 == 0)`. This function takes a single argument `x` and returns `True` if `x` is even and `False` otherwise.
  3. `lst` is `[1, 2, 3, 4, 5, 6]`.
  4. Applying `filter` to `f` and `lst` gives us a new iterable with only the even elements of `lst`.
  5. The result is `[2, 4, 6]`.

  So, the evaluated expression is `[2, 4, 6]`.
     
  </details>
</details>




 <details>
  <summary> <b>Question 9</b> </summary>
  
  Evaluate:
  ```python
  (lambda f, lst: f(lambda x, y: x * y, lst))(lambda func, items: reduce(func, items), [1, 2, 3, 4])
  ```
  
   <details>
    <summary>Question 9 Answer</summary>

  The given expression can be evaluated step-by-step:

  1. The outer lambda function has two arguments `f` and `lst`. It applies `f` to a lambda function and `lst`.
  2. `f` is a lambda function: `(lambda func, items: reduce(func, items))`. This function takes two arguments `func` and `items` and reduces `items` using the `reduce` function with `func` as the reducing function.
  3. The lambda function `(lambda x, y: x * y)` is a function that takes two arguments `x` and `y` and returns their product.
  4. `lst` is `[1, 2, 3, 4]`.
  5. Applying `f` to the lambda function and `lst` gives us the result of reducing `lst` by multiplying its elements: `reduce(lambda x, y: x * y, [1, 2, 3, 4])`.
  6. The result is `1 * 2 * 3 * 4`, which is `24`.

  So, the evaluated expression is `24`.
     
  </details>
</details>


 <details>
  <summary> <b>Question 10</b> </summary>
  
  Evaluate:
  ```python
 list(filter (lambda x : 10 % x, filter (lambda x: x % 2,[1,2,3,5,7,64])))
  ```
  
   <details>
    <summary>Question 10 Answer</summary>

  1. You have a list of numbers: [1, 2, 3, 5, 7, 64].
  2. The first filter function has a lambda function (lambda x: x % 2) which filters out only odd numbers from the list, as x % 2 will be non-zero (truthy) for odd numbers.
  3. After applying the first filter, you get the list [1, 3, 5, 7].
  4. The second filter function has a lambda function (lambda x: 10 % x), which checks if 10 is divisible by x. If 10 is not divisible by x, the result will be non-zero (truthy), and the element will be included in the filtered list.
  5. After applying the second filter to the list [1, 3, 5, 7], you get the list [3, 7], as 10 is not divisible by 3 or 7.

  So, the evaluated expression is [3, 7].
     
  </details>
</details>


 <details>
  <summary> <b>Question 11</b> </summary>
  
  Evaluate:
  ```python
(lambda x, y: x(2, 3) + y(4))(lambda x, y: x * y, lambda y: y ** 2)
  ```
  
   <details>
    <summary>Question 11 Answer</summary>

  1. The outer lambda function has two arguments `f` and `g`. It applies `f` to `2` and `3`, and `g` to `4`, then adds the results.
  2. `f` is a lambda function: `(lambda x, y: x * y)`. This function takes two arguments `x` and `y` and returns their product.
  3. `g` is a lambda function: `(lambda z: z ** 2)`. This function takes a single argument `z` and returns its square.
  4. Applying `f` to `2` and `3` gives us `f(2, 3)`, which is `2 * 3`.
  5. The result is `6`.
  6. Applying `g` to `4` gives us `g(4)`, which is `4 ** 2`.
  7. The result is `16`.
  8. The sum of the results is `6 + 16`, which is `22`.

  So, the evaluated expression is `22`.
     
  </details>
</details>






 <details>
  <summary> <b>Question 12</b> </summary>
  
  Evaluate:
  ```python
  (lambda f, lst: f(lambda x: reduce(lambda x, y: x * y, x), lst))(
    lambda func, items: list(map(func, items)),
    [[1, 2], [3, 4], [5, 6]]
  )
  ```
  
   <details>
    <summary>Question 12 Answer</summary>

  1. The outer lambda function has two arguments `f` and `lst`. It applies `f` to a lambda function and `lst`.
  2. `f` is a lambda function: `(lambda func, items: list(map(func, items)))`. This function takes two arguments `func` and `items` and applies `func` to each element of `items` using the `map` function, returning the result as a list.
  3. The lambda function inside `f` is `(lambda x: reduce(lambda a, b: a * b, x))`. This function takes a single argument `x` and reduces it using the `reduce` function with a lambda function that multiplies its arguments.
  4. `lst` is `[[1, 2], [3, 4], [5, 6]]`.
  5. Applying `f` to the lambda function and `lst` gives us a list where the lambda function is applied to each element of `lst`: `list(map(lambda x: reduce(lambda a, b: a * b, x), lst))`.
  6. The lambda function applies the `reduce` function to each element of `lst`. This results in a list of products: `[1 * 2, 3 * 4, 5 * 6]`.
  7. The final result is `[2, 12, 30]`.

  So, the evaluated expression is `[2, 12, 30]`.
     
  </details>
</details>






 <details>
  <summary> <b>Question 13</b> </summary>
  
  Evaluate:
  ```python
tuple(map(lambda x:x[0]+x[1],((1,2),(3,4))))
  ```
  
   <details>
    <summary>Question 13 Answer</summary>

  1. The outer function is tuple. It takes an iterable as an argument and returns a tuple containing the elements of the iterable.
  2. The map function is applied to a lambda function and a tuple of tuples. The lambda function takes a single tuple argument and returns the sum of the first and second elements of the tuple. The tuple of tuples is ((1, 2), (3, 4)).
  3. The lambda function is (lambda x: x[0] + x[1]). It takes one tuple argument x and returns the sum of the first element x[0] and the second element x[1].
  4. Applying the map function to the lambda function and the tuple of tuples results in an iterable with the sum of the elements in each tuple: [1 + 2, 3 + 4].
  5. The iterable created by the map function is [3, 7].
  6. The tuple function takes the iterable [3, 7] and returns a tuple containing the elements of the iterable.

  So, the evaluated expression is (3, 7)
     
  </details>
</details>





 <details>
  <summary> <b>Question 14</b> </summary>
  
  Evaluate:
  ```python
(lambda x,y,z:x(y)+x(z))((lambda x:x*2),4,5)
  ```
  
   <details>
    <summary>Question 14 Answer</summary>
     
    1. The outer lambda function has three arguments x, y, and z. It applies x to y and x to z and returns the sum of the results.
    2. x is a lambda function: (lambda x: x * 2). This function takes one argument x and returns the product of x and 2.
    3. y is 4.
    4. z is 5.
    5. Applying x to y and z gives us: x(4) + x(5).
    6. The lambda function (lambda x: x * 2) is applied to the arguments 4 and 5, resulting in 4 * 2 and 5 * 2, which are 8 and 10 respectively.
    7. The final result is the sum of the two results: 8 + 10.

    So, the evaluated expression is 18.
   ...
     
  </details>
</details>






 <details>
  <summary> <b>Question 15</b> </summary>
  
  Evaluate:
  ```python
(lambda x: (lambda x : x)(lambda x: x+x)(x))(2)
  ```
  
<details>
  <summary>Question 12 Answer</summary>
  
  1. The outer lambda function has one argument x. It applies an inner lambda function to another lambda function and the argument x.
  2. The inner lambda function is (lambda x : x). This function takes one argument x and returns it as-is.
  3. The lambda function inside the inner lambda function is (lambda x: x + x). This function takes a single argument x and returns the sum of x and x.
  4. x is 2.
  5. Applying the inner lambda function to the (lambda x: x + x) function and x=2 gives us: (lambda x: x)(4).
  6. The inner lambda function, (lambda x: x), now takes the argument 4 and simply returns it.
  7. The final result is 4.

  So, the evaluated expression is 4.

</details>






 <details>
  <summary> <b>Question 16</b> </summary>
  
  Evaluate:
  ```python
def combinator(y):
  return (lambda x: lambda y: x(y))(lambda x:y)
combinator(lambda x:x*10)(11)(12)
  ```
  
   <details>
    <summary>Question 16 Answer</summary>
     
  The given code defines a function called combinator that takes a single argument y. The function returns a lambda function that takes another argument x, which itself returns a lambda function that takes a single argument y.

  The innermost lambda function simply returns the value of y. The second lambda function takes y as an argument and returns the result of applying the first lambda function to y. The first lambda function takes x as an argument and returns the result of applying x to y.

  When we call combinator(lambda x:x*10)(11), it returns a lambda function that takes a single argument y. We then call this function with 12 as the argument, which applies the first lambda function (lambda x:x*10) to 12. This returns 120, which is the result of 12 * 10.

  So, the final output of the code is 120.
     
  </details>
</details>
