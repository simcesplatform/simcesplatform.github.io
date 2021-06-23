# Pseudocode reference

This page explains how to interpret the pseudocode that appears in some pages.

## Motivation

The principles behind the pseudocode are:

- Be easily readable
- Express what is essential
- Leave freedom of expression, i.e., avoid strict rules

What are the advantages? Pseudocode:

- is more compact than human language
- does not tie the algorithm to any programming language
- contains fewer details than source code and therefore:
    - pseudocode is faster to write
    - it's easier to see the overall picture from pseudocode


## Comments

```nohighlight
// This is a comment
```


## Variables and strings

```nohighlight
// Declaring a string variable:
my_var = "Example string"
 
// Concatenation happens as follows.
// This will result in "Example string is nice"
my_var_longer = my_var + " is nice"
```


## Calculation (calc)

```nohighlight
// "calc result" indicates that the component generates a result to be published.
// Often, this includes calculation (such as simulation)
// but the component may as well supply a constant value.
calc result
```


## Messaging (publ, recv)

```nohighlight
// The component receives a message with some input to calculation.
// This does not specify where the input comes from.
recv input
 
// Use the input to calculate a result.
calc result
 
// Publish the result.
// This does not specify the topic to which the result is published.
publ result
```

Often, pseudocode does not specify to which topic to publish. This is to avoid redundancy with another page or location that specifies the exact topic. However, if there is a need to specify the topic explicitly, you can say:

```nohighlight
topic = "TopicX"
publ result to topic
 
// Alternatively:
publ result to "TopicX"
```


## If conditions (if, else)

```nohighlight
// Indentation indicates which lines belong to which code block (if and else).
if x
  // This line is executed if condition "x" matches
else if y
  // This line is executed if condition "y" matches
else
  // This line is executed if none of the earlier conditions matches
 
// This line is executed after the if-else block
```


## Looping (loop, break)

```nohighlight
loop
  // This line is executed as long as "break loop" has not occurred.
  // Indentation indicates which lines are within the loop.
 
  // The loop breaks conditionally.
  // If there are nested loops, break only affects the innermost loop.
  if x
    break loop
```


## Wait for events

```nohighlight
// Wait for one of events to occur first
wait for one of
  recv x
    // This line is executed if "recv x" occurs first
  recv y
    // This line is executed if "recv y" occurs first
  recv z
 
// Because there is no indented line after the event "recv z",
// this causes the execution to jump here.
// Still, even the other events will eventually lead here.
```

## Quitting (quit)

```nohighlight
// To quit execution, say:
quit
```


## Functions

Functions are defined as follows:

```nohighlight
function my_function()
   
  // This is some code within the function.
  // The indentation indicates which lines belong to the function.
```

The function above is called as follows:

```nohighlight
my_function()
```
