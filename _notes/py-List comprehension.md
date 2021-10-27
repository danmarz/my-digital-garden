---
Title: List comprehension in Python
---

# List comprehension in Python

List comprehension is an elegant way to define and create lists in Python. These lists have the quality of sets, but are not necessarily sets.

1. **How can I use this?**
	In  _[[py-Lambda functions]]_ and `map()` we designed a `map()` function to convert Celsius values into Fahrenheit and vice versa. It looks like this with list comprehension:
	```py
	Celsius = [39.2, 36.5, 37.3, 37.8]
	
	Fahrenheit = [ (( float(9) / 5 ) * x + 32) for x in Celsius ]

	print(Fahrenheit)
	### -> [102.56, 97.7, 99.14, 100.03999999999999]
	```
2. **Why must I use this?**
	List comprehension is a complete substitute for the _lambda_ function as well as the functions `map()`, `filter()` and `reduce()`. For most people the syntax of list comprehension is easier to be grasped.

3. **When will I use this?**
	When we need a list as output. For when we need _set_ functionality we have a similar syntax called [[py-Set comprehension]] which is similar to a list comprehension, but returns a set (which by definition cannot contain duplicate values) and not a list. Syntactically, we use curly brackets `{}` instead of square brackets to create a set.

# ---

Tags: #coding
Topics: [[Python]]

