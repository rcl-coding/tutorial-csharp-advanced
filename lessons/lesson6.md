---
title: Lesson 5 - Async Programming
has_children: false
nav_order: 5
description: C# Async Programming
---

[![ad](../img/bootcamp.jpg)](https://rclapp.com/bootcamp.html)

****

# Task asynchronous programming model

Asynchrony is essential for activities that are potentially blocking, such as web access. 

If such an activity is blocked in a synchronous process, the entire application must wait. 

In an asynchronous process, the application can continue with other work that doesn't depend on the web resource until the potentially blocking task finishes.

- Write the following code

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

namespace HelloWorld
{
    class Program
    {
        public static async Task<string> GetWebResponseAsync()
        {
            return await Task.Run(() =>
            {
                // Simulate a long running process
                Thread.Sleep(6000);
                return "200 OK";
            });
        }

        public static void DoIndependentWork()
        {
            Console.Write("Doing independent work ...\r\n");
        }

        static async Task Main(string[] args)
        {
            // 1. This Makes a request and yields to the caller 'Main"
            Task<string> getResponseTask = GetWebResponseAsync();
            // 2. You can continue to do independent work
            DoIndependentWork();
            // 3. Waits for the response to be returned, does not block the current thread
            string response = await getResponseTask;
            // 4. Proceeds when the response is returned
            Console.WriteLine($"Received web response : {response} ...\r\n");
        }
    }
}
```

1. Make a request to a long running process. Then, yield to the caller, 'Main', so that work can continue.

2. Continue with independent work in the caller method, 'Main'.

3. Wait for the request to complete before proceeding. The 'await' operator ensures that the caller thread is not blocked. The string result is stored in the task that represents the completion of the method. The 'await' operator retrieves the result from the task.

4. Proceed with the code when the string response is successfully retrieved.

****

[![ad](../img/online-mentoring.jpg)](https://rclapp.com/mentors.html)

****

<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = 'https://csharpadvanced.tutorial.rclapp.com/lessons/lesson6.html';
this.page.identifier = 'f05-06'; 
};
(function() { 
var d = document, s = d.createElement('script');
s.src = 'https://coding-skills-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>