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
{: .language-bash}

You can then edit this script in which ever text editor we choose. For the sake of consistency let's use Nano
```
nano my_functions.sh
```
{: .language-bash}

Let's start by using the following script that checks the time 100 times but only prints it if the seconds are a 
multiple of 3:

```
#!/bin/bash

# Start a loop 
while [ $i -le 100 ]
do
   # get the time
   time_var=$(date "+%H:%M:%S")
   # split the time into hours, miniutes, and seconds
   IFS=':' read -ra time_arr <<< "$time_var"
   # use the moduli operator to check if the number is divisible by 3
   if ((${time_arr[2]} % 3 == 0));
   then
      # if the number of seconds is divisible by 3 then print the time to console
      echo $time_var
   fi
   # if wait one second
   sleep 1
   ((i++))
done
```
{: .language-bash}

This script has many lines and a decent amount of complexity so we may want to break it down additionally, the script 
is inflexible. We can use functions to address both these points. And along the way we will learn about the way 
functions return data and how to instruct a function to have different behaviours.

## How can I use the result of my function?

Let's start by considering a function that reads the current time and returns the time array, the first four lines of 
the while loop.
```
function get_time_arr {
   # get the time
   time_var=$(date "+%H:%M:%S")
   # split the time into hours, miniutes, and seconds
   IFS=':' read -ra time_arr <<< "$time_var"
   echo ${time_arr[@]}
}
```
{: .language-bash}

If you have experience with other programing languages then you may be hesitant about the way this function looks. Most 
languages have some kind of return to end a function and return the result. However, in bash the return is only used to 
return the status of the function, a 0 indicates success and anything else indicates failure. To check the status of the
last run function one can use a special variable ```$?```. To get the result of this function we are using 
'command substitution' the final line of the function prints the result of the function which can then be collected. 

Let's take an aside to look at this behaviour. 

Copy and paste the above function to the terminal then run.

```
get_time_arr

echo $?
```
{: .language-bash}

The function prints the time then the function result which should be ```0```. But this isn't particularly useful we 
want to be able to collect and use the output. To do this we collect the output of the function in a variable.

```
time_arr=$( get_time_arr )
echo $?
echo $time_arr
```
{: .language-bash}

Here we have assigned the output to the variable ```time_arr``` then printed it at our convince. We will look at other
ways of returning variables later.

For now lets get back to our script and replace the first few lines with the function we just wrote.

```
#!/bin/bash

# Define a function to get a time and split it into an array
function get_time_arr {
   # get the time
   time_var=$(date "+%H:%M:%S")
   # split the time into hours, miniutes, and seconds
   IFS=':' read -ra time_arr <<< "$time_var"
   # return the array
   echo $time_arr
}

# Start a loop 
while [ $i -le 100 ]
do
   # Run the funtion to get the current time as an array are assign the result to time_arr
   time_arr=$( get_time_arr )
   
   # use the moduli operator to check if the number is divisible by 3
   if ((${time_arr[2]} % 3 == 0));
   then
      # if the number of seconds is divisible by 3 then print the time to console
      echo ${time_arr[@]}
   fi
   # if wait one second
   sleep 1
   ((i++))
done
```
{: .language-bash}

Unfortunately, before we had access to the variables ```time_var``` and ```time_arr``` but now we only have 
```time_arr``` as using 'command substitution' we can only return a single argument now we will upgrade the 
```get_time_array``` to be able to assign multiple values.

```
#!/bin/bash

# Define a function to get a time and split it into an array
function get_time_vars {
   # get the time
   time_var=$(date "+%H:%M:%S")
   # split the time into hours, miniutes, and seconds
   IFS=':' read -ra time_arr <<< "$time_var"
}
```
{: .language-bash}

To use the function above we need to use two variables that will store the results

```
time_var='time_var'
time_arr='time_arr'
get_time_vars
# check the return status of the function
echo $?
# print the variables to the console
echo $time_var
echo ${time_arr[*]}
```
{: .language-bash}

We can use this updated function to restructure our script like so:

```
#!/bin/bash

function get_time_vars {
   # get the time
   time_var=$(date "+%H:%M:%S")
   # split the time into hours, miniutes, and seconds 
   IFS=':' read -ra time_arr <<< "$time_var"
}

# Initilise some variables
i=0
time_var='time_var'
time_arr='time_arr'
# Start a loop 
while [[ $i -le 100 ]]; do
   # Run the funtion to get the current time as an array
   get_time_vars  
   # use the moduli operator to check if the number is divisible by 3 n.b. we are using '10#' to let bash know seconds '01, 02, 03...' are in base 10
   seconds=10#${time_arr[2]}
   floor_val=$(( seconds % 3 ))
   # if the number of seconds is divisible by 3 then print the time to console
   if [ $floor_val -eq 0 ]; then
      echo $time_var
   fi
   # if wait one second
   sleep 1
   (( i++ ))
done
```
{: .language-bash}

We should note here that we don't return the variables from the function we let the function edit global variables this
means the function is less useful than it could be as we have to know the names of the variables beforehand. But it is 
necessary for returning more than one variable (where one of the variables isn't a status code). The above function is 
also dangerous, it can edit variables without the user being aware. This concept is called variable scope and its worth 
investigating in more detail.

Let's consider a simple example, run the three following codes and predict the outcome of each before you do:

Firstly we assign and read out two variables.

> ```
> var_a = 'I am global var_a'
> var_b = 'I am global var_b'
>
> echo $var_a
> echo $var_b
> ```
> {: .language-bash}
>
> ```
> ```
> {: .solution}


> Now we introduce a function like the one above that uses these variable names, what should happen now?

> ```
> function i_break_things {
> var_a = 'I am var_a inside a function'
> var_b = 'I am var_b inside a function'
> echo $var_a
> echo $var_b
> }
>
> var_a = 'I am global var_a'
> var_b = 'I am global var_b'
> 
> echo $var_a
> echo $var_b
>
> i_break_things
>
> echo $var_a
> echo $var_b
> ```
> {: .language-bash}
>
> > ```
> > ```
> > {: .output}
> >
> > The function has updated the variables in this example and the function we created above it seems intentional but 
> > what if your code contained hundreds of lines or the function got included in a path there would be no way to 
> > predict what any given output should be!
> {: .solution}
> 
> This time we add the local keyword to `var_b`. Have a guess how this might change the result...
>
> ```
> function i_break_fewer_things {
> var_a = 'I am var_a inside a function'
> local var_b = 'I am local var_b'
> echo $var_a
> echo $var_b
> }
> 
> var_a = 'I am global var_a'
> var_b = 'I am global var_b'
>
> echo $var_a
> echo $var_b
> 
> i_break_fewer_things
> 
> echo $var_a
> echo $var_b
> ```
> {: .language-bash}
>
> > ```
> > ```
> > {: .output}
> > The inclusion of the local keyword has changed the scope of the variable `var_b` now it is local to the function
> > and the variable outside the function is left unchanged. The words here 'global' and 'local' are the names of the 
> > scopes. The 'global' scope is accessible from anywhere in the script including inside functions 
> {: .solution}
{: .challenge}
## How can I tell my function what do?

## What can I do with a function now?

