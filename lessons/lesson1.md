---
title: Lesson 1 - Dependency Injection
has_children: false
nav_order: 2
description: C# Dependency Injection
---

[![ad](../img/bootcamp.jpg)](https://rclapp.com/bootcamp.html)

****

# Dependency inversion principle

This principle tells you not to write any tightly coupled code. It focuses on the approach where the higher classes are not dependent on the lower classes, but instead, depend upon the abstraction of the lower classes. 

Consider the following tightly coupled code.

```java
using System;

namespace LearnCSharp
{
    class Program
    {
        static void Main(string[] args)
        {
            MessageBroker messageBroker = new MessageBroker();
            string msg = messageBroker.TransmitMessage();
            Console.WriteLine($"{msg}");
        }
    }
}

public class Email
{
    public string SendEmail()
    {
        return "Message sent by email";
    }
}

public class MessageBroker
{
    private Email _email;

    public MessageBroker()
    {
        _email = new Email();
    }

    public string TransmitMessage()
    {
        return _email.SendEmail();
    }
}
   
```
**Output**
```
Message sent by email
```

The MessageBroker class is tightly coupled to the Email class. This is depicted by the instantiation of the Email class using the 'new' keyword in the constructor of the MessageBroker class.

If we want to use SMS instead of Email, we will need to modify the MessageBroker class. 

To fix the problem, we will introduce an abstraction for sending messages. The MessageBroker class will depend on this abstraction rather than the concrete Email class.

- Update the code as follows :

```java
using System;

namespace LearnCsharp
{
    class Program
    {
        static void Main(string[] args)
        {
            MessageBroker messageBroker = new MessageBroker(new SMSSender());
            string msg = messageBroker.TransmitMessage();
            Console.WriteLine($"{msg}");
        }
    }
}

// Abstraction
public interface IMessageSender
{
    string SendMessage();
}

// Concrete class A
public class EmailSender : IMessageSender
{
    public string SendMessage()
    {
        return "Message sent by email";
    }
}

// Concrete class B
public class SMSSender : IMessageSender
{
    public string SendMessage()
    {
        return "Message sent by SMS";
    }
}

// Dependency inversion, class depends on abstraction
public class MessageBroker
{
    private readonly IMessageSender _messageSender;

    public MessageBroker(IMessageSender messageSender)
    {
        _messageSender = messageSender;
    }

    public string TransmitMessage()
    {
        return _messageSender.SendMessage();
    }
}
```
**Output**
```
Message sent by SMS
```

The MessageBroker now depends on the IMessageSender interface, thus satisfying the dependency inversion principle. 

The MessageBroker class takes the IMessageSender interface in its constructor, it is 'injected' into the constructor. This forms the basis of **Dependency Injection**. 

The EmailSender and SMSSender class inherits fom the interface. This allows the MessageBroker class to send both Email and SMS messages.

****

[![ad](../img/online-mentoring.jpg)](https://rclapp.com/mentors.html)

****

<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = 'https://csharpadvanced.tutorial.rclapp.com/lessons/lesson1.html';
this.page.identifier = 'a05-01'; 
};
(function() { 
var d = document, s = d.createElement('script');
s.src = 'https://coding-skills-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>