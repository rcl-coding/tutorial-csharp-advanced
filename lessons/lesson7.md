---
title: Lesson 7 - Extensions
has_children: false
nav_order: 8
description: C# Extensions
---

[![ad](../img/bootcamp.jpg)](https://rclapp.com/bootcamp.html)

****

# Extension 

Extension methods enable you to "add" methods to existing types without creating a new derived type, recompiling, or otherwise modifying the original type. Extension methods are a special kind of static method, but they are called as if they were instance methods on the extended type.

## Create an extension

- Create a new C# file and add the following code

```csharp
using System;

namespace HelloWorld
{
    public static class MyExtensions
    {
        // Extension method for String type
        public static string Reverse(this String s)
        {
            char[] charArray = s.ToCharArray();
            Array.Reverse(charArray);
            return new string(charArray);
        }
    }
}
```

Extension methods are defined as static methods but are called by using instance method syntax. 

Their first parameter specifies which type the method operates on, and the parameter is preceded by the this modifier.

The example shows an extension method defined for the System.String class. Note that it is defined inside a non-nested, non-generic static class.

## Use an extension

```csharp
namespace HelloWorld
{
    class Program
    {
        static void Main(string[] args)
        {
            string s = "olleh";
            // Use the extension
            string reverse = s.Reverse();
            Console.WriteLine(reverse);
        }
    }
}
```

Output

```
hello
```

The example above demonstrates how the extension is used.

****

[![ad](../img/online-mentoring.jpg)](https://rclapp.com/mentors.html)

****

<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = 'https://csharpadvanced.tutorial.rclapp.com/lessons/lesson7.html';
this.page.identifier = 'f05-07'; 
};
(function() { 
var d = document, s = d.createElement('script');
s.src = 'https://coding-skills-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>