---
title: Lesson 5 - Attributes 
has_children: false
nav_order: 5
description: C# Attributes
---

[![ad](../img/bootcamp.jpg)](https://rclapp.com/bootcamp.html)

****

# Attributes

Attributes provide a method for associating metadata with assemblies, classes, methods, properties, etc.

****

After an attribute is associated with an entity, the attribute can be queried at run time.

- Write the following code :

```csharp
using System;

namespace LearnCSharp
{
    class Program
    {
        static void Main(string[] args)
        {
            // Get the attribute
            Attribute[] attrs = Attribute.GetCustomAttributes(typeof(Adder));
            foreach (Attribute attr in attrs)
            {
                if (attr is Developer)
                {
                    Developer developer = (Developer)attr;
                    Console.WriteLine($"{developer.GetName()} wrote the class {nameof(Adder)}");
                }
            }
        }
    }
}
// Use the attribute
[Developer("Anil")]
public class Adder
{
    public int Execute(int x, int y)
    {
        return x + y;
    }
}
// Create an attribute
[AttributeUsage(AttributeTargets.Class)]
public class Developer : Attribute
{
    private string Name;

    public Developer(string name)
    {
        Name = name;
    }

    public string GetName()
    {
        return Name;
    }
}
```
**Output**
```
Anil wrote the class Adder
```

The Developer attribute has a property called Name that we will use to identity the developer who wrote a class.

We add the attribute to the Adder class and specify the name of the developer who wrote the class as metadata for the class.

Finally, we locate the Developer custom attribute for the Adder class and use it to print the name of the developer who wrote the class in the console window.

****

[![ad](../img/online-mentoring.jpg)](https://rclapp.com/mentors.html)

****

<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = 'https://csharpadvanced.tutorial.rclapp.com/lessons/lesson5.html';
this.page.identifier = 'f05-05'; 
};
(function() { 
var d = document, s = d.createElement('script');
s.src = 'https://coding-skills-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>