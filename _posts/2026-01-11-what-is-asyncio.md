---
layout: default
title: "What is asyncio and why does it matter"
description: "Lets look how python actually gets over GIL and gives you performnace"
---

## What is asyncio and why does it matter

TL;DR


0. Why in first place do we need to talk about this- 

    Many applications, especially in today’s world of web applications, rely heavily on I/O (input/output) operations. These types of operations include downloading the contents of a web page from the internet, communicating over a network with a group of microservices, or running several queries together against a database such as MySQL or Postgres. A web request or communication with a microservice may take hundreds of milliseconds, or even seconds if the network is slow.

    Making many of these I/O requests at once can lead to substantial performance issues. If we run these requests one after another as we would in a sequentially run application, we’ll see a compounding performance impact. As an example, if we’re writing an application that needs to download 100 web pages or run 100 queries, each of which takes 1 second to execute, our application will take at least 100 sec- onds to run. However, if we were to exploit concurrency and start the downloads and wait simultaneously, in theory, we could complete these operations in as little as 1 second.



1. What is asyncio? -> 

    Many of us have seen this before in buggy user interfaces, where we happily click around until the application freezes, leaving us with a spinner or an unresponsive user interface. This is an example of an application being blocked leading to a poor user experience.

    This can cause performance and responsiveness issues, as we can only have one long operation running at any given time, and that operation will stop our application from doing anything else.

    THE HERO CONCURRENCY COMES IN

    aSYNCIO OUR HERO - Python library that allows us to run code using an asynchronous programming model. This lets us handle multiple I/O operations at once, while still allowing our application to remain responsive.

    asyncio is a library to execute these coroutines(A coroutine is a method that can be paused when we have a potentially long-running task and then resumed when that task is finished.) in an asynchronous fashion using a concurrency model known as a single-threaded event loop.

**I/O bound vs CPU bound operations(YES asyncio can handle both like other)**

In the case of a CPU-bound operation(computing the digits of pi or looping over the contents of a dictionary, applying business logic), it would complete faster if our CPU was more powerful, for instance by increasing its clock speed from 2 GHz to 3 GHz. 

In the case of an I/O-bound operation(making a request to a web server or reading a file from our machine’s hard drive), it would get faster if our I/O devices could handle more data in less time. This could be achieved by increasing our network bandwidth through our ISP or upgrading to a faster network card.


#### Understanding concurrency, parallelism and multitasking
We’ll learn more about what concurrency means and how asyncio uses a con- cept called multitasking to achieve it. Concurrency and parallelism are two concepts that help us understand how programming schedules and carries out various tasks, methods, and routines that drive action.

1. Concurrency
    Happening at same time.
    Say two cakes are to be made, first we pre heat at same time we prepare batter for cake 1 then rest then since 1st is 
    done and oven is still pre hrating we can make 2nd cake batter as well.
    In this model, we’re switching between different tasks concurrently. This switching between tasks (doing something else while the oven heats, switching between two different cakes) is concur- rent behavior.

2. Parallelism 
    When we say something is running in parallel, we mean not only are there two or more tasks happening concurrently, but they are also executing at the same time.  
    Two people making batter at once is parallel because we have two distinct tasks running concurrently.

    ![Parallel]({{ site.baseurl }}/assets/images/concurrency_vs_parallelism.png)

    Bottom line - With concurrency, we have multiple tasks happening at same time, but only one is actively done at a particular time.
    With parallelism we have multiple task happening and are active simultaneously 

    In a system that is only concurrent, we can switch between running these applications, running one application for a short while before letting the other one run. If we do this fast enough, it gives the appearance of two things hap- pening at once. In a system that is parallel, two applications are running simultane- ously, and we’re actively running two things concurrently.

    ![CPU]({{ site.baseurl }}/assets/images/CPU_Con_vs_Par.png)


    While parallelism implies concurrency, concurrency does not always imply parallelism.

    A multithreaded application running on a multiple-core machine is both concur- rent and parallel. In this setup, we have multiple tasks running at the same time, and there are two cores independently executing the code associated with those tasks. However, with multitasking we can have multiple tasks happening concurrently, yet only one of them is executing at a given time.

    asyncio uses cooperative multitasking to achieve concurrency. When our application reaches a point where it could wait a while for a result to come back, we explicitly mark this in code. This allows other code to run while we wait for the result to come back in the background. Once the task we marked has completed, we in effect “wake up” and resume executing the task. This gives us a form of concurrency because we can have multiple tasks started at the same time but, importantly, not in parallel because they aren’t executing code simultaneously.
    Cooperative multitasking has benefits over preemptive multitasking. First, cooper- ative multitasking is less resource intensive. When an operating system needs to switch between running a thread or process, it involves a context switch. Context switches are intensive operations because the operating system must save information about the running process or thread to be able to reload it.

    3. Multi-tasking
    with multitasking we can have multiple tasks happening concurrently, yet only one of them is executing at a given time.
        1. PREEMPTIVE MULTITASKING - os decide based on time_slicing
        2. COOPERATIVE MULTITASKING - we decide based on code
            Cooperative multitasking has benefits over preemptive multitasking. First, cooper- ative multitasking is less resource intensive. When an operating system needs to switch between running a thread or process, it involves a context switch. Context switches are intensive operations because the operating system must save information about the running process or thread to be able to reload it.
            A second benefit is granularity. An operating system knows that a thread or task should be paused based on whichever scheduling algorithm it uses, but that might not be the best time to pause. With cooperative multitasking, we explicitly mark the areas that are the best for pausing our tasks. 

