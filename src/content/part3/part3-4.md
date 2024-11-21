---
title: "Strings"
nav_order: 4
hidden: false
---


Let's first revise what we already know about strings and see how to split them. Below we create a string variable magicWord, that contains value "abracadabra".

```cpp
string magicWord = "abracadabra";
```

Passing a string as a parameter to a print command (or, for that matter, any method that takes a string parameter) happens in the familiar way:

```cpp
string magicWord = "abracadabra";
Console.WriteLine(magicWord);
```

```console
abracadabra
```

## Reading and Printing Strings

You can read a string using the ReadLine-method offered by Console. The program below reads the name of the user and prints it:

```cpp
Console.WriteLine("What's your name?");
// reading a line from the user and assigning it to the name variable
string name = Console.ReadLine();

Console.WriteLine(name);
```

```console
What's your name?
> Hank
Hank
```

Strings can also be concatenated. If you place a +-operator between two strings, you get a new string that's a combination of those two strings. Be mindful of any white spaces in your variables!

```cpp
string greeting = "Hi ";
string name = "Lily";
string goodbye = " and see you later!";

string phrase = greeting + name + goodbye;

Console.WriteLine(phrase);
```

```console
Hi Lily and see you later!
```

## String comparisons

Strings can be compared with **==** just like any other variable.

```cpp
string text = "Hello";
string word = "Hullo";

if (text == word)
{
  Console.WriteLine("Hooray!");
}
else
{
  Console.WriteLine("Boo");
}
```

```console
Boo
```

As we've come to know, a boolean value can be inverted through negation - **!**.

```cpp
string text = "Hello";
string word = "Hullo";

if (!(text == word))
{
  Console.WriteLine("Boo!");
}
else
{
  Console.WriteLine("Hooray!");
}
```

Or by negating the comparison

```cpp
string text = "Hello";
string word = "Hullo";

if (text != word)
{
  Console.WriteLine("Boo!");
}
else
{
  Console.WriteLine("Hooray!");
}
```

## Splitting a String

You can split a string to multiple pieces with the **Split-method** of the string class. The method takes as a parameter a string denoting the place around which the string should be split. The split method returns an array of the resulting sub-parts. In the example below, the string has been split around a space.

```cpp
string text = "first second third fourth";
string[] pieces = text.Split(" ");
Console.WriteLine(pieces[0]);
Console.WriteLine(pieces[1]);
Console.WriteLine(pieces[2]);
Console.WriteLine(pieces[3]);

// For empty line print
Console.WriteLine();

for (int i = 0; i < pieces.Length; i++)
{
  Console.WriteLine(pieces[i]);
}
```

```console
first
second
third
fourth

first
second
third
fourth
```

## Data of Fixed Format

Splitting strings is used particularly when the data is of a fixed format. This refers to data that adheres to some predefined format. An example of this of this is the comma-separated values (csv) format, where commas are used to separate values. Below you'll find an example of data in csv form containing names and ages. The first column contains names and the second one ages. The columns are separed by a comma.

```console
sebastian,2 
lucas,2 
lily,1
```

Let's assume the user enters the data above row by row, ending with an empty line.

A program to print the names and ages looks like the following:

```cpp
while (true)
{
  string input = Console.ReadLine();
  if (input == "")
  {
    break;
  }
  string[] pieces = input.Split(",");
  Console.WriteLine("Name: " + pieces[0] + ", age: " + pieces[1]);
}
```

```console
> sebastian,2 
Name: sebastian, age: 2 
> lucas,2 
Name: lucas, age: 2 
> lily,1 
Name: lily, age: 1
```

## Using Diverse Text

We've printed strings in the examples above. Some of the data contained in a fixed-format string can be numerical. In the previous data we used that contained names and ages, the ages were integers.

```console
sebastian,2 
lucas,2 
lily,1
```

Splitting a string always produces an array of strings. If the text is of fixed format, we can assume the data in a specific index to always be of the a specific type -- e.g., in the example above, age at index 1 is an integer.

The program below computes the sum of ages in this fixed format data. In order to compute the sum, the age must first be converted to a number (the familiar command Convert.ToInt32).

```cpp
int sum = 0;

while (true)
{
  string input = Console.ReadLine();
  if (input == "")
  {
    break;
  }
  string[] parts = input.Split(",");
  sum = sum + Convert.ToInt32(parts[1]);
}
Console.WriteLine("Sum of the ages is " + sum);
```

```console
> sebastian,2 
> lucas,2 
> lily,1

Sum of the ages is 5
```

We can write a program to compute the average of the ages in the same way:

