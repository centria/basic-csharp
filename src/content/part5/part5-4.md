---
title: "Objects and references"
nav_order: 4
hidden: false
---


Let's continue working with objects and references. Assume we can use the class that represents a person, shown below. 

Person has object variables name, age, weight, and height; additionally, it offers methods to calculate the body mass index, among other things. 

```cpp
namespace Exercise001
{

  public class Person
  {
    private string name;
    private int age;
    private int weight;
    private int height;

    // Constructor which only sets the name
    public Person(string name) : this(name, 0, 0, 0)
    {
    }

    // Constructor to set name and age
    public Person(string name, int age) :this(name, age, 0, 0)
    {
    }

    // Constructor to set all the variables.
    // The two constructors above call this when they are used.
    public Person(string name, int age, int weight, int height)
    {
      this.name = name;
      this.age = age;
      this.weight = weight;
      this.height = height;
    }

    public double BodyMassIndex()
    {
      double heigthPerHundred = this.height / 100.0;
      return this.weight / (heigthPerHundred * heigthPerHundred);
    }

    public double MaximumHeartRate()
    {
      return 206.3 - (0.711 * this.age);
    }
    
    public bool IsAdult()
    {
      if (this.age < 18)
      {
        return false;
      }
      return true;
    }

    public void GrowOlder()
    {
      this.GrowOlder(1);
    }

    public void GrowOlder(int years)
    {
      this.age += years;
    }

    public override string ToString()
    {
      return this.name + ", age " + this.age + " years";
    }


  }
}
```

Precisely what happens when a new object is created?

```cpp
Person joan = new Person("Joan Ball");
```

Calling a constructor with the command new causes several things to happen. 
* First, space is reserved in the computer memory for storing object variables. 
* Then default or initial values are set to object variables (e.g. an int type variable receives an initial value of 0). 
* Lastly, the source code in the constructor is executed.

A constructor call returns a reference to an object. A **reference** is information about the location of object data.

So the value of the variable is set to be a reference, i.e. knowledge about the location of related object data. The image above also reveals that strings -- the name of our example person, for instance -- are objects, too.

## Assigning a reference type variable copies the reference

Let's add a Person type variable called **ball** into the program, and assign joan as its initial value. What happens then?

```cpp
Person joan = new Person("Joan Ball");
Console.WriteLine(joan);

Person ball = joan;
```

The statement **Person ball = joan;** creates a new Person variable call, and copies the value of the variable joan as its value. As a result, ball refers to the same object as joan.

Let's inspect the same example a little more thoroughly.

```cpp
Person joan = new Person("Joan Ball");
Console.WriteLine(joan);

Person ball = joan;
ball.GrowOlder();
ball.GrowOlder();

Console.WriteLine(joan);
```

```console
Joan Ball, age: 0
Joan Ball, age: 2
```

**Joan Ball** -- i.e. the Person object that the reference in the **joan** variable points at -- starts as 0 years old. After this the value of the joan variable is assigned (so copied) to the **ball** variable. The **Person object ball** is aged by two years, and Joan Ball ages as a consequence!

An object's internal state is not copied when a variable's value is assigned. A new object is not being created in the statement **Person ball = joan;** -- the value of the variable ball is assigned to be the copy of joan's value, i.e. a reference to an object.

Next, the example is continued so that a new object is created for the **joan** variable, and a reference to it is assigned as the value of the variable. The variable **ball** still refers to the object that we created earlier.

```cpp
Person joan = new Person("Joan Ball");
Console.WriteLine(joan);

Person ball = joan;
ball.GrowOlder();
ball.GrowOlder();

Console.WriteLine(joan);

joan = new Person("Joan B.");
Console.WriteLine(joan);
```
The following is printed:


```console
Joan Ball, age: 0
Joan Ball, age: 2
Joan B., age: 0
```

So in the beginning the variable **joan** contains a reference to one object, but in the end a reference to another object has been copied as its value.

## null value of a reference variable

Let's extend the example further by setting the value of the reference variable **ball** to **null**, i.e. a reference "to nothing". The **null** reference can be set as the value of any reference type variable.

```cpp
Person joan = new Person("Joan Ball");
Console.WriteLine(joan);

Person ball = joan;
ball.GrowOlder();
ball.GrowOlder();

Console.WriteLine(joan);

joan = new Person("Joan B.");
Console.WriteLine(joan);

ball = null;
```

```console
Joan Ball, age: 0
Joan Ball, age: 2
Joan B., age: 0
```

The object whose name is Joan Ball is referred to by nobody. In other words, the object has become "garbage". garbage collector manages the allocation and release of memory for your application. 

