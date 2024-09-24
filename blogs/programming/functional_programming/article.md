# Functional Programming

## What is Functional Programming

> [!NOTE]
> Functional programming is a paradigm, an approach to programming, a way of breaking up the world and putting it back together in code.

Programming paradigms can be divided into two categories: imperative and declarative. The imperative paradigm has two subtypes: procedural, represented by C, and object-oriented, represented by C++ and Java. These languages use loops and mutate states quite often. Procedural programming is very close to assembly language thinking: it eliminates the dreaded "goto" statements by using loops and functions instead. Object-oriented programming, on the other hand, addresses the issue of resource management at the code level by encapsulating related data and the functions that operate on that data, thereby improving code readability and maintainability.

However, functional programming, a subtype of declarative paradigm, is totally another story. Most people start from imperative languages, such as C/C++, Java, Python, etc., and thereby are adapted to this paradigm. When they turn to functional programming, they feel hard to get accustomed to it.

Indeed, functional programming is more abstract and less intuitively, and often requires programmers to do some math. However, every cloud has a silver lining, functional programming has many irresistible advantages, allowing it to shine in specific situations.

### Functional Programming Languages

A functional programming languages treats functions as first class citizens. That is being said, function can be passed as an argument like a scalars. Some languages only support FP paradigm, such as Haskell and Erlang. Nowadays, most programming languages are multi-paradigm, and support both functional programming and object-oriented programming. For instance, Fortran introduced support for object-oriented programming starting from Fortran 2003; Java introduced functional programming features, such as lambda expressions and steams API, in Java 8, which was released in 2014.

Now that Java has FP support, you can be convinced that all prevalent languages must support FP. Nevertheless, the level of support for functional programming varies between languages. For example, in Java, defining a function requires creating a functional interface and learning a bunch of obscure built-in functional interfaces, such as `Runnable`, `BiConsumer`, and so on. Hence, people prefer to use Scala, an awesome FP language that is also run on JVM.

## Basic Concepts

### Immutability

The single most important aspect of functional programming is immutability. Something is considered immutable, if we cannot modify it in some way. Once a variable is set, its value cannot be changed. Consider the following simple for loop in Python that prints the numbers 0 to 99:

```Python
i = 0
while i < 100:
    print(i)
    i += 1
```

This type of code occurs all the time. The variable `i` is mutable and is changed at the end of each iteration of the loop. How to avoid this from happening? The answer is recursion.

```Python
def dump(i):
    if i >= 100:
        return

    print(i)
    return dump(i + 1)

dump(0)
```

The `dump` function above prints the argument `i` and call `dump(i + 1)` to try to print its successor. But the recursion will end when `i` is greater than or equal to 100, in which case the function terminates before printing the number. Now, this code does not mutate any state, but is it a good implementation?

An iteration can be converted into a recursion, and vice versa. But recursion often consumes more resources, as each recursive call adds a new frame to the call stack, which contains information such as the function's parameters, return address, and local variables. For this reason, we need a tool that allows us do the iteration without mutating any states, and there comes the iterator. We will talk about iterator later.

Let us consider another technique for avoiding the mutation of state. Imagine we have a box for oranges, and we use `count` to represent the number of oranges in the box.

```Python
class Box:
    def __init__(self, count):
        self.count = count

box = Box(5)    # The box has 5 oranges at first
box.count += 2  # We add two oranges to the box
```

In this example, the `count` property is mutated outside the `Box` class, which is not allowed in functional programming. That is because, if an instance of box is passed to a function, the function may change its state and cause problems:

```Python
def print_count(box):
    print(box.count)

def malice(box):
    box.count += 1

box = Box(5)
print_count(box)    # 5
malice(box)         # Changed box.count
print_count(box)    # 6, but is supposed to be 5
```

To avoid this potential issue from happening, we should copy and create a new box every time we change the count of the box:

```Python
class Box:
    def __init__(self, count):
        self._count = count

    @property
    def count(self):
        return self._count

def add_oranges(box, count):
    return Box(box.count + count)

box = Box(5)
new_box = add_oranges(box, 2)
print(new_box.count)  # 7
```

For the record, I am not saying that the original `Box` class should no longer be used due to the potential of being mutated. When we write a program, we don't have to stick with a single paradigm. Just use the right tool for the right job.

### Referential Transparency and Purity

**Referential transparency** refers to the property of expressions where they can be replaced with their corresponding values without changing the program's behavior. Simply put, if an expression yields the same result
whenever it is evaluated, regardless of where or when it is used in the program, then the expression is referentially transparent.

```Python
from typing import Final

x: Final[int] = 1
y: Final[int] = 2
z = x + y
```

In this example, both x and y are constants, and therefore, the expression `x + y` is referentially transparent, and can be replaced with its value, `3`, wherever it appears, without changing the program's behavior.