```cpp
int sum = 0;
int count = 0;

while (true)
{
  string input = Console.ReadLine();
  if (input == "")
  {
    break;
  }
  string[] parts = input.Split(",");
  sum = sum + Convert.ToInt32(parts[1]);
  count++;
}
if (count > 0)
{
  Console.WriteLine("Age average: " + ((double)sum / count));
}
else
{
  Console.WriteLine("No input");
}
```

```console
> sebastian,2
> lucas,2
> lily,1

Age average: 1.6666666666666667
```

# Exercises

<Exercise title={'022 String three times'}>

Write a program, that reads a string from the user and then prints it three times.

```console
Give a word: cake

cakecakecake
```

<Note>The program should ask for only one string. Don't use a loop here.</Note>

</Exercise>

<Exercise title={'023 True string'}>

Write a program that asks the user for a string. If the user writes the string "true", the program prints "You got it right!", otherwise it prints "Try again!".

```console
Give a string: true 
You got it right!
```


```console
Give a string: trueish 
Try again!
```

</Exercise>

<Exercise title={'024 Login'}>

Write a program that recognizes the following users:


|username	|password|
|--|--|
|alex|	sunshine|
|emma|	haskell|

The program either shows a login message or informs of incorrect username or password.

```console
Enter username: 
>alex 
Enter password: 
> sunshine 
You have successfully logged in!
```
```console
Enter username: 
> emma 
Enter password: 
> haskell 
You have successfully logged in!
```

```console
Enter username: 
> alex 
Enter password: 
> haskell 
Incorrect username or password!
```

<Note>In real life login should not be implemented like this! This is just an exercise.</Note>


</Exercise>

<Exercise title={'025 String split'}>

Write a program that reads strings from the user. If the input is empty, the program stops reading input and halts. For each non-empty input it splits the string input by whitespaces and prints each part of the string on a new line.

```console
> once upon a time 
once 
upon 
a 
time 
> a little program 
a 
little 
program 
> halted 
halted
>
```

</Exercise>

<Exercise title={'026 Split contains av'}>

Write a program, that reads user input until an empty line. For each non-empty string, the program splits the string by spaces and then prints the pieces that contain `av`, each on a new line.

```console
> navy blue shirt
navy
> Do you have a favourite flavour
have
favourite
flavour
> was that a cat
>
```

Tip! Strings have a Contains method, which tells if a string contains another string. It works like this:

```cpp
string text = "volcanologist";

if (text.Contains("can")) 
{
  Console.WriteLine("can was found");
}

if (!text.Contains("tin")) 
{
  Console.WriteLine("tin wasn't found");
}
```

```console
can was found 
tin wasn't found
```

</Exercise>

<Exercise title={'027 First part split'}>

Write a program, that reads user input until an empty line. For each non-empty line the program splits the string by spaces and prints the first part of the string.

```console
> one two three four 
one 
> this is a very important message 
this
>
```

</Exercise>

<Exercise title={'028 Last part split'}>

Write a program, that reads user input until an empty line. For each non-empty line the program splits the string by spaces and prints the last part of the string.

```console
> one two three four 
four 
> this is a very important message 
message
>
```

<Note>
You can find out the length of the array like this:

```cpp
string[] parts = {"one", "two", "three"};
Console.WriteLine("Number of parts: " + parts.Length);
```

```console
Number of parts: 3
```
</Note>

</Exercise>

<Exercise title={'029 CSV age'}>

Write a program that reads names and ages from the user until an empty line is entered. The name and age are separed by a comma.

After reading the program prints the age of the oldest person. You can assume, that the user enters at least one person, and that one of the users is older than the others.

```console
> sebastian,2 
> lucas,2
> lily,1
> hanna,5
> gabriel,10
>
Age of the oldest: 10
```

</Exercise>

<Exercise title={'030 CSV name'}>

Write a program that reads names and ages from the user until an empty line is entered. The name and age are separed by a comma.

After reading the program prints the name of the oldest person. You can assume, that the user enters at least one person, and that one of the users is older than the others.

```console
> sebastian,2 
> lucas,2
> lily,1
> hanna,5
> gabriel,10
>
Name of the oldest: gabriel
```

</Exercise>

<Exercise title={'031 Maximum name and age'}>

Write a program that reads names and birth years from the user until an empty line is entered. The name and birth year are separed by a comma.

After that the program prints the longest name and the highest age. If multiple names are equally longest, you can print any of them. You can assume the user to enter at least one person, and the current year to be the current year.

```console
> sebastian,2017 
> lucas,2017 
> lily,2017 
> hanna,2014 
> gabriel,2009
>
Longest name: sebastian 
Highest age: 11
```

</Exercise>
