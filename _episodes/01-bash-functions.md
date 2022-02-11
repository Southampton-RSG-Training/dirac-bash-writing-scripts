---
title: "Functions in Bash"
slug: dirac-bash-writing-scripts-functions-bash
teaching: 25 
exercises: 10
questions:
- What is a function?
- Why use a function?
- Where do I write a function?
- When should I use a function?
- How do I write a function?
objectives:
- Understand the use case for a function.
- Write a simple function.
- Write a function that accepts inputs.
- Write a function that returns an output.
- Refactor a piece of code to use functions.
keypoints:
- Functions reduce code reuse.
- Functions make testing the behaviour of code easier by isolating tasks.
- Functions make abstracting tasks easier.
---

## What is a function and why use one?
This is a function:

```
function hello_world {
  echo Hello World from teh function 'hello_world'!
}
```
{: .languaage-bash}

or with slightly different syntax,
```
hello_world () {
  echo Hello World from teh function 'hello_world'!
}
```
{: .languaage-bash}

Both of these blocks define a function that when used prints ```Hello World from teh function 'hello_world'!``` the following to the terminal.

Try it out now by copying one of the functions into the command line hit return to save it. Then run it by typing ```hello_world``` and pressing return.

```
Hello World from teh function 'hello_world'!
```
{: .output}

You may note that this is quite similar to some other commands we have been using in the terminal like ```pwd``` or 
```ls```. These are both functions that have been predefined for us because they are useful tools that we are likely to
want to use multiple times.

We use functions to provide utilities that we want to use quickly and repeatedly, the above ```hello_world``` functions
are actually a bad example in this regard. Whilst we can now print the string 
```Hello World from the function 'hello_world'!``` somewhat more quickly is unlikely that this is something we will 
frequently want to do. On the other hand ```pwd``` is an extremely useful tool that we will use frequently and is a 
complicated script to write.

## Where do I write a function and when should I use them?
In the above example we wrote and used a function directly in the command line. Whilst there is nothing wrong with this
it doesn't give us any options to edit the function without completely rewriting it. For example, in the above function
there was a typo and we put ```teh``` and not ```the``` to fix this we need to write the whole function again and update
it in the command line.

```
function hello_world {
  echo Hello World from the function 'hello_world'!
}
```
{: .languaage-bash}

```
Hello World from the function 'hello_world'!
```
{: .output}

It's therefore much more useful to define and use your functions inside a script where they can be edited and 
used quickly.


Using the following command we will make a script to work on some functions. Then use chmod so we can run it.
```
touch my_functions.sh
chmod +x my_functions.sh
```
{: .languaage-bash}

You can then edit this script in which ever text editor we choose. For the sake of consistency I will use Nano
```
nano my_functions.sh
```
{: .languaage-bash}

Let's start by using the following script that checks the time 100 times but only prints it if the seconds are a 
multiple of 3:

```
#!/bin/bash
for 
do
   time_var=$(date "+%H:%M:%S")
   readarray -d : -t time_arr <<< "$time_var"
   if time_arr
done
```
{: .languaage-bash}


## How can I tell my function what do?

## How can I use the result of my function?

## What can I do with a function now?