#### I/O-Bound vs. CPU-Bound Operations (Continued)
In the case of an I/O-bound operation, our application spends most of its time waiting on a network card, a hard drive, or other input/output devices. Examples include making a request to a web server or reading a large file from a disk.If we increased our network bandwidth or upgraded to a faster SSD, these operations would complete faster. asyncio is specifically designed to handle these "waiting" periods by allowing other code to run while the I/O device does its work.

#### Concurrency vs. Parallelism
Concurrency is about multiple tasks being "in progress" at once, but they aren't necessarily executing at the exact same millisecond. Think of a single baker switching between mixing batter for Cake #1 and then checking the oven for Cake #2. The baker is doing one thing at a time but managing two tasks concurrently. Parallelism, on the other hand, means tasks are executing simultaneously. This requires a second baker (or a second CPU core) so that two people can mix batter at the exact same time. While parallelism implies concurrency, concurrency does not always imply parallelism—especially on a single-core machine where the OS must switch between tasks quickly to give the appearance of simultaneous work. 


#### Understanding Processes, Threads, Multithreading, and Multiprocessing 
Process: An application run that has its own dedicated memory space which other applications cannot access. When you start a Python script, you start a process.

**Thread:** Lighter-weight constructs that live inside a process. Threads are unique because they share the same memory as the process that created them.

**Multithreading:** Using multiple threads within a single process to handle work concurrently. In Python, this is mostly useful for I/O-bound work due to the limitations of the GIL.

**Multiprocessing:** Creating multiple child processes to distribute work. This is the best approach for CPU-intensive work, as each process gets its own Python interpreter and its own memory.


#### The Global Interpreter Lock (GIL)
The GIL is a controversial lock that prevents a single Python process from executing more than one Python bytecode instruction at a time. This means that even if your computer has 32 CPU cores, a single Python process with 10 threads will still only use one core at any given moment for Python code.

**Why does it exist?** 
It protects Python's memory management. Python uses reference counting to track objects; if two threads tried to increment a reference count at the same time, it could lead to a race condition where memory is deleted while still in use.

**The Loophole:** 
The GIL is released during I/O operations. This is why asyncio and multithreading can still make I/O-bound apps much faster—they wait for the network while the GIL is "unlocked," allowing other parts of the program to run.


#### **How an Event Loop Works**
The Event Loop is the engine of any asyncio application. It manages a queue of Tasks (wrappers around coroutines). The loop starts a task and runs it until it hits an await point (usually an I/O operation). The task "pauses," and the event loop registers the I/O operation (like a network socket) with the Operating System's notification system (such as epoll on Linux or kqueue on macOS).

Instead of waiting, the loop picks up the next ready task from the queue and runs it. When the OS notifies the loop that the I/O is finished, the loop "wakes up" the original task and resumes it from where it left off.


This allows a single thread to handle thousands of connections by simply managing the waiting rather than being blocked by it. 


#### **Summary!**
CPU-bound work needs more processors; I/O-bound work needs better waiting strategies.
Processes have separate memory; Threads share memory.
The GIL limits us to one Python instruction at a time but is released during I/O.
asyncio uses Cooperative Multitasking, where we explicitly tell the event loop when it’s okay to switch tasks using await.
The Event Loop achieves high concurrency on a single thread by offloading I/O waiting to the operating system

