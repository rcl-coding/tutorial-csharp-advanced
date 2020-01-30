---
title: Lesson 8 - Factories
has_children: false
nav_order: 9
description: C# Factories
---

[![ad](../img/bootcamp.jpg)](https://rclapp.com/bootcamp.html)

****

## Factory Method

The Factory Method is a frequently used creational design pattern. The responsibility for instantiating objects is delegated to a factory.

- Write the following code :

```csharp
using System;

namespace LearnCSharp
{
    class Program
    {
        static void Main(string[] args)
        {
            // Object creation is delegated to the factory
            MessageSenderFactory factory = new MessageSenderFactory();
            IMessageSender myMessageSender = factory.Create("sms");
            Console.WriteLine($"{myMessageSender.TransmitMessage()}");
        }
    }
}

// Factory
public class MessageSenderFactory
{
    public IMessageSender Create(string senderType)
    {
        if(senderType == "email")
        {
            return new Email();
        }
        else if(senderType == "sms")
        {
            return new SMS();
        }
        else
        {
            throw new NotImplementedException();
        }
    }
}

// Interface
public interface IMessageSender
{
    string TransmitMessage();
}

// Concrete class A
public class Email : IMessageSender
{
    public string TransmitMessage()
    {
        return "Message sent with email";
    }
}

// Concrete class B
public class SMS : IMessageSender
{
    public string TransmitMessage()
    {
        return "Message sent with SMS";
    }
}
```

Output

```
Message sent with SMS
```

The MessageSenderFactory has been delegated the responsibility for creating the objects (Email and SMS).

The IMessageSender defines the interface for the objects (Email and SMS) that the factory creates.

## Abstract Factory

A factory that creates other factories, and these factories in turn create objects derived from interfaces.

- Write the following code :

```csharp
using System;

namespace LearnCSharp
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("What meal do you prefer? (V)egan or (M)eat?");
            char input = Console.ReadKey().KeyChar;
            ICuisineFactory cuisineFactory;
            switch (input)
            {
                case 'V':
                    cuisineFactory = new VegetableDishFactory();
                    break;
                case 'M':
                    cuisineFactory = new MeatDishFactory();
                    break;
                default:
                    throw new NotImplementedException();
            }
            IMeal meal = cuisineFactory.Create();
            Console.WriteLine($"\nI got a {meal.Get()}");
        }
    }
}

// Abstract factory
public interface ICuisineFactory
{
    IMeal Create();
}

// Concrete factory A
public class VegetableDishFactory : ICuisineFactory
{
    public IMeal Create()
    {
        return new VegetableSoup();
    }
}

// Concrete factory B
public class MeatDishFactory : ICuisineFactory
{
    public IMeal Create()
    {
        return new Steak();
    }
}

// Abstract product
public interface IMeal
{
    string Get();
}

// Concrete product A
public class Steak : IMeal
{
    public string Get()
    {
        return "steak";
    }
}

// Concrete product B
public class VegetableSoup : IMeal
{
    public string Get()
    {
        return "vegetable soup";
    }
}
```

output

```
What meal do you prefer? (V)egan or (M)eat?
M
I got a steak
```

The ICuisineFactory interface is used to create the VegetableDishFactory and the MeatDishFactory.

These factories in turn create objects from the VegetableSoup and Steak classes using the IMeal interface.

****

[![ad](../img/online-mentoring.jpg)](https://rclapp.com/mentors.html)

****

<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = 'https://csharpadvanced.tutorial.rclapp.com/lessons/lesson8.html';
this.page.identifier = 'f05-08'; 
};
(function() { 
var d = document, s = d.createElement('script');
s.src = 'https://coding-skills-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>