From [**C# Documentation on garbage collection**](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/):

"Each time you create a new object, the common language runtime allocates memory for the object from the managed heap. As long as address space is available in the managed heap, the runtime continues to allocate space for new objects. 

However, memory is not infinite. Eventually the garbage collector must perform a collection in order to free some memory.  If the garbage collection did not happen, the garbage objects would reserve a memory location until the end of the program execution."

In other words, when an object is not needed anymore, it will be taken care of, so the memory space will be accesible for other use.

Let's see what happens when we try to print a variable that references "nothing" i.e. null.

```cpp
Person joan = new Person("Joan Ball");
Console.WriteLine(joan);

Person ball = joan;
ball.GrowOlder();
ball.GrowOlder();

Console.WriteLine(joan);

joan = new Person("Joan B.");
Console.WriteLine(joan);

ball = null;
Console.WriteLine(ball);
```

```console
Joan Ball, age: 0
Joan Ball, age: 2
Joan B., age: 0

```

We cannot see anything in the last line of print: That's because **ball** now refers to **null**, or "nothing".

Let's see what happens if we grow our **ball** older.

```cpp
Person joan = new Person("Joan Ball");
Console.WriteLine(joan);

Person ball = joan;
ball.GrowOlder();
ball.GrowOlder();

Console.WriteLine(joan);

joan = new Person("Joan B.");
Console.WriteLine(joan);

ball = null;
Console.WriteLine(ball);
ball.GrowOlder();
```

```console
Joan Ball, age: 0
Joan Ball, age: 2
Joan B., age: 0

Unhandled exception. System.NullReferenceException: Object reference not set to an instance of an object.
    in /.../src/Exercise001/Program.cs:line 24
```

Bad things happen. We get a **NullReferenceException**. The name for the exception is quite self-explanatory (as they aim to be). In the course of the program,there occured an error indicating that we called a method on a variable that refers to nothing.

We promise that this is not the last time you will encounter the previous error. When you do, the first step is to look for variables whose value could be **null**. Fortunately, the error message is useful: it tells which row caused the error. Try it out yourself!

## Object as a method parameter

We have seen both value and reference variables act as method parameters. Since objects are reference variables, any type of object can be defined to be a method parameter. Let's take a look at a practical demonstration.

Amusement park rides only permit people who are taller than a certain height. The limit is not the same for all attractions. Let's create a class representing an amusement park ride. When creating a new object, the constructor receives as parameters the name of the ride, and the smallest height that permits entry to the ride.

```cpp
public class AmusementParkRide
{
  private string name;
  private int lowestHeight;

  public AmusementParkRide(string name, int lowestHeight)
  {
    this.name = name;
    this.lowestHeight = lowestHeight;
  }

  public override string ToString()
  {
    return this.name + ", minimum height: " + this.lowestHeight;
  }
}
```

Then let's write a method that can be used to check if a person is allowed to enter the ride, so if they are tall enough. The method returns true if the person given as the parameter is permitted access, and false otherwise.

We can safely assume our Person class now has the ability to tell the age outside the class (i.e. the variable **int age** is public).

```cpp
public bool AllowedToRide(Person person)
{
  if (person.height < this.lowestHeight)
  {
    return false;
  }

  return true;
}
```

So the method AllowedToRide of an AmusementParkRide object is given a Person object as a parameter. Like earlier, the value of the variable -- in this case, a reference -- is copied for the method to use. The method handles a copied reference, and it calls the name property of the person passed as a parameter.

Below is an example main program where the amusement park ride method is called twice: first the supplied parameter is a person object **matt**, and then a person object **jasper**:

```cpp
static void Main(string[] args)
{

  // Our constructor has name, age, weight, height
  Person matt = new Person("Matt", 15, 86, 180);

  Person jasper = new Person("Jasper", 8, 34, 132);

  AmusementParkRide waterTrack = new AmusementParkRide("Water track", 140);

  if (waterTrack.AllowedToRide(matt))
  {
    Console.WriteLine(matt.name + " may enter the ride");
  }
  else
  {
    Console.WriteLine(matt.name + " may not enter the ride");
  }

  if (waterTrack.AllowedToRide(jasper))
  {
    Console.WriteLine(jasper.name + " may enter the ride");
  }
  else
  {
    Console.WriteLine(jasper.name + " may not enter the ride");
  }

  Console.WriteLine(waterTrack);
}
```

```console
Matt may enter the ride
Jasper may not enter the ride
Water track, minimum height: 140
```

What if we wanted to know how many people have taken the ride?

Let's add an object variable to the amusement park ride. It keeps track of the number of people that were permitted to enter.

```cpp
public class AmusementParkRide
{
  private string name;
  private int lowestHeight;
  private int visitors;

  public AmusementParkRide(string name, int lowestHeight)
  {
    this.name = name;
    this.lowestHeight = lowestHeight;
    this.visitors = 0;
  }

  public bool AllowedToRide(Person person)
  {
    if (person.height < this.lowestHeight)
    {
      return false;
    }
    this.visitors++;
    return true;
  }

  public override string ToString()
  {
    return this.name + ", minimum height: " + this.lowestHeight +
            ", visitors: " + this.visitors;
  }
}
```

Now the previously used example program also keeps track of the number of visitors who have experienced the ride.

```cpp
static void Main(string[] args)
{

  // Our constructor has name, age, weight, height
  Person matt = new Person("Matt", 15, 86, 180);

  Person jasper = new Person("Jasper", 8, 34, 132);

  AmusementParkRide waterTrack = new AmusementParkRide("Water track", 140);

  if (waterTrack.AllowedToRide(matt))
  {
    Console.WriteLine(matt.name + " may enter the ride");
  }
  else
  {
    Console.WriteLine(matt.name + " may not enter the ride");
  }

  if (waterTrack.AllowedToRide(jasper))
  {
    Console.WriteLine(jasper.name + " may enter the ride");
  }
  else
  {
    Console.WriteLine(jasper.name + " may not enter the ride");
  }

  Console.WriteLine(waterTrack);
}
```

```console
Matt may enter the ride
Jasper may not enter the ride
Water track, minimum height: 140, visitors: 1
```

## Object as object variable

Objects may contain references to objects.

Let's keep working with people, and add a birthday to the person class. A natural way of representing a birthday is to use a **Date** class. We could use the classname Date, but for the sake of **avoiding confusion with the similarly named existing C# class**, we will use **SimpleDate** here.

```cpp
public class SimpleDate
{
  private int day;
  private int month;
  private int year;

  public SimpleDate(int day, int month, int year)
  {
    this.day = day;
    this.month = month;
    this.year = year;
  }


  public override string ToString()
  {
    return this.day + "." + this.month + "." + this.year;
  }
}
```

Since we know the birthday, there is no need to store that age of a person as a separate object variable. The age of the person can be inferred from their birthday. Let's assume that the class Person now has the following variables.

```cpp
public class Person
{
  public string name;
  private SimpleDate birthday;
  private int weight;
  public int height;

//  ...
```

Let's create a new Person constructor that allows for setting the birthday:

```cpp
public Person(string name, SimpleDate date)
{
  this.name = name;
  this.birthday = date;
  this.weight = 0;
  this.height = 0;
}
```

Along with the constructor above, we could give Person another constructor where the birthday was given as integers.

```cpp
public Person(string name, int day, int month, int year)
{
  this.name = name;
  this.birthday = new SimpleDate(day, month, year);
}
```

The constructor receives as parameters the different parts of the date (day, month, year). They are used to create a date object, and finally the reference to that date is copied as the value of the object variable birthday.

Let's modify the ToString method of the Person class so that instead of age, the method returns the birthday:

```cpp
public override string ToString()
{
  return this.name + ", born on " + this.birthday;
}
```

Let's see how the updated Person class works.

```cpp
SimpleDate date = new SimpleDate(1, 1, 780);
Person muhammad = new Person("Muhammad ibn Musa al-Khwarizmi", date);
Person pascal = new Person("Blaise Pascal", 19, 6, 1623);

Console.WriteLine(muhammad);
Console.WriteLine(pascal);
```

```console
Muhammad ibn Musa al-Khwarizmi, born on 1.1.780
Blaise Pascal, born on 19.6.1623
```

Now a person object has object variables name and birthday. The variable name is a string, which itself is an object; the variable birthday is a **SimpleDate object**.

Both variables contain a reference to an object. Therefore a person object contains two references. 

So the Main program has is connected to two Person objects by strands. A person has a name and a birthday. Since both variables are objects, these attributes exist at the other ends of the strands.

Birthday appears to be a good extension to the Person class. Earlier we noted that the object variable age can be calculated with birthday, so it was removed.

In the section above, we use our own class SimpleDate to represent date, because it is suitable for illustrating and practising the operation of objects. If we want to handle date (and time) in our own programs, we would most probably use [**C#'s DateTime**](https://docs.microsoft.com/en-us/dotnet/api/system.datetime?view=netframework-4.8) rather than code our own version.

```cpp
DateTime now = DateTime.Now;
Console.WriteLine(now);
int year = now.Year;
int month = now.Month;
int day = now.Day;

Console.WriteLine("today is  " + day + "." + month + "." + year);
```

```console
2/13/2020 7:12:07 PM
today is  13.2.2020
```

<Note>With C# the output of the first line can differ, depending on the language setting of your environment. This is due to culture in C# environments. We will discuss this later.</Note>

## Object of same type as method parameter

We will continue working with the **Person** class. We recall that persons know their birthdays:

```cpp
public class Person
{
  public string name;
  private SimpleDate birthday;
  private int weight;
  public int height;
  
// ...
```

We would like to compare the ages of two people. The comparison can be done in multiple ways. We could, for instance, implement a method called **public int AgeAsYears()** for the Person class; in that case, the comparison would happen in the following manner:

```cpp
Person muhammad = new Person("Muhammad ibn Musa al-Khwarizmi", 1, 1, 780);
Person pascal = new Person("Blaise Pascal", 19, 6, 1623);

if (muhammad.AgeAsYears() > pascal.AgeAsYears()) {
    Console.WriteLine(muhammad.name + " is older than " + pascal.name);
}
```

We are now going to learn a more "object-oriented" way to compare the ages of people.

We are going to create a new method **public bool OlderThan(Person compared)** for the Person class. It can be used to compare a certain person object to the person supplied as the parameter based on their ages.

The method is meant to be used like this:

```cpp
Person muhammad = new Person("Muhammad ibn Musa al-Khwarizmi", 1, 1, 780);
Person pascal = new Person("Blaise Pascal", 19, 6, 1623);

if (muhammad.OlderThan(pascal)) {  //  same as muhammad.OlderThan(pascal)==true
    Console.WriteLine(muhammad.name + " is older than " + pascal.name);
} else {
    Console.WriteLine(muhammad.name + " is not older than " + pascal.name);
}
```

The program above tells if al-Khwarizmi older than Pascal. The method **OlderThan** returns true if the object that is used to call the method **(object OlderThan(objectGivenAsParameter))** is older than the object given as the parameter, and false otherwise.

In practice, we call the **OlderThan** method of the object that matches "Muhammad ibn Musa al-Khwarizmi", which is referred to by the variable **muhammad**. The reference **pascal**, matching the object "Blaise Pascal", is given as the parameter to that method.

The program prints:

```console
Muhammad ibn Musa al-Khwarizmi is older than Blaise Pascal
```

The method OlderThan receives a **person object** as its parameter. More precisely, the variable that is defined as the method parameter receives a copy of the value contained by the given variable. That value is a reference to an object, in this case.

The implementation of the method is illustrated below. Note that the method may return a value in more than one place -- here the comparison has been divided into multiple parts based on the years, the months, and the days:

```cpp
public bool OlderThan(Person compared)
{
  // 1. First compare years
  int ownYear = this.birthday.year;
  int comparedYear = compared.birthday.year;

  if (ownYear < comparedYear)
  {
    return true;
  }

  if (ownYear > comparedYear)
  {
    return false;
  }

  // 2. Same birthyear, compare months
  int ownMonth = this.birthday.month;
  int comparedMonth = compared.birthday.month;

  if (ownMonth < comparedMonth)
  {
    return true;
  }

  if (ownMonth > comparedMonth)
  {
    return false;
  }

  // 3. Same birth year and month, compare days
  int ownDay = this.birthday.day;
  int comparedDay = compared.birthday.day;

  if (ownDay < comparedDay)
  {
    return true;
  }

  return false;
}
```

Let's pause for a moment to consider abstraction, one of the principles of object-oriented programming. The idea behind abstraction is to conceptualize the programming code so that each concept has its own clear responsibilities. When viewing the solution above, however, we notice that the comparison functionality would be better placed inside the SimpleDate class instead of the **Person** class.

We'll create a method called **public boole Before(SimpleDate compared)** for the class SimpleDate. The method returns the value **true** if the date given as the parameter is after (or on the same day as) the date of the object whose method is called.

```cpp
public class SimpleDate
{
  private int day;
  private int month;
  private int year;

  public SimpleDate(int day, int month, int year)
  {
    this.day = day;
    this.month = month;
    this.year = year;
  }


  public override string ToString()
  {
    return this.day + "." + this.month + "." + this.year;
  }

  // used to check if this date object (`this`) is before
  // the date object given as the parameter (`compared`)
  public bool Before(SimpleDate compared)
  {
    // first compare years
    if (this.year < compared.year)
    {
      return true;
    }

    if (this.year > compared.year)
    {
      return false;
    }

    // years are same, compare months
    if (this.month < compared.month)
    {
      return true;
    }

    if (this.month > compared.month)
    {
      return false;
    }

    // years and months are same, compare days
    if (this.day < compared.day)
    {
      return true;
    }

    return false;
  }
}
```

Even though the object variables year, month, and day are encapsulated (private) object variables, we can read their values by writing compared.**variableName** This is because private variable can be accessed from all the methods contained by that class. Notice that the syntax here matches calling some object method. Unlike when calling a method, we refer to a property or a field of an object, so the parentheses that indicate a method call are not written.

An example of how to use the method:

```cpp
SimpleDate d1 = new SimpleDate(14, 2, 2011);
SimpleDate d2 = new SimpleDate(21, 2, 2011);
SimpleDate d3 = new SimpleDate(1, 3, 2011);
SimpleDate d4 = new SimpleDate(31, 12, 2010);

Console.WriteLine(d1 + " is earlier than " + d2 + ": " + d1.Before(d2));
Console.WriteLine(d2 + " is earlier than " + d1 + ": " + d2.Before(d1));

Console.WriteLine(d2 + " is earlier than " + d3 + ": " + d2.Before(d3));
Console.WriteLine(d3 + " is earlier than " + d2 + ": " + d3.Before(d2));

Console.WriteLine(d4 + " is earlier than " + d1 + ": " + d4.Before(d1));
Console.WriteLine(d1 + " is earlier than " + d4 + ": " + d1.Before(d4));
```

```console
14.2.2011 is earlier than 21.2.2011: True
21.2.2011 is earlier than 14.2.2011: False
21.2.2011 is earlier than 1.3.2011: True
1.3.2011 is earlier than 21.2.2011: False
31.12.2010 is earlier than 14.2.2011: True
14.2.2011 is earlier than 31.12.2010: False
```

Let's tweak the method OlderThan of the Person class so that from here on out, we take use of the comparison functionality that date objects provide.

```cpp
public bool OlderThan(Person compared) {
    if (this.birthday.Before(compared.birthday)) {
        return true;
    }

    return false;

    // or return more directly:
    // return this.birthday.Before(compared.birthday);
}
```

Now the concrete comparison of dates is implemented in the class that it logically (based on the class names) belongs to.

## Equality

If we want to be able to compare two objects of our own design with the **Equals** method, that method must be defined in the class. The method equals is defined as a method that returns a boolean type value -- the return value indicates whether the objects are equal.

The method equals is implemented in a way that allows for using it to compare the current object with any other object. The method receives an Object type object as its single parameter -- all objects are Object type, in addition to their own type. The equals method first compares if the addresses are equal: if so, the objects are equal. After this, we examine if the types of the objects are the same: if not, the objects are not equal. Then the Object object passed as the parameter is converted to the type of the object that is being examined by using a type cast. Then the values of the object variables can be compared. Below the equality comparison has been implemented for the SimpleDate class.

```cpp
namespace Exercise001
{
  public class SimpleDate
  {
    private int day;
    private int month;
    private int year;

    public SimpleDate(int day, int month, int year)
    {
      this.day = day;
      this.month = month;
      this.year = year;
    }

    public override bool Equals(object compared)
    {
      // if the variables are located in the same position, they are equal
      if (this == compared)
      {
        return true;
      }

      // if the compared object is null, or
      // if the type of the compared object is not SimpleDate, the objects are not equal
      if ((compared == null) || !this.GetType().Equals(compared.GetType()))
      {
        return false;
      }

      // convert the Object type compared object
      // into an SimpleDate type object called comparedSimpleDate
      SimpleDate comparedSimpleDate = (SimpleDate)compared;

      // if the values of the object variables are the same, the objects are equal
      if (this.day == comparedSimpleDate.day &&
          this.month == comparedSimpleDate.month &&
          this.year == comparedSimpleDate.year)
      {
        return true;
      }

      // otherwise the objects are not equal
      return false;
    }


    public override string ToString()
    {
      return this.day + "." + this.month + "." + this.year;
    }
  }
}
```

<Note>So far we have compared only with ==, but now we're also using Equals. They compare different things. Try out, what happens if you compare two identical SimpleDates with both:

```cpp
static void Main(string[] args)
{
  SimpleDate date1 = new SimpleDate(1,2,2020);
  SimpleDate date2 = new SimpleDate(1,2,2020);
  Console.WriteLine(date1.Equals(date2));
  Console.WriteLine(date1 == date2);
}
```
</Note>


## Some inheritance

Every class we create (and every ready-made C# class) inherits the class Object, even though it is not specially visible in the program code. This is why an instance of any class can be passed as a parameter to a method that receives an Object type variable as its parameter. Inheriting the Object can be seen elsewhere, too: for instance, the **ToString** method exists even if you have not implemented it yourself, just as the equals method does.

To illustrate, the following source code compiles successfully: **Equals** method can be found in the Object class inherited by all classes.

```cpp
namespace Exercise001
{
  public class Bird
  {
    private string name;

    public Bird(string name)
    {
      this.name = name;
    }
  }
}
```

```cpp
static void Main(string[] args)
{
  Bird red = new Bird("Red");
  // We're using GetHashCode to show they're unique
  // The method returns the exact runtime type of the current instance.
  Console.WriteLine(red.GetHashCode());

  Bird chuck = new Bird("Chuck");
  Console.WriteLine(chuck.GetHashCode());

  if (red.Equals(chuck))
  {
    Console.WriteLine(red + " equals " + chuck);
  }
  else
  {
    Console.WriteLine("They're not equal!");
  }
}
```

```console
58225482
54267293
They're not equal!
```

## Object equality and lists

Let's examine how the **Equals** method is used with Lists. Let's assume we have the previously described class **Bird** with only the default **Equals** method (not defined by us).

```cpp
public class Bird
{
  private string name;

  public Bird(string name)
  {
    this.name = name;
  }
}
```

Let's create a list and add a bird to it. After this we'll check if that bird is contained in it.

```cpp
static void Main(string[] args)
{
  List<Bird> birds = new List<Bird>();
  Bird red = new Bird("Red");

  if (birds.Contains(red))
  {
    Console.WriteLine("Red is on the list.");
  }
  else
  {
    Console.WriteLine("Red is not on the list.");
  }

  birds.Add(red);
  if (birds.Contains(red))
  {
    Console.WriteLine("Red is on the list.");
  }
  else
  {
    Console.WriteLine("Red is not on the list.");
  }


  Console.WriteLine("However!");

  red = new Bird("Red");
  if (birds.Contains(red))
  {
    Console.WriteLine("Red is on the list.");
  }
  else
  {
    Console.WriteLine("Red is not on the list.");
  }
}
```

```console
Red is not on the list.
Red is on the list.
However!
Red is not on the list.
```

We can notice in the example above that we can search a list for our own objects. First, when the bird had not been added to the list, it is not found -- and after adding it is found. When the program switches the **red** object into a new object, with exactly the same contents as before, it is no longer equal to the object on the list, and therefore cannot be found on the list.

The **Contains** method of a list uses the **Equals** method that is defined for the objects in its search for objects. In the example above, the **Bird** class has no definition for that method, so a bird with exactly the same contents -- but a different reference -- cannot be found on the list.

Let's implement the **Equals** method for the class **Bird**. The method examines if the names of the objects are equal -- if the names match, the birds are thought to be equal.

```cpp
public override bool Equals(object compared)
{
  // if the variables are located in the same position, they are equal
  if (this == compared)
  {
    return true;
  }

  // if the compared object is null or not of type Bird, the objects are not equal
  if ((compared == null) || !this.GetType().Equals(compared.GetType()))
  {
    return false;
  }
  else
  {
    // convert the object to a Bird object
    Bird comparedBird = (Bird)compared;
    // if the values of the object variables are equal, the objects are, too
    return this.name.Equals(comparedBird.name);
  }
}
```

Now the Contains list method recognizes birds with identical contents.

```cpp
static void Main(string[] args)
{
  List<Bird> birds = new List<Bird>();
  Bird red = new Bird("Red");

  if (birds.Contains(red))
  {
    Console.WriteLine("Red is on the list.");
  }
  else
  {
    Console.WriteLine("Red is not on the list.");
  }

  birds.Add(red);
  if (birds.Contains(red))
  {
    Console.WriteLine("Red is on the list.");
  }
  else
  {
    Console.WriteLine("Red is not on the list.");
  }


  Console.WriteLine("However!");

  red = new Bird("Red");
  if (birds.Contains(red))
  {
    Console.WriteLine("Red is on the list.");
  }
  else
  {
    Console.WriteLine("Red is not on the list.");
  }
}
```

```console
Red is not on the list.
Red is on the list.
However!
Red is on the list.
```

## Object as a method's return value

We have seen methods return boolean values, numbers, and strings. Easy to guess, a method can return an object of any type.

In the next example we present a simple counter that has the method **Clone**. The method can be used to crete a clone of the counter; i.e. a new counter object that has the same value at the time of its creation as the counter that is being cloned.

```cpp
namespace Exercise001
{
  public class Counter
  {
    private int value;

    // example of using multiple constructors:
    // you can call another constructor from a constructor by calling this
    // notice that the this call must be on the first line of the constructor
    public Counter() : this(0)
    {
    }

    public Counter(int initialValue)
    {
      this.value = initialValue;
    }

    public void Increase()
    {
      this.value = this.value + 1;
    }

    public override string ToString()
    {
      return "value: " + value;
    }

    public Counter Clone()
    {
      // create a new counter object that receives the value of the cloned counter as its initial value
      Counter clone = new Counter(this.value);

      // return the clone to the caller
      return clone;
    }
  }
}
```

An example of using counters follows:

```cpp
Counter counter = new Counter();
counter.Increase();
counter.Increase();

Console.WriteLine(counter);         // prints 2

Counter clone = counter.Clone();

Console.WriteLine(counter);         // prints 2
Console.WriteLine(clone);          // prints 2

counter.Increase();
counter.Increase();
counter.Increase();
counter.Increase();

Console.WriteLine(counter);         // prints 6
Console.WriteLine(clone);          // prints 2

clone.Increase();

Console.WriteLine(counter);         // prints 6
Console.WriteLine(clone);          // prints 3
```

```console
value: 2
value: 2
value: 2
value: 6
value: 2
value: 6
value: 3
```

Immediately after the cloning operation, the values contained by the clone and the cloned object are the same. However, they are two different objects, so increasing the value of one counter does not affect the value of the other in any way.

Similarly, a **Factory** object could also be used to create and return new Car objects. Below is a sketch of the outline of the factory -- the factory also knows the makes of the cars that are created.

```cpp
public class Factory 
{
  private string make;

  public Factory(string make) 
  {
    this.make = make;
  }

  public Car ProcuceCar() 
  {
    return new Car(this.make);
  }
}
```

# Exercises

<Exercise title={'006 NullReferenceException'}>

Implement a program that causes the `NullReferenceException` error. The error should occur directly after starting the program -- don't wait to read input from the user, for instance.

<Note>Change an object into null, and try to use it.</Note>

</Exercise>

<Exercise title={'007 Health station'}>

In the exercise base there is the class `Person`, which we are already quite familiar with. There is also an outline for the class `HealthStation`. Health station objects process people in different ways, they e.g. weigh and feed people. In this exercise we will construct a health station. The code of the Person class should not be modified in this exercise!

* Section 1 - Weighing people

The `Weigh` method receives a person as a parameter, and it is meant to return to its caller the weight of that person. The weight information can be found by calling a suitable property of the Person person. So your task is to complete the code of the method!

* Section 2 - Feeding people

It is possible to modify the state of the object that is received as a parameter. Fill in the method called `public void Feed(Person person)` for the health station. It should increase the weight of the parameter person by one.

* Section 3 - Counting weighings

Use the variable `public int weighings { get; private set; }` to count weighings - That is, when ever the method `Weigh` is called, the variable should increase by one.

Here's a Main class to test all of the sections:

```cpp
public static void Main(string[] args)
{
  // create new Station
  HealthStation childrensHospital = new HealthStation();

  // create two new persons
  Person ethan = new Person("Ethan", 1, 110, 7);
  Person peter = new Person("Peter", 33, 176, 85);

  // Try out the Persons and method Weigh
  Console.WriteLine(ethan.name + " weight: " + childrensHospital.Weigh(ethan) + " kilos");
  Console.WriteLine(peter.name + " weight: " + childrensHospital.Weigh(peter) + " kilos");

  // Test feeding the persons
  childrensHospital.Feed(ethan);
  childrensHospital.Feed(peter);

  // See that the weights have changed
  Console.WriteLine(ethan.name + " weight: " + childrensHospital.Weigh(ethan) + " kilos");
  Console.WriteLine(peter.name + " weight: " + childrensHospital.Weigh(peter) + " kilos");

  // Keep weighing to increase the 'int weighings'
  childrensHospital.Weigh(ethan);
  childrensHospital.Weigh(ethan);
  childrensHospital.Weigh(ethan);
  childrensHospital.Weigh(ethan);

  // See that the variable has increased to 8
  Console.WriteLine("weighings performed: " + childrensHospital.weighings);
}
```

Should print out 

```console
Ethan weight: 110 kilos
Peter weight: 176 kilos
Ethan weight: 111 kilos
Peter weight: 177 kilos
weighings performed: 8
```
</Exercise>

<Exercise title={'008 Card payments'}>

In a previous exercises part we created a class called `PaymentCard`. The card had methods for Eating a lunch and drinking coffee, and also for adding money to the card.

However, there was a problem with the PaymentCard class that is implemented in this fashion. The card knew the prices of the different payments, and therefore was able to decrease the balance by the proper amount. What about if the prices are raised? Or new items are added to the list of offered products? A change in the pricing would mean that all the existing cards would have to be replaced with new cards that are aware of the new prices.

An improved solution is to make the cards "dumb"; unaware of the prices and products that are sold, and only keeping track of their balance. All the intelligence is better placed in separate objects, payment terminals.


* Section 1

Let's first implement the "dumb" version of the PaymentCard. The card only has ability for asking for the balance, adding money, and taking money. Complete the method `public bool TakeMoney(double amount)` in the class below (and found in the exercise template), using the following as a guide:

```cpp
namespace Exercise008
{
  public class PaymentCard
  {
    public double balance { get; private set; }

    public PaymentCard(double balance)
    {
      this.balance = balance;
    }

    public void AddMoney(double increase)
    {
      this.balance = this.balance + increase;
    }

    public bool TakeMoney(double amount)
    {
      // implement the method so that it only takes money from the card if
      // the balance is at least the amount parameter.
      // returns true if successful and false otherwise

      return false;
    }
  }
}
```

```cpp
static void Main(string[] args)
{
  PaymentCard petesCard = new PaymentCard(10);

  Console.WriteLine("money " + petesCard.balance);
  bool wasSuccessful = petesCard.TakeMoney(8);
  Console.WriteLine("successfully withdrew: " + wasSuccessful);
  Console.WriteLine("money " + petesCard.balance);

  wasSuccessful = petesCard.TakeMoney(4);
  Console.WriteLine("successfully withdrew: " + wasSuccessful);
  Console.WriteLine("money " + petesCard.balance);

}
```

Should print like this:

```console
money 10
successfully withdrew: True
money 2
successfully withdrew: False
money 2
```

* Section 2

When visiting a student cafeteria, the customer pays either with cash or with a payment card. The cashier uses a payment terminal to charge the card or to process the cash payment. First, let's create a terminal that's suitable for cash payments.

The outline of the payment terminal. The comments inside the methods tell the wanted functionality:

```cpp
namespace Exercise008
{
  public class PaymentTerminal
  {
    private double money;  // amount of cash
    private int coffeeAmount; // number of sold coffees
    private int lunchAmount;  // number of sold lunches

    public PaymentTerminal()
    {
      // register initially has 1000 euros of money
    }

    public double DrinkCoffee(double payment)
    {
      // an coffee now costs 2.50 euros
      // increase the amount of cash by the price of an coffee mean and return the change
      // if the payment parameter is not large enough, no coffee is sold and the method should return the whole payment
    }

    public double EatLunch(double payment)
    {
      // a lunch now costs 10.30 euros
      // increase the amount of cash by the price of a lunch and return the change
      // if the payment parameter is not large enough, no lunch is sold and the method should return the whole payment
    }

    public override string ToString()
    {
      return "money: " + money + ", number of sold coffees: " + coffeeAmount + ", number of sold lunches: " + lunchAmount;
    }
  }
}
```

The terminal starts with 1000 euros in it. Implement the methods so they work correctly, using the basis above and the example prints of the main program below.

```cpp
PaymentTerminal lunchCafeteria = new PaymentTerminal();

double change = lunchCafeteria.DrinkCoffee(10);
Console.WriteLine("remaining change " + change);

change = lunchCafeteria.DrinkCoffee(5);
Console.WriteLine("remaining change " + change);

change = lunchCafeteria.EatLunch(20);
Console.WriteLine("remaining change " + change);

Console.WriteLine(lunchCafeteria);
```

```console
remaining change 7.5
remaining change 2.5
remaining change 9.7
money: 1015.3, number of sold coffees: 2, number of sold lunches: 1
```

* Section 3

Let's extend our payment terminal to also support card payments. We are going to create new methods for the terminal. It receives a payment card as a parameter, and decreases its balance by the price of the meal that was purchased. Here are the outlines for the methods, and instructions for completing them.

```cpp
public bool DrinkCoffee(PaymentCard card)
{
  // a coffee costs 2.50 euros
  // if the payment card has enough money, the balance of the card is decreased by the price, and the method returns true
  // otherwise false is returned
}

public bool EatLunch(PaymentCard card)
{
  // a lunch costs 10.30 euros
  // if the payment card has enough money, the balance of the card is decreased by the price, and the method returns true
  // otherwise false is returned
}
```

Notice! Card payments do not increase the cash in the register.

```cpp
PaymentTerminal lunchCafeteria = new PaymentTerminal();

double change = lunchCafeteria.DrinkCoffee(10);
Console.WriteLine("remaining change: " + change);

PaymentCard annesCard = new PaymentCard(15);

bool wasSuccessful = lunchCafeteria.EatLunch(annesCard);
Console.WriteLine("there was enough money: " + wasSuccessful);
wasSuccessful = lunchCafeteria.EatLunch(annesCard);
Console.WriteLine("there was enough money: " + wasSuccessful);
wasSuccessful = lunchCafeteria.DrinkCoffee(annesCard);
Console.WriteLine("there was enough money: " + wasSuccessful);

Console.WriteLine(lunchCafeteria);
```

```console
remaining change: 7.5
there was enough money: True
there was enough money: False
there was enough money: True
money: 1002.5, number of sold coffees: 2, number of sold lunches: 1
```

* Section 4

Let's create a method for the terminal that can be used to add money to a payment card. Recall that the payment that is received when adding money to the card is stored in the register (adding cash). The basis for the method:

```cpp
public void AddMoneyToCard(PaymentCard card, double sum)
{
  // ...
}
```

A main program to illustrate:


```cpp
public static void Main(string[] args)
{
  // Try your code here, if you want

  PaymentTerminal lunchCafeteria = new PaymentTerminal();
  Console.WriteLine(lunchCafeteria);

  PaymentCard annesCard = new PaymentCard(2);

  Console.WriteLine("amount of money on the card is " + annesCard.balance + " euros");

  bool wasSuccessful = lunchCafeteria.EatLunch(annesCard);
  Console.WriteLine("there was enough money: " + wasSuccessful);

  lunchCafeteria.AddMoneyToCard(annesCard, 100);

  wasSuccessful = lunchCafeteria.EatLunch(annesCard);
  Console.WriteLine("there was enough money: " + wasSuccessful);

  Console.WriteLine("amount of money on the card is " + annesCard.balance + " euros");

  Console.WriteLine(lunchCafeteria);
}
```

```console
money: 1000, number of sold coffees: 0, number of sold lunches: 0
amount of money on the card is 2 euros
there was enough money: False
there was enough money: True
amount of money on the card is 91.7 euros
money: 1100, number of sold coffees: 0, number of sold lunches: 1
```

</Exercise>

<Exercise title={'009 Biggest pet shop'}>

Two classes, Person and Pet, are included in the exercise template. Each person has one pet. Modify the `public override string ToString` method of the `Person class` so that the string it returns tells the pet's name and breed in addition to the person's own name.

```cpp
public static void Main(string[] args)
{

  Pet lucy = new Pet("Lucy", "golden retriever");
  Person leo = new Person("Leo", lucy);
  Console.WriteLine(leo);

  Person mike = new Person("Mike");
  Console.WriteLine(mike);
  
  Person lilo = new Person();
  Console.WriteLine(lilo);
}
```

```console
Leo, has a friend called Lucy (golden retriever)
Lilo, has a friend called Stitch (blue alien)
Mike, has a friend called Toothless (dragon)
```

</Exercise>

<Exercise title={'010 Comparing apartments'}>

Fill in the method `public bool LargerThan(Apartment compared)` that returns true if the apartment object whose method is called has a larger total area than the apartment object that is being compared.

An example of how the method should work:

```cpp
public static void Main(string[] args)
{
  Apartment manhattanStudioApt = new Apartment(1, 16, 5500);
  Apartment atlantaTwoBedroomApt = new Apartment(2, 38, 4200);
  Apartment bangorThreeBedroomApt = new Apartment(3, 78, 2500);

  Console.WriteLine(manhattanStudioApt.LargerThan(atlantaTwoBedroomApt));
  Console.WriteLine(bangorThreeBedroomApt.LargerThan(manhattanStudioApt));
}
```

```console
False
True
```

Fill in the method `public int PriceDifference(Apartment compared)` that returns the price difference of the apartment object whose method was called and the apartment object received as the parameter. The price difference is the absolute value of the difference of the prices (price can be calculated by multiplying the price per square by the number of squares). Use the method `private int Price()` to calculate the price for the apartments.

An example of how the method should work:

```cpp
Apartment manhattanStudioApt = new Apartment(1, 16, 5500);
Apartment atlantaTwoBedroomApt = new Apartment(2, 38, 4200);
Apartment bangorThreeBedroomApt = new Apartment(3, 78, 2500);
Console.WriteLine(manhattanStudioApt.PriceDifference(atlantaTwoBedroomApt));
Console.WriteLine(bangorThreeBedroomApt.PriceDifference(manhattanStudioApt));
```

```console
71600
107000
```

Fill in the method `public bool MoreExpensiveThan(Apartment compared)` that returns true if the apartment object whose method is called is more expensive than the apartment object being compared.

An example of how the method should work:

```cpp
Apartment manhattanStudioApt = new Apartment(1, 16, 5500);
Apartment atlantaTwoBedroomApt = new Apartment(2, 38, 4200);
Apartment bangorThreeBedroomApt = new Apartment(3, 78, 2500);
Console.WriteLine(manhattanStudioApt.MoreExpensiveThan(atlantaTwoBedroomApt));
Console.WriteLine(bangorThreeBedroomApt.MoreExpensiveThan(manhattanStudioApt));
```

```console
False
True
```

</Exercise>

<Exercise title={'011 Song'}>

In the exercise base there is a class called `Song` that can be used to create new objects that represent songs. Add to that class the `Equals` method so that the similarity of songs can be examined.

You can try your code with this:

```cpp
public static void Main(string[] args)
{
  // Try your code here, if you want
  Song jackSparrow = new Song("The Lonely Island", "Jack Sparrow", 196);
  Song anotherSparrow = new Song("The Lonely Island", "Jack Sparrow", 196);

  if (jackSparrow.Equals(anotherSparrow))
  {
    Console.WriteLine("Songs are equal.");
  }

  if (jackSparrow.Equals("Another object"))
  {
    Console.WriteLine("Strange things are afoot.");
  }
}
```

</Exercise>

<Exercise title={'012 Books'}>

In the exercise base is a program which asks for books from the user and adds them to a list.

Modify the program so that books that are already on the list are not added to it again. Two books should be considered the same if they have the same name and publication year.

Example print:

```console
Name (empty will stop): 
> Bossypants 
Publication year: 
>2013 
Name (empty will stop):
> Seriously...I'm Kidding
Publication year:
> 2012 
Name (empty will stop):
> Seriously...I'm Kidding 
Publication year:
> 2012 
The book is already on the list. Let's not add the same book again. 
Name (empty will stop):
>

Thank you! Books added: 2
```

</Exercise>

<Exercise title={'013 Archive'}>

The program should read items from the user. When all the items from the user have been read, the program prints the information of each item.

For each item, its identifier and name should be read. If the identifier or name is empty, the program stops asking for input, and prints all the item information.

Example print:

```console
Identifier? (empty will stop):
> B07H8ND8HH 
Name? (empty will stop):
> He-Man figure
Identifier? (empty will stop):
> B07H8ND8HH 
Name? (empty will stop):
> He-Man 
Identifier? (empty will stop):
> B07NQFMZYG 
Name? (empty will stop):
> He-Man figure 
Identifier? (empty will stop):
> B07NQFMZYG 
Name? (empty will stop):
> He-Man figure
Identifier? (empty will stop):
>

==Items== 
B07H8ND8HH: He-Man figure 
B07NQFMZYG: He-Man figure
```


The printing format of the items should be identifier: name.

After entering the items, each item is printed at most once. Two items should be considered the same if their identifiers are the same (there can be variation in their names in different countries, for instance).

If the user enters the same item multiple times, the print uses the item that was added first.

<Note>It is probably smart to add each item to the list at most once -- compare the equality of the objects based on their identifiers.</Note>

</Exercise>

<Exercise title={'014 Dating app'}>

This exercise is worth 2 points without individual sections.

With the exercise base the class SimpleDate is supplied. The date is stored with the help of the object variables year, month, and day. There are some methods that need fulfilling:

- `public void Advance()` advances the date by one day.  In this exercise we assume that each month has 30 day. Remember! In certain situations you need to change the values of month and year.
- `public void Advance(int howManyDays)` advances the date by the amount of days. Use the method Advance() that you implemented to help you in this.
- `public SimpleDate AfterNumberOfDays(int days)`  It creates a new SimpleDate object whose date is the specified number of days greater than the object that the method was called on. You may still assume that each month has 30 days. Notice that the old date object must remain unchanged!

Since the last method must create a new object, the structure of the code should be somewhat similar to this:

```cpp
SimpleDate newDate = new SimpleDate( ... );

// Do something here

return newDate;
```

The whole class template looks like this:

```cpp
namespace Exercise014
{
  public class SimpleDate
  {
    private int day;
    private int month;
    private int year;

    public SimpleDate(int day, int month, int year)
    {
      this.day = day;
      this.month = month;
      this.year = year;
    }

    public void Advance() {
      // Do something here
    }

    public void Advance(int howManyDays) {
      // Do something here
    }

    public SimpleDate AfterNumberOfDays(int days) {
      SimpleDate newDate = new SimpleDate( ... );

      // Do something here

      return newDate;
    }

    
    public override string ToString()
    {
      return this.day + "." + this.month + "." + this.year;
    }

    // used to check if this date object (`this`) is before
    // the date object given as the parameter (`compared`)
    public bool Before(SimpleDate compared)
    {
      // first compare years
      if (this.year < compared.year)
      {
        return true;
      }

      // if the years are the same, compare months
      if (this.year == compared.year && this.month < compared.month)
      {
        return true;
      }

      // the years and the months are the same, compare days
      if (this.year == compared.year && this.month == compared.month &&
          this.day < compared.day)
      {
        return true;
      }

      return false;
    }
  }
}
```

Here's example use:
```cpp
public static void Main(string[] args)
{
  SimpleDate date = new SimpleDate(13, 2, 2015);
  Console.WriteLine("Friday of the examined week is " + date);

  SimpleDate newDate = date.AfterNumberOfDays(7);
  int week = 1;
  while (week <= 7)
  {
    Console.WriteLine("Friday after " + week + " weeks is " + newDate);
    newDate = newDate.AfterNumberOfDays(7);

    week = week + 1;
  }

  Console.WriteLine("The date after 790 days from the examined Friday is ... try it out yourself!");
  // Console.WriteLine("Try " + date.AfterNumberOfDays(790));

}
```

```console
Friday of the examined week is 13.2.2015
Friday after 1 weeks is 20.2.2015
Friday after 2 weeks is 27.2.2015
Friday after 3 weeks is 4.3.2015
Friday after 4 weeks is 11.3.2015
Friday after 5 weeks is 18.3.2015
Friday after 6 weeks is 25.3.2015
Friday after 7 weeks is 2.4.2015
The date after 790 days from the examined Friday is ... try it out yourself!
```

<Note>Instead of modifying the state of the old object we return a new one with AfterNumberOfDays. Imagine that the SimpleDate class has a method Advance that works similarly to the method we programmed, but it modifies the state of the old object. In that case the next block of code would cause problems.</Note>

```cpp
SimpleDate now = new SimpleDate(13, 2, 2015);
SimpleDate afterOneWeek = now;
afterOneWeek.Advance(7);

Console.WriteLine("Now: " + now);
Console.WriteLine("After one week: " + afterOneWeek);
```

```console
Now: 20.2.2015 
After one week: 20.2.2015
```

This is because a normal assignment only copies the reference to the object. So the objects now and afterOneWeek in the program now refer to the one and same SimpleDate object.

</Exercise>

<Exercise title={'015 Money'}>

This exercise is worth 2 points without individual sections.

In the Payment card exercise we used a double-type object variable to store the amount of money. In real applications this is not the approach you want to take, since as we have seen, calculating with doubles is not exact. A more reasonable way to handle amounts of money is create an own class for that purpose. Here is a layout for the class:

```cpp
namespace Exercise015
{
  public class Money
  {

    private int euros;
    private int cents;

    public Money(int euros, int cents)
    {
      if (cents > 99)
      {
        euros = euros + cents / 100;
        cents = cents % 100;
      }

      this.euros = euros;
      this.cents = cents;
    }

    public Money Plus(Money addition)
    {
      Money newMoney = new Money(/* Do something here*/);
      // create a new Money object that has the correct worth

      // return the new Money object
      return newMoney;
    }

    public Money Minus(Money decreaser)
    {
      Money newMoney = new Money(/* Do something here*/);
      // create a new Money object that has the correct worth

      // return the new Money object
      return newMoney;
    }

    public bool LessThan(Money compared)
    {
      // Do something here
      return false;
    }

    public string toString()
    {
      string zero = "";
      if (cents <= 10)
      {
        zero = "0";
      }

      return euros + "." + zero + cents + "e";
    }
  }
}
```

Next we'll create a few operations for processing money.

- First create the method `public Money Plus(Money addition)` that returns a new money object that is worth the total amount of the object whose mehtod was called and the object that is received as the parameter.

The basis for the method is the following:

```cpp
public Money Plus(Money addition)
{
  Money newMoney = new Money(/* Do something here*/);
  // create a new Money object that has the correct worth

  // return the new Money object
  return newMoney;
}
```
<Note>If the cents would go over 99 (100 or more), the euros should increase as well!</Note>

- create the method `public bool LessThan(Money compared)` that returns true if the money object whose method is called has a lesser worth than the money object that is received as the method parameter.

- Write the method `public Money Minus(Money decreaser)` that returns a new money object worth the difference of the object whose method was called and the object received as the parameter. If the difference would be negative, the worth of the created money object is set to 0.

<Note>If the cents would go under 0, the euros should decrease as well!</Note>

Example of all the methods:
```cpp
Money money = new Money(100, 00);
Money moreMoney = new Money(500, 50);

Money combined = money.Plus(moreMoney);

Console.WriteLine(money);
Console.WriteLine(moreMoney);
Console.WriteLine(combined);

Money lessMoney = moreMoney.Minus(money);

Console.WriteLine(money);
Console.WriteLine(moreMoney);
Console.WriteLine(lessMoney);

lessMoney = lessMoney.Minus(money);

Console.WriteLine(money);
Console.WriteLine(moreMoney);
Console.WriteLine(lessMoney);

Console.WriteLine(lessMoney.LessThan(moreMoney));
Console.WriteLine(lessMoney.LessThan(money));

lessMoney = lessMoney.Minus(moreMoney);
Console.WriteLine(lessMoney);
```

Prints out

```console
100.00e
500.50e
600.50e
100.00e
500.50e
400.50e
100.00e
500.50e
300.50e
True
False
0.00e
```

</Exercise>
