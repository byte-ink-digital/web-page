---
title: Reactive systems architecture
date: 2021-02-02 00:00:00
description: There are two ways of constructing a software design. One way is to make it so simple that there are obviously no deficiencies, and the other way is to make it so complicated that there are no obvious deficiencies. The first method is far more difficult. - "C.A.R. Hoare"
featured_image: /assets/img/blog/complex_systems_architecture.jpeg
author: Mate
sitemap:
  lastmod: 2021-02-02 00:00:00
  priority: 0.4
---
<div class="horizontal-line"></div>

####  The origin
Last few decades demand for online services surges, everything goes online - from gaming, online shopping to remote working. Revolution of smartphones, rise of IoT and promised opportunities from Big Data puts a lot of demand on new software systems, but also on upgrading and improving existing ones.
{: style="text-align: justify;"}

One way to address huge demand and rise of software systems complexity is to use Reactive systems architecture. Reactive systems is an architectural style that enables applications composed of multiple microservices working together as a single unit to better react to their surroundings and one another, manifesting in greater elasticity when dealing with changing workload demands and resiliency when components fail.
{: style="text-align: justify;"}

This kind of architecture was first described 2013 by so called [Reactive Manifesto](https://reactivemanifesto.org) which purpose was to sublime all the knowledge that has been accumulated in IT industry in designing and building highly reliable and scalable applications.
{: style="text-align: justify;"}

Reactive manifesto describes following main traits of such systems (see image below):
{: style="text-align: justify;"}

<img src="{{ '/assets/img/blog/complex_systems_architecture_1.png' | relative_url }}" width="100%">

**Responsive:** The system responds in a timely manner if at all possible. Responsiveness is the cornerstone of usability and utility, but more than that, responsiveness means that problems may be detected quickly and dealt with effectively. Responsive systems focus on providing rapid and consistent response times, establishing reliable upper bounds so they deliver a consistent quality of service. This consistent behaviour in turn simplifies error handling, builds end user confidence, and encourages further interaction.
{: style="text-align: justify;"}
**Resilient:** The system stays responsive in the face of failure. This applies not only to highly-available, mission-critical systems — any system that is not resilient will be unresponsive after a failure. Resilience is achieved by replication, containment, isolation and delegation. Failures are contained within each component, isolating components from each other and thereby ensuring that parts of the system can fail and recover without compromising the system as a whole. Recovery of each component is delegated to another (external) component and high-availability is ensured by replication where necessary. The client of a component is not burdened with handling its failures.
{: style="text-align: justify;"}
**Elastic:** The system stays responsive under varying workload. Reactive Systems can react to changes in the input rate by increasing or decreasing the resources allocated to service these inputs. This implies designs that have no contention points or central bottlenecks, resulting in the ability to shard or replicate components and distribute inputs among them. Reactive Systems support predictive, as well as Reactive, scaling algorithms by providing relevant live performance measures. They achieve elasticity in a cost-effective way on commodity hardware and software platforms.
{: style="text-align: justify;"}
**Message Driven:** Reactive Systems rely on asynchronous message-passing to establish a boundary between components that ensures loose coupling, isolation and location transparency. This boundary also provides the means to delegate failures as messages. Employing explicit message-passing enables load management, elasticity, and flow control by shaping and monitoring the message queues in the system and applying back-pressure when necessary. Location transparent messaging as a means of communication makes it possible for the management of failure to work with the same constructs and semantics across a cluster or within a single host. Non-blocking communication allows recipients to only consume resources while active, leading to less system overhead.
{: style="text-align: justify;"}


####  Reactive everywhere
Today we have many tools and frameworks claiming to be Reactive. One can find reactive programing tools (e.g. ReactiveX), reactive streaming implementation tools etc, but what we are focused here is reactive architecture not the library or specification.
{: style="text-align: justify;"}
**Reactive programming**<br>
Reactive  programming is a useful implementation technique for managing internal logic and dataflow transformation locally within components, through asynchronous and non-blocking  execution. But these kind of programs are still focused on single node/process and do not offer all traits of reactive sytems such as elacitcity.
{: style="text-align: justify;"}
**Reactive streaming**<br>
Other often reffered reactive programming style is reactive streaming. It is actually a specification designed to provide a standard for asynchronous stream processing with nonblocking  back  pressure i.e. flow control.
{: style="text-align: justify;"}
**Reactive architecture**<br>
So when we talk about reactive system architecture we are looking at architecture of whole solution e.g. multiple nodes not just programing style or single process implementation.
{: style="text-align: justify;"}

#### Reactive system architecture
How to achieve reactive architecture? First we need to understand how to get most out of available hardware. First option is to use concurrency and parallelism. In short concurrency enables us to run multiple interleaving tasks using single CPU unit (e.g. context switching) while with parallelism we can run multiple computations in parallel on multiple CPU units.
So the application that is part of reactive system has to be able to achieve better parallelism by adding more “workers” on the job at once, and keeping them all busy (concurrency) to get the job done faster and more efficiently. There are many implementations how to handle concurrency and parallelism.
{: style="text-align: justify;"}

##### Multithreading
One way to achieve a concurrency and parallelism is to use multithreading where one or more tasks are executed within their own thread. This is standard approach e.g. on JVM based systems event today by frameworks leveraging Servlet API. But multithreading has some disadvantages:
* Very hard to implement correctly
* Very costly (e.g. each thread has some significant memory footprint) which reduces number of task that can be executed by system
* Location dependency – only threads from local process can be used for computation
{: style="text-align: justify;"}

##### Reactor pattern
Other programming platforms such us node.js address concurrency and parallelism using different parading called Reactor Pattern which is basically event loop described with following image
{: style="text-align: justify;"}
<img src="{{ '/assets/img/blog/complex_systems_architecture_2.png' | relative_url }}" width="100%">

##### The Actor Model to the rescue
The actor model is programming model ideal for designing distributed systems that have all the traits of reactive system architecture. Actor model that was originally used in telecom applications (see Erlang language and platform) and it is becoming more and more used in today’s modern multi-threaded systems running on multi-CPU architectures.
{: style="text-align: justify;"}
The actor model describes how system components should behave and interact with each other. It is based on single computation unit called Actor. Whole system is constructed from hierarchy of Actors that interact between themselves by passing messages.
{: style="text-align: justify;"}
Actors are completely isolated, so no shared memory is involved - this way concurrency and parallelism is guaranteed to be without fallacies that we have seen in “classic” multithreaded applications. Additionally, isolation and message passing enables location transparency i.e. actors can be run in parallel in different processes, even on different machines.
{: style="text-align: justify;"}

Every actor has:
* its own state (no shared memory is used),
* behavior – by processing message actor can execute some login
* mailbox – abstraction for queuing incoming messages
* child actors – actors form tree like structure. Since Actor has small memory footprint it is easy, even advised, to divide task to smaller pieces and delegate child actor to deal with it
{: style="text-align: justify;"}

Simplified example of actor system is depicted with image below:
<img src="{{ '/assets/img/blog/complex_systems_architecture_3.png' | relative_url }}" width="100%">


#### Akka – JVM based actor model
[Akka](https://akka.io) is set of libraries help us implement and use actor model using JVM based languages (Java/Scala). Besides, actor model programming, Akka also have few interesting projects:
* akka streams reactive streams implementation
* akka http – REST api library
{: style="text-align: justify;"}

Akka’s actor model is implemented around Dispatcher abstraction that is main component that makes actor model run. Dispatcher receives available threads and allocates a thread per actor per a message (default behavior). So each thread is reused by many actors and thread is allocated only when message needs to be processed. Image below describes dispatcher functionality:
<br>
<img src="{{ '/assets/img/blog/complex_systems_architecture_4.png' | relative_url }}" width="100%">
<br>

Actors are organized into actor system: hierarchical structure where each actor has parent and child actors.
{: style="text-align: justify;"}

One important aspect of actor model is so-called Location transparency meaning that actors can run and communicate inside same process, in different processes on same machine and even on remote machines transparently. This is available due to high level of isolation of each actor and messages can be sent remotely over network. Following image depicts actor system that is distributed between different nodes over network.
<br><br>
<img src="{{ '/assets/img/blog/complex_systems_architecture_5.png' | relative_url }}" width="100%">
{: style="text-align: justify;"}

Akka as a set of libraries supports many interesting features such as:
{: style="text-align: justify;"}
* Remoting – actor systems can run transparently on different machines
* Routing – used for scaling up – reusing multiple CPU cores
* Clustering – native clustering of many actor systems into one cluster
    * Cluster singleton – sometimes it is convenient to run only one type actor in whole cluster
    * Cluster sharding – akka can natively partition (shard) your actors across multiple nodes
* Event sourcing (CQRS pattern) – native support for event sourcing backed by different event stores (e.g. Cassandra, RDBMS…)
* Streaming – support for reactive streaming
{: style="text-align: justify;"}

<br>
All in all Akka toolkit provide many interesting features to build robust and distributed systems capable of handling high number of requests from client applications.
{: style="text-align: justify;"}

So, reflecting to quote at the very beginning of this blog post, we can use Akka to build reactive systems that are capable of handling complex tasks by splitting its complexity to may distributed units of computation that are simple to design and reason about.
{: style="text-align: justify;"}
<div class="horizontal-line"></div>