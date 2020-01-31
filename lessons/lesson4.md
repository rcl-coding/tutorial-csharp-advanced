---
title: Lesson 4 - Generics
has_children: false
nav_order: 5
description: C# Generics
---

[![ad](../img/bootcamp.jpg)](https://rclapp.com/bootcamp.html)

****

# Generics

Generics introduce the concept of type parameters to the .NET Framework, which make it possible to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code. 

- Consider the following code

```java
using System;

namespace HelloWorld
{
    class Program
    {
        public class ArrayCounter
        {
            public int GetStringArrayLength(string[] stringArray)
            {
                return stringArray.Length;
            }

            public int GetIntArrayLength(int[] intArray)
            {
                return intArray.Length;
            }
        }

        static void Main(string[] args)
        {
            string[] strings = new string[] { "one", "two", "three" };
            int[] ints = new int[] { 1, 2, 3, 4 };

            ArrayCounter arrayCounter = new ArrayCounter();

            Console.WriteLine($"Length of string array is {arrayCounter.GetStringArrayLength(strings)}");
            Console.WriteLine($"Length of int array is {arrayCounter.GetIntArrayLength(ints)}");
        }
    }
}
```

Output

```
Length of string array is 3
Length of int array is 4
```

The code looks a bit duplicitous. Following this example, we will need to create a method in the ArrayCounter class each time we have an array of a different type.

To solve this we can use Generics.

- Write the following code :

```java
using System;

namespace HelloWorld
{
    class Program
    {
        public class ArrayCounter<T>
        {
            public int GetArrayLength(T[] array)
            {
                return array.Length;
            }
        }

        static void Main(string[] args)
        {
            string[] strings = new string[] { "one", "two", "three" };
            int[] ints = new int[] { 1, 2, 3, 4 };

            // Specify the type T when the class is instantiated
            ArrayCounter<string> stringArrayCounter = new ArrayCounter<string>();
            ArrayCounter<int> intArrayCounter = new ArrayCounter<int>();

            Console.WriteLine($"Length of string array is {stringArrayCounter.GetArrayLength(strings)}");
            Console.WriteLine($"Length of int array is {intArrayCounter.GetArrayLength(ints)}");
        }
    }
}
```

Output

```
Length of string array is 3
Length of int array is 4
```

The ArrayCounter class now has a single method to get the length of the array. This method takes and array of type T (Generic).

```java
public int GetArrayLength(T[] array)
```

The specification for the generic type T is deferred to when the class is instantiated. 

```java
ArrayCounter<string> stringArrayCounter = new ArrayCounter<string>();
```

## List< T > example

The **List< T >** collection is a commonly used Generic. T can be any type.

The following code uses List<T> :

```java
using System;
using System.Collections.Generic;

namespace HelloWorld
{
    class Program
    {
        static void Main(string[] args)
        {
            // Specify the type T when the class is instantiated
            List<string> strings = new List<string> { "red", "green", "blue" };

            Console.WriteLine($"The first item in the list is {strings[0]}");
        }
    }
}
```

Output

```
The first item in the list is red
```

## Constraints

Constraints inform the compiler about the capabilities a type argument must have.

By constraining the type parameter, you increase the number of allowable operations and method calls to those supported by the constraining type and all types in its inheritance hierarchy.

- Write the following code :

```java
using System;

namespace HelloWorld
{
    class Program
    {
        public class Person
        {
            public string Name { get; set; }
            public int Age { get; set; }
        }

        // Add a constraint
        public class Profile<T> where T: Person
        {
            public string GetName(T t)
            {
                // The Property call (for Name) is now allowed
                return t.Name;
            }
        }

        static void Main(string[] args)
        {
            Person p = new Person
            {
                Name = "John Doe",
                Age = 20
            };

            Profile<Person> profile = new Profile<Person>();

            Console.WriteLine($"The name of the person is {profile.GetName(p)}");
        }
    }
}
```

```Output
The name of the person is John Doe
```

By adding the constraint that T must be of type Person, the properties (methods, etc) calls on the Person type is now allowed.

Commonly use constraints include :

- where T : class - T is a class
- where T : new() - T is a class with empty constructor
- where T : base-class-name - T is derived from a base class
- where T : interface-name - T is derived from an interface

- Write the following code :

```java
using System;

namespace HelloWorld
{
    class Program
    {
        public interface IProfile
        {
            string GetName();
        }

        public class Person : IProfile
        {
            public Person()
            {
            }

            public string Name { get; set; }
            public int Age { get; set; }

            public string GetName()
            {
                return Name;
            }
        }

        // Generics with multiple Constraints
        public class Profile<T> where T : Person, IProfile, new()
        {

            public string GetProfileName(T t)
            {
                return t.GetName();
            }
        }

        static void Main(string[] args)
        {
            Person p = new Person
            {
                Name = "John Doe",
                Age = 20
            };

            Profile<Person> profile = new Profile<Person>();

            Console.WriteLine($"The name of the person is {profile.GetProfileName(p)}");
        }
    }
}
```

Output

```
The name of the person is John Doe
```

The code above illustrates the use of multiple constraints.

## Multiple Generics and Constraints

You can apply constraints to multiple parameters, and multiple constraints to a single parameter, as shown in the following example:

```java
class Base { }
class Test<T, U>
    where U : struct
    where T : Base, new()
{ }
```

## Generic Methods

A generic method is a method that is declared with type parameters

- Write the following code :

```java
using System;

namespace HelloWorld
{
    class Program
    {
        // Generic method
        static int GetArrayLength<T>(T[] array)
        {
            return array.Length;
        }

        static void Main(string[] args)
        {
            int[] ints = new int[] { 1, 2, 3, 4 };
            Console.WriteLine($"The length of the int array is {GetArrayLength<int>(ints)}");
        }
    }
}
```

Output

```
The length of the int array is 4
```

The GetArrayLength method is a Generic method of type T.

## Generic Interfaces

The code below is an example of a Generic Interface.

- Write the following code :


```java
using System;
using System.Collections.Generic;

namespace HelloWorld
{
    class Program
    {
        // Generic Interface
        public interface IRepository<T>
        {
            T GetById(int id);
        }

        public class Book
        {
            public int id { get; set; }
            public string Title { get; set; }
        }

        // The Library class inherits the Generic interface 
        public class Library : IRepository<Book>
        {
            private List<Book> books = new List<Book>()
            {
                new Book{id = 1, Title = "Moby Dick"},
                new Book{id = 2, Title = "Oliver Twist"},
                new Book{id = 3, Title = "Animal Farm"}
            };

            // This method implements the interface method
            public Book GetById(int id)
            {
                return books.Find(f => f.id == id);
            }
        }

        static void Main(string[] args)
        {
            Library library = new Library();
            int id = 3;
            Console.WriteLine($"The book with id {id} is {library.GetById(id).Title}");
        }
    }
}
```

Output

```
The book with id 3 is Animal Farm
```

IRepository is a Generic Interface of type T.

****

[![ad](../img/online-mentoring.jpg)](https://rclapp.com/mentors.html)

****

<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = 'https://csharpadvanced.tutorial.rclapp.com/lessons/lesson4.html';
this.page.identifier = 'a05-04'; 
};
(function() { 
var d = document, s = d.createElement('script');
s.src = 'https://coding-skills-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>