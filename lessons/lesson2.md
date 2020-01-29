---
title: Lesson 2 - Delegates
has_children: false
nav_order: 3
description: C# Delegates
---

[![ad](../img/bootcamp.jpg)](https://rclapp.com/bootcamp.html)

****

## Delegates

Coders often need to pass a method as a parameter to other methods. For this purpose we create and use delegates.

- Add a delegate named DelegateAdder

- Add a method for the delegate named Adder

- Call the delegate as shown below :

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        // Add a delegate
        public delegate int DelegateAdder(int x, int y);

        // Add a method for the delegate
        public static int Adder(int x, int y)
        {
            return x + y;
        }

        static void Main(string[] args)
        {
            int _x = 1;
            int _y = 3;

            // Instantiate the delegate
            DelegateAdder delAdder = Adder;

            // Call the delegate
            int sum = delAdder(_x, _y);

            Console.WriteLine($"Sum of {_x} and {_y} is {sum}");
        }
    }
}
```

Output
```
Sum of 1 and 3 is 4
```

Delegates are created using the 'delegate' keyword. The delegate, DelegateAdder, can encapsulate a method that takes two int parameters (x and y) and returns an int.

The method, Adder, takes two int parameters and returns and int, therefore , the delegate can encapsulate this method.

We instantiate the delegate as follows :

```csharp
DelegateAdder delAdder = Adder;
```
We call the delegate as follows :

```csharp
int sum = delAdder(x, y);
```

## Using the Delegate as a parameter

- Write the following code :

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        // Add a delegate
        public delegate int DelegateOperation(int x, int y);

        // Add a method for the delegate
        public static int Adder(int x, int y)
        {
            return x + y;
        }

        // Add another method for the delegate
        public static int Multiplier(int x, int y)
        {
            return x * y;
        }

        // Add a method that takes the delegate as a parameter
        public static int MathOperation(int x, int y, DelegateOperation delOperation)
        {
            return delOperation(x, y);
        }

        static void Main(string[] args)
        {
            int _x = 1;
            int _y = 3;

            // Instantiate the delegate
            DelegateOperation delAdder = Adder;
            DelegateOperation delMultiplier = Multiplier;

            // Use the delegate as an argument in a method
            int sum = MathOperation(_x, _y, delAdder);
            int product = MathOperation(_x, _y, delMultiplier); 

            Console.WriteLine($"Sum of {_x} and {_y} is {sum}");
            Console.WriteLine($"Product of {_x} and {_y} is {product}");
        }
    }
}
```

Output
```
Sum of 1 and 3 is 4
Product of 1 and 3 is 3
```

The example above illustrates how a delegate can be used as a parameter to a method.

The MathOperation method uses the delegate, delegateOperation as a parameter.

## Delegate and lambda expressions

You are able to create "inline" delegates without having to specify any additional type or method. You simply inline the definition of the delegate where you need it.

- Write the following code :

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        public delegate int DelegateOperation(int x, int y);

        public static int MathOperation(int x, int y, DelegateOperation delOperation)
        {
            return delOperation(x, y);
        }

        static void Main(string[] args)
        {
            int _x = 1;
            int _y = 3;

            // Add the delegate as a inline lambda expression
            int sum = MathOperation(_x, _y, 
                (int x, int y) => {
                  return  x + y;
                });

            Console.WriteLine($"Sum of {_x} and {_y} is {sum}");
        }
    }
}
```

Output

```
Sum of 1 and 3 is 4
```

In the lambda expression, (int x, int y) are the inputs. The => symbol denotes a method.

In this case, the lambda expression can be further shortened by removing method notations and inferring the types for the parameters x and y.

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        public delegate int DelegateOperation(int x, int y);

        public static int MathOperation(int x, int y, DelegateOperation delOperation)
        {
            return delOperation(x, y);
        }

        static void Main(string[] args)
        {
            int _x = 1;
            int _y = 3;

            // Add the delegate as a inline lambda expression
            int sum = MathOperation(_x, _y, (x,y) => x + y);

            Console.WriteLine($"Sum of {_x} and {_y} is {sum}");
        }
    }
}
```

## Func Delegate

The .NET framework has some built-in delegates. The Func delegate is one of them. The Func delegate encapsulates a method that returns a type T. The method can take 0, 1 or many parameters.

