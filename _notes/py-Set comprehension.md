---
Title: Set comprehension in Python
---

# Set comprehension in Python

A _set comprehension_ is similar to a list comprehension, but returns a set and not a list. Syntactically, we use curly brackets `{}` instead of square brackets to create a set.

1. **How can I use this?**
	In  _[[py-Lambda functions]]_ and `map()` we designed a `map()` function to convert Celsius values into Fahrenheit and vice versa. It looks like this with set comprehension:
	```py
	Celsius = [39.2, 36.5, 37.3, 37.8]
	
	Fahrenheit = { (( float(9) / 5 ) * x + 32) for x in Celsius }

	print(Fahrenheit)
	### -> {102.56, 97.7, 99.14, 100.03999999999999}
	```
2. **Why must I use this?**
	Set comprehension is a complete substitute for the _lambda_ function as well as the functions `map()`, `filter()` and `reduce()`. For most people the syntax of list comprehension is easier to be grasped.

3. **When will I use this?**
	When we need a set as output. For when we need _list_ functionality we have a similar syntax called [[py-List comprehension]] which is similar to set comprehension, but returns a list (which by definition can contain duplicate values) and not a set. 
	
# ---

Tags: #coding
Topics: [[Python]] [[py-List comprehension]]

