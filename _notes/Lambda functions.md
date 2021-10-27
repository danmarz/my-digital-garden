---
Title: Lambda functions in Python
---

# Lambda functions in Python

Lambda functions are small, anonymous, throw-away functions, i.e. they are needed where they have been created.

1. **How can I use this?**
	-	`function_name_var = lambda argument_list: expression`
	-	i.e. `sum = lambda x, y : x + y`	-> sum(3,4) outputs 7

2. **Why must I use this?**
	- The advantage of the lambda operator can be seen when used in combination with the `map()`, `filter()` and `reduce()` functions

3. **When will I use this?**
### Filter() function
The function `filter(function, sequence)` offers an elegant way to filter out all elements of a sequence "sequence" for which the function returns _True_. In other words the filter() function needs a `function` as the first argument that **needs to return a Boolean value**, i.e either _True_ or _False_. The function will then be applied to every element of the list `sequence`. Only if `function` returns _True_ will the element be produced by the iterator, which is the return value of `filter(function, sequence)`.

```py
fibonacci = [0,1,1,2,3,5,8,13,21,34,55]
odd_numbers = list(filter(lambda x: x % 2, fibonacci))
print(odd_numbers) 
// [1, 1, 3, 5, 13, 21, 55]

even_numbers = list(filter(lambda x: x % 2 == 0, fibonacci))
print(even_numbers)

// [0, 2, 8, 34]
// Same as:
even_numbers = list(filter(lambda x: x % 2 -1, fibonacci))
print(even_numbers)

//[0, 2, 8, 34]

```

### Map() function
The function `map(function, sequence)` applies the function `function` to all elements of the list `sequence` and returns an iterator with the resulting values in Py3.

By using lambda, we wouldn't have to define and name the functions fahrenheit() and celsius() to transform the values between the metrics:

```py
C = [39.2, 36.5, 37.3, 38, 37.8] 
F = list(map(lambda x: (float(9)/5)*x + 32, C))
print(F)

[102.56, 97.7, 99.14, 100.4, 100.03999999999999]
```

map() can be applied to more than one list. The lists don't have to have the same length. map() will apply its lambda function to the elements of the argument lists, i.e. it first applies to the elements with the 0th index, then to the elements with the 1st index until the n-th index is reached:

```py
a = [1, 2, 3, 4]
b = [17, 12, 11, 10]
c = [-1, -4, 5, 9] 

list(map(lambda x, y, z : x+y+z, a, b, c))

Output:
[17, 10, 19, 23]
```

**Note**:   
If one list has fewer elements than the others, map() will stop when the shortest list has been consumed.

### Reduce() function
The function `reduce(function, sequence)` continually applies the function `function()` to the sequence `sequence`. It returns a single value.

![Reduce](https://www.python-course.eu/images/reduce.webp)

```py
# find max() value in a list of numbers
print(reduce(lambda a, b: a if (a > b) else b, [47, 11, 42, 102, 13]))

# sum of the numbers from 1 to 100:
print(reduce(lambda x, y: x + y, range(1, 101)))

# factorial (the product) of the numbers from 1 to 49
print(reduce(lambda x, y: x * y, range(1, 49)))
```

# ---

Tags: #python #lambda
Topics: [[Python]]