Such an expression can also be a function. A function is called **pure** if it is referentially transparent, and **impure** if it is not. Alternatively, a function can be determined to be pure if it satisfies the following two conditions:

1. **Deterministic**: Given the same inputs, it always produces the same output.
2. **No Side Effects**: It does not cause any observable changes in the system's state outside of returning a value. For example, it does not modify global variables, perform I/O operations, or alter mutable data.

```Python
def square(x):
    return x ** 2
```

The `square` function above is a pure function because given the same input, say 5, it always returns 25. That is why we say pure functions are _predictable_ and _consistent_. Now let us take a look at two common types of impure functions.

```Python
import time

def timestamp():
    return time.time()

PI = 3.14

def circle_area(radius):
    PI = 3.14159
    return PI * radius ** 2
```

Both `timestamp` and `circle_area` are impure. The former is not deterministic because the return value increases over time. The latter has an side effect of changing the global variable `PI`. Beware that throwing errors, considered as a side effect, is also not allowed in a pure function. If you look into any codebase, you will find that most functions are impure: fetching data from database, appending some text to a file, and so forth.

### High Order Functions

We mentioned earlier that functional programming treat functions as first class citizens. This means we should be able to pass them as function parameters and return them from functions as return values. At the heart of functional programming, we are seeking ways to simplify the code using this feature.

```Python
def square(num):
    return num ** 2

def square_list_imperative(nums):
    squared = []
    for num in nums:
        squared.append(square(num))

    return squared

def square_list_fp(nums):
    return map(square, nums)
```

Both `square_list_imperative` and `square_list_fp` functions do the same thing, but obviously the latter is more terse and elegant. Before you get completely enchanted by functional programming, let me pour some cold water on the idea: functional programming isn't always more concise than imperative programming; sometimes it can get pretty messy and crappy. We should strive for a balance between different programming paradigms to make the most of each.

Let us examine another example, where functions as return values.

```Python
def add(x, y):
    return x + y

def multiply(x, y):
    return x * y

def mux(op_code):
    return [add, multiply][op_code]

def apply(op_code, x, y):
    return mux(op_code)(x, y)

print(apply(1, 2, 3))  # Print 6 because 2 * 3 = 6
```

In this example, the `mux` function accepts an `op_code` (integer type, either 0 or 1), and returns a corresponding function. More specifically, `mux(0)` returns the function `add`, while `mux(1)` returns the function `multiply`. This is a frequently used pattern, which makes code more straightforward and easy to maintain.

### Lazy Evaluation

Lazy evaluation means an expression is not evaluated until it is needed. Although it is not an essential feature for FP, most FP languages tend to be lazy. However, most popular languages, including Python, are not completely lazy, they use eager evaluation instead. This means an expression is evaluated the first time it is encountered.

```Python
award = get_cake() if is_won else get_candy()
```

The conditional expression on the right hand side of `=` is known as ternary operator, which is evaluated in a lazy manner. If `is_won` is true, only `get_cake()` will be evaluated, vice versa.

## Category Theory

## Functional Patterns

### Functor

A functor is any object or structure that allows us to apply a function to its contents, transforming those contents while preserving the structure itself. In python, a list is a functor, and we can use `map` function to apply a function to its contents, namely the elements in the list. Note that `map` function returns a map object, so we need to use `list` function to convert it to a list.

```python
def add_one(num):
    return num + 1

nums = [1, 2, 3]
nums = list(map(add_one, nums))

print(nums)  # [2, 3, 4]
```

We can also write a functor class on our own:

```Python
class Functor:
    def __init__(self, value) -> None:
        self.value = value

    def fmap(self, func) -> "Functor":
        return Functor(func(self.value))

    def __repr__(self):
        return f"Functor({self.value})"


f1 = Functor(5)
f2 = f1.fmap(lambda x: x + 1)
print(f1)  # Functor(5)
print(f2)  # Functor(6)
```

In the above example, a functor contains a value, and we can apply a function on that value using the `fmap` method, which returns a new functor containing the new value.

### Monoid

A monoid consists of:

1. Set of values: The collection of elements that can be combined using the binary operation.
2. Binary Operation: A function (let's call it `combine`) that takes two elements from the set and returns another element from the same set and returns another element from the same set.
3. Identity Element: An element `e` in the set such that for any element `a` in the set, we have `e combine a` equals `a combine e` equals `a`.

For example, integer addition is a monoid. Therefore, given an integer list an `add` function, we can find the summation of the list:

```Python
from functools import reduce

def add(x, y):
    return x + y

e = 0  # Identity element

nums = [1, 3, 5, 7]
result = reduce(add, nums, 0)
print(result)
```

In the above example, the
