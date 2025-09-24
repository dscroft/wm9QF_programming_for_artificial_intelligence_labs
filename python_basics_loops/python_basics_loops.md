<!--
module_id: python_basics_loops
author:   Meredith Lee
email:    leemc@chop.edu
version: 1.2.2
current_version_description: Replaced SageMathCells with Pyodide cells for better usability
module_type: standard
docs_version: 1.2.0
language: en
narrator: UK English Female
mode: Textbook

title: Python Basics: Loops

comment: Learn how to use loops in Python. 

long_description: Before using Python for data analysis, there are some basics to learn that will set the foundation for more advanced Python coding. This module will teach you about how to loop through sequences. 

estimated_time_in_minutes: 20

@pre_reqs
Learners should be familiar with using [functions and methods](https://liascript.github.io/course/?https://raw.githubusercontent.com/arcus/education_modules/main/python_basics_variables_functions_methods/python_basics_variables_functions_methods.md#1) and [collections](https://liascript.github.io/course/?https://raw.githubusercontent.com/arcus/education_modules/main/python_basics_lists_dictionaries/python_basics_lists_dictionaries.md#1) at a beginner level. 
@end

@learning_objectives

- Iterate through lists using loops

@end

good_first_module: false
collection: learn_to_code
sequence_name: python_basics
previous_sequential_module: python_basics_lists_dictionaries
coding_required: true
coding_level: basic
coding_language: python

@sets_you_up_for

- python_basics_exercise
- pandas_transform

@end

@depends_on_knowledge_available_in

- demystifying_python
- python_basics_variables_functions_methods
- python_basics_lists_dictionaries

@end


@version_history

Previous versions: 

- [1.0.1](https://liascript.github.io/course/?https://raw.githubusercontent.com/arcus/education_modules/5e1bbae6792dc5adc7cfcc99860b0f9e1447daa6/python_basics_loops/python_basics_loops.md#) Initial version
@end

link:  ../assets/styles.css
import: ../module_templates/macros.md
import: ../module_templates/macros_python.md
import: https://dscroft.github.io/Pyodide/README.md
import: https://github.com/LiaScript/CodeRunner/blob/master/README.md
-->

# Python Basics: Loops and Conditional Statements

@overview

## Attribution

@attribution

## Lesson Preparation

@sage

@lesson_prep_python_pyodide

## Introduction

In programming, we often need to perform a task repeatedly (or **iteratively**), or only if certain conditions are met. Iterating over an series of inputs and performing a specified task on each input is accomplished using **loops**.  Loops are not unique to Python-- they appear in almost every language! This module will discuss how these concepts work in Python. 

## Loops

In Python, [lists and dictionaries](https://liascript.github.io/course/?https://raw.githubusercontent.com/arcus/education_modules/main/python_basics_lists_dictionaries/python_basics_lists_dictionaries.md#1) are examples of **iterable** objects, or objects whose members can be returned one at a time. This is important in **loops**, which repeat (or iterate) the same operation for each element in an iterable object like a list.

For a sense of how loops work in Python, here is a simple example. Let's say there are five children at a party, and they each start with a certain number of pieces of candy. Next, let's say that we give each child 5 more pieces of candy. Using a simple loop, we can get a list of how many total pieces of candy each child has now using the `for` and `in` keywords.
    

```python
starting_candy = [3, 10, 11, 6, 7]
candy_day1 = [] #Here we are initiating an empty list, so that we can add elements to it later
for i in starting_candy: #In the loop, i will take on the value of each list element in turn
	j = i + 5
	candy_day1.append(j)
print(candy_day1)
```
@Pyodide.eval

<div class = "important">
<b style="color: rgb(var(--color-highlight));">Important note</b><br>

Did you notice the explanatory text in the above code cell that started with a pound sign (or hash mark)? The pound sign / hash mark ( # ) in a code cell indicates the start of a **comment**. Comments aren't recognized as code and won't be run. Each line of a comment must be marked with a new pound sign. Comments are an excellent way to add brief explanations and clarifications about your code.

</div>

This example loops through the list of numbers, adds 5 to each number one at a time, and adds the sum to a new list using the `.append()` method for lists. Finally, we printed the new list to our screen. This kind of loop is sometimes called a **for loop**; there is another kind of loop called a **while loop**, which is often used when we don't know the number of times we'll have to iterate through a block of code before we start ([check out this page for more information about while loops](https://www.geeksforgeeks.org/python-while-loop/)).


### Indentation

```python
starting_candy = [3, 10, 11, 6, 7]
candy_day1 = [] #Here we are initiating an empty list, so that we can add elements to it later
for i in starting_candy: #In the loop, i will take on the value of each list element in turn
	j = i + 5
	candy_day1.append(j)
print(candy_day1)
```
@Pyodide.eval

The code cell above is exactly the same as the one on the previous page. Notice that the two lines of code after the `for` statement are **indented**. This indentation is important in Python, and not just for readability! Indents indicate blocks of code, or lines of code that do a specific thing. In this case, the two lines after the `for` statement are in a different code block than the rest; they comprise the body of the loop. Indentation tells Python what statements to evaluate in what order. You also may have noticed a colon (:) at the end of the `for` statement. This tells Python that the following lines are a new code block and should be indented. All of the lines of a code block need to be indented the same number of spaces.

Try changing the code cell above by removing the indentation before `candy_day1.append(j)` in line 5. How does the output change? Why do you think it changes the way that it does?

<div class = "care">
<b style="color: rgb(var(--color-highlight));">A little encouragement...</b><br>

Indentation is tricky, especially for complex nested loops; experienced Python programmers sometimes struggle with this, so don't worry if it takes some trial and error to figure out! If you end up struggling with a tricky loop, try isolating the individual pieces, get those working, and then build outward. [Drawing a flowchart of the problem](https://problemsolvingwithpython.com/09-Loops/09.04-Flowcharts-Describing-Loops/) can also help!

</div>


### Range
The built-in function `range()` is frequently used in loops. The `range()` function returns a range object.

```python
print(range(1, 20))
```
@Pyodide.eval

This is not terribly interesting on its own, but is very useful if you want to loop through a sequence of numbers, such as 1 to 20, without having to manually build a list of all of those numbers. If you create a list from the range object and then print that list, you'll see what's going on behind the scenes. What do you think this code will do? Run the code cell below and find out.

```python
print(list(range(1, 20)))
```
@Pyodide.eval

Notice that the last number listed is 19, not 20. Just like in subsetting, the first number passed to `range()` is **inclusive**, and the second is **exclusive**. And don't forget that the first position is index 0!

**Your Turn**: Before running the code cell below, try to predict what number will be returned. Got a number in mind? Run the code and see if your hypothesis is correct! (**Hint**: the bracket notation below (`[ ]`) allows Python to access an element in a collection object, like a list; for a refresher take a look at this [introduction to lists in Python](https://liascript.github.io/course/?https://raw.githubusercontent.com/arcus/education_modules/main/python_basics_lists_dictionaries/python_basics_lists_dictionaries.md#4).)

```python
print(list(range(1, 20))[7])
```
@Pyodide.eval


### Quiz: Loops


```python
for i in range(0, 6):
	j = i*i
```
@Pyodide.eval

The loop in the code cell above is missing a `print()` statement to show us the output. How would you include a `print()` statement such that all of the squares of `i` in `range(0,6)` are displayed? Feel free to edit the code cell above and experiment to find the answer.

[( )] `print(i)`, indented so that it **inside** the loop
[(X)] `print(j)`, indented so that it **inside** the loop
[( )] `print(i)` with no indentation, so that it is **outside** of the loop
[( )] `print(j)` with no indentation, so that it is **outside** of the loop
***
<div class = "answer">

According to our code, we know that `j` is the square of `i`, so that's the number we need to print. If we put the `print(j)` statement **outside** of the loop, we only see the last number, 25, because that is the value that `j` had once the loop was completed. If we place the `print(j)` statement **inside** the loop, each value of `j` is printed before the next iteration of the loop. Therefore, the second option is correct.

</div>
***


## Additional Resources

* There are many helpful resources on [python.org](https://www.python.org/), including a [Beginner's Guide](https://wiki.python.org/moin/BeginnersGuide) and [FAQs](https://docs.python.org/3/faq/).

* The Python documentation also has [more details and examples about loops and conditional statements](https://docs.python.org/3/tutorial/controlflow.html). 

## Recap

@recap