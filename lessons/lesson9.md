---
title: Lesson 9 - SOLID
has_children: false
nav_order: 10
description: C# SOLID Principles
---

[![ad](../img/bootcamp.jpg)](https://rclapp.com/bootcamp.html)

****

# SOLID principles

SOLID is an acronym for 5 design principles when doing Object Oriented Programming. These principles were introduced by Robert C. Martin in his 2000 paper 'Design Principles and Design Patterns'.

****

## S - Single responsibility principle

A class should have only one responsibility. Consider the following class:

```java
using System;

namespace DesignPatterns
{
    public class GenerateReport
    {
        public void CreateReport()
        {
            Console.WriteLine("Report generated");
        }

        public void SaveFile()
        {
            Console.WriteLine("File Saved");
        }

        public void DownloadFile()
        {
            Console.WriteLine("File Downloaded");
        }
    }
}
```

The GenerateReport class violates the single responsibility principle since the SaveFile() and DownloadFile() methods are not directly related to generating a report. 

We can fix this violation by creating separate classes for saving and downloading files.

```java
using System;

namespace DesignPatterns
{
    public class GenerateReport
    {
        public void CreateReport()
        {
            Console.WriteLine("Report generated");
        }
    }

    public class FileSave
    {
        public void SaveFile()
        {
            Console.WriteLine("File Saved");
        }
    }

    public class FileDownload
    {
        public void DownloadFile()
        {
            Console.WriteLine("File Downloaded");
        }
    }
}
```

## O - Open for Extension, Closed for Modification

A class should be open for extension but closed for modification. The EmployeePayment class below violates this principle.


```java
using System;

namespace LearnCSharp
{
    class Program
    {
        static void Main(string[] args)
        {
            EmployeePayment employeePayment = new EmployeePayment();
            string pay = employeePayment.CalculatePay("A");

            Console.WriteLine($"The calculated pay is {pay}");
        }
    }
}

public class EmployeePayment
{
    public string CalculatePay(string EmployeeClass)
    {
        string pay = string.Empty;

        if (EmployeeClass == "A")
        {
            pay = "1,500";
        }
        else if (EmployeeClass == "B")
        {
            pay = "2,000";
        }

        return pay;
    }
}
```
**Output**
```
The calculated pay is 1,500
```

How would we calculate the pay if we had a new EmployeeClass, say employee class 'C'? 

We would need to modify the EmployeePayment class and add another 'if' statement. But this violates the principle, a class should be closed for modification. 

Also, the EmployeePayment class is closed for extension, since the CalculatePay() method cannot be overridden. A class should be open for extension.

We can use an interface to fix the problem.

```java
using System;

namespace LearnCSharp
{
    class Program
    {
        static void Main(string[] args)
        {
            IEmployeePayment employeePayment = new EmployeeClassA();
            string pay = employeePayment.CalculatePay();

            Console.WriteLine($"The calculated pay is {pay}");
        }
    }
}

// Interface is open for extension and closed for modification
public interface IEmployeePayment
{
    string CalculatePay();
}

// Concrete class A
public class EmployeeClassA : IEmployeePayment
{
    public string CalculatePay()
    {
        return "1,500";
    }
}

// Concrete class B
public class EmployeeClassB : IEmployeePayment
{
    public string CalculatePay()
    {
        return "2,000";
    }
}
```
**Output**
```
The calculated pay is 1,500
```

The interface IEmployeePayment can be inherited from any new EmployeeClass, say employee class 'C'. Therefore, it is open for extension.

You do not need to change the interface if you need to add a new payment class, you just need to provide a concrete implementation for the CalculatePay() method. It is closed for modification.

## L - Liskov substitution principle

A child class should be replaceable (substitutable) with its parent class. Child classes should not break parent classes. The following code violates this principle. 

If you run this code , you will get an exception.

```java
using System;
using System.Collections.Generic;

namespace LearnCSharp
{
    class Program
    {
        static void Main(string[] args)
        {
            int amt = 0;
            List<IEmployeePay> pension = new List<IEmployeePay>();
            pension.Add(new PermanentEmployee());
            pension.Add(new TemporaryEmployee());
            foreach (IEmployeePay payslip in pension)
            {
                amt = amt + payslip.CalculatePensionContribution();
            }

            Console.WriteLine($"The total pension contribution is {amt}");
        }
    }
}

public interface IEmployeePay
{
    int CalculatePay();
    int CalculatePensionContribution();
}

public class PermanentEmployee : IEmployeePay
{
    public int CalculatePay()
    {
        return 1500;
    }

    public int CalculatePensionContribution()
    {
        return 50;
    }
}

public class TemporaryEmployee : IEmployeePay
{
    public int CalculatePay()
    {
        return 350;
    }

    public int CalculatePensionContribution()
    {
        throw new NotImplementedException();
    }
}
```

In the code above, the child class, 'TemporaryEmployee', breaks the inherited parent interface, 'IEmployeePay', since it does not implement the CalculatePensionContribution() method. This is because a temporary employee does not get a pension. 

This violates the Liskov substitution principle.

# I - Interface segregation principle

This principle states that a class should not be forced to use an interface which is irrelevant to it. 

In the previous example, we saw the TemporaryEmployee class being forced to implement the CalculatePensionContribution() method that is irrelevant to it, since temporary employees do not receive a pension.

We can fix the code by creating an additional interface named IEmployeePension and segregating the CalculatePensionContribution() method.

```java
using System;
using System.Collections.Generic;

namespace LearnCSharp
{
    class Program
    {
        static void Main(string[] args)
        {
            int amt = 0;
            List<IEmployeePension> pension = new List<IEmployeePension>();
            pension.Add(new PermanentEmployee());
            foreach (IEmployeePension payslip in pension)
            {
                amt = amt + payslip.CalculatePensionContribution();
            }

            Console.WriteLine($"The total pension contribution is {amt}");
        }
    }
}

public interface IEmployeePay
{
    int CalculatePay();
}

public interface IEmployeePension
{
    int CalculatePensionContribution();
}

public class PermanentEmployee : IEmployeePay, IEmployeePension
{
    public int CalculatePay()
    {
        return 1500;
    }

    public int CalculatePensionContribution()
    {
        return 50;
    }
}

public class TemporaryEmployee : IEmployeePay
{
    public int CalculatePay()
    {
        return 350;
    }
}
```
**Output**
```
The total pension contribution is 50
```

The child classes (TemporaryEmployee and PermanentEmployee) now implement interfaces that are only relevant to its operation.

## D - Dependency inversion principle

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

If we want to use SMS instead of Email, we will need to modify the MessageBroker class. This also violates the 'open - closed' principle.

To fix the problem, we will introduce an abstraction for sending messages. The MessageBroker class will depend on this abstraction rather than the concrete Email class.

```java
using System;

namespace LearnCSharp
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

The EmailSender and SMSSender class inherits fom the interface. This allows the MessageBroker class to send both Email and SMS messages.

****

[![ad](../img/online-mentoring.jpg)](https://rclapp.com/mentors.html)

****

<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = 'https://csharpadvanced.tutorial.rclapp.com/lessons/lesson9.html';
this.page.identifier = 'a05-09'; 
};
(function() { 
var d = document, s = d.createElement('script');
s.src = 'https://coding-skills-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>