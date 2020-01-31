---
title: Lesson 3 - Events
has_children: false
nav_order: 4
description: C# Events
---

[![ad](../img/bootcamp.jpg)](https://rclapp.com/bootcamp.html)

****

## Events

Events in .NET are based on the delegate model. The delegate model follows the observer design pattern, which enables a subscriber to register with and receive notifications from a publisher. 

An event sender pushes a notification that an event has happened, and an event receiver receives that notification and defines a response to it.

1. In the publisher class, create an event using the built-in EventHandler delegate.

2. In the publisher, create a method to raise the event. 

3. In the publisher, raise the event based on a condition.

4. In the subscriber, create a method to handle the event when it is raised. This method should match the built-in EventHandler delegate.

5. In the subscriber, subscribe to the event

Write the following code :

```java
using System;

namespace HelloWorld
{
    class Program
    {
        // Custom EventArgs
        public class SpeedLimitEventArgs : EventArgs
        {
            public int Speed { get; set; }
            public int SpeedLimit { get; set; }
        }

        // Event publisher
        public class Car
        {
            public int SpeedLimit { get; set; }
            private int _speed = 0;

            public Car(int speedLimit)
            {
                SpeedLimit = speedLimit;
            }

            public int Speed
            {
                get
                {
                    return _speed;
                }
            }

            // 1. Create the event using the EventHandler delegate
            public event EventHandler<SpeedLimitEventArgs> SpeedLimitExceeded;

            // 2. Create a method to raise the event
            public virtual void OnSpeedLimitExceeded(SpeedLimitEventArgs e)
            {
                SpeedLimitExceeded?.Invoke(this, e);
            }

            public void Accelerate(int mph)
            {
                _speed += mph;

                // 3. Raise the event based on a condition
                if (_speed > SpeedLimit)
                {
                    SpeedLimitEventArgs eventArgs = new SpeedLimitEventArgs
                    {
                        Speed = _speed,
                        SpeedLimit = SpeedLimit
                    };

                    OnSpeedLimitExceeded(eventArgs);
                }     
            }
        }

        static void Main(string[] args)
        {
            // 4. Create a method to handle the event (must match EventHandler delegate)
            static void CarSpeedLimitExceeded(object source, SpeedLimitEventArgs e)
            {
                Console.WriteLine($"Speeding event raised : speed limit of {e.SpeedLimit}mph exceeded at {e.Speed}mph");
            }

            Car car = new Car(speedLimit:50);

            // 5. Subscribe to the event
            car.SpeedLimitExceeded += CarSpeedLimitExceeded;

            for (int i = 0; i < 3; i++)
            {
                car.Accelerate(20);
                Console.WriteLine("Speed: {0}mph", car.Speed);
            }
        }
    }
}
```

Output

```
Speed: 20mph
Speed: 40mph
Speeding event raised : speed limit of 50mph exceeded at 60mph
Speed: 60mph
```

****

[![ad](../img/online-mentoring.jpg)](https://rclapp.com/mentors.html)

****

<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = 'https://csharpadvanced.tutorial.rclapp.com/lessons/lesson3.html';
this.page.identifier = 'a05-03'; 
};
(function() { 
var d = document, s = d.createElement('script');
s.src = 'https://coding-skills-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>