You do not need to define a custom method for the Func delegate.

- Write the following code :

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        public static int MathOperation(int x, int y, Func<int,int,int> func)
        {
            return func(x, y);
        }

        static void Main(string[] args)
        {
            int _x = 1;
            int _y = 3;

            int sum = MathOperation(_x, _y, (x,y) => x + y);
            Console.WriteLine($"Sum of {_x} and {_y} is {sum}");
        }
    }
}
```

Output

```
Sum of 1 and 3 is 4
```

The MathOperation method takes a Func delegate as a parameter. The Func takes two int parameters and returns an int.

We use a lambda expression to use the Func to add two numbers.

```csharp
(x,y) => x + y
```

## Action Delegate

The Action delegate is a built-in delegate. The Action delegate returns void. It can 0, 1 or many parameters.

- Write the following code :

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        public static void OutputLine(string s, Action<string> action)
        {
            action(s);
        }

        static void Main(string[] args)
        {
            OutputLine("Hello World!",s => Console.WriteLine(s));
        }
    }
}
```

Output

```
Hello World!
```

The OutputLine method takes an Action delegate as a parameter. The delegate returns void and takes one string parameter.

A lambda expression is used to write to the console window using the delegate.

```csharp
s => Console.WriteLine(s)
```

## Predicate Delegate

The Predicate delegate is a built-in delegate. It is used when you need to determine if the argument satisfies the condition of the delegate.

It is equivalent to Func<T,bool>

- Write the following code :

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        public static bool HasWon(int score, Predicate<int> predicate)
        {
            return predicate(score);
        }

        static void Main(string[] args)
        {
            bool b = HasWon(20, s => s > 50);
            Console.WriteLine(b);
        }
    }
}
```

The HasWon method takes a predicate. We use the lambda expression :

```csharp
s => s > 50
```

to determine if the score is more than 50.

### Call Backs

You can use delegates for call back patterns. The following code illustrates this :

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        public static string GetReponse()
        {
            return "200";
        }

        public static void MakeRequest(Action<string> callBack)
        {
            string resp = GetReponse();
            callBack(resp);
        }

        static void Main(string[] args)
        {
            // Define a method to handle the call back
            MakeRequest(response => Console.WriteLine($"Responded with {response}"));
        }
    }
}
```

Output
```
Responded with 200
```

The MakeRequest method uses an Action delegate to define a call back method.

The call back method processes the result of the GetResponse method.

## Multicast Delegate

Multiple objects can be assigned to one delegate instance by using the + operator. 

The multicast delegate contains a list of the assigned delegates. When the multicast delegate is called, it invokes the delegates in the list, in order. Only delegates of the same type can be combined.

The - operator can be used to remove a component delegate from a multicast delegate.

- Write the following code :

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        // Add a delegate
        delegate void greeterDelegate(string s);

        // Add a method for the delegate
        static void Hello(string s)
        {
            Console.WriteLine($"Hello, {s}!");
        }

        // Add another method for the delegate
        static void Goodbye(string s)
        {
            Console.WriteLine($"Goodbye, {s}!");
        }

        static void Main(string[] args)
        {
            // Instantiate the delegates
            greeterDelegate hi = Hello;
            greeterDelegate bye = Goodbye;

            // Combine delegates
            greeterDelegate multiDelegate = hi + bye;
            // Remove a delegate
            greeterDelegate minusDelegate = hi - bye;

            // Call the delegates
            multiDelegate("Combined Delegate");
            minusDelegate("Minus Delegate");
        }
    }
}

```

Output

```
  Hello, Combined Delegate!
  Goodbye, Combined Delegate!
  Hello, Minus Delegate!
```

In the example above , we combine delegates with the + operator

```csharp
greeterDelegate multiDelegate = hi + bye
```

We remove a delegate component with the - operator

```csharp
greeterDelegate minusDelegate = hi - bye;
```

****

[![ad](../img/online-mentoring.jpg)](https://rclapp.com/mentors.html)

****

<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = 'https://csharpadvanced.tutorial.rclapp.com/lessons/lesson2.html';
this.page.identifier = 'f05-02'; 
};
(function() { 
var d = document, s = d.createElement('script');
s.src = 'https://coding-skills-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>



