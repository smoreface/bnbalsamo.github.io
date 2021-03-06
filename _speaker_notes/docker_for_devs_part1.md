---
layout: page
---


The agenda for todays talk

Things you should know: bash, of VMs, networking and programs that communicate over networks

More knowledge of bash is handy for more complicated flags/args/etc,
but traversing the file system is pretty important for understanding
examples, when it comes to building images

We're going to learn:
- What Docker is
- What a container is
- How containers differ from VMs
- How VMs are containers coexist, and why

By the end of the talk you will have:
- Run a container
- Run a service within a container
- Used a container to help with development
- Containized and run a project

---

What is Docker?

this blurb is straight off the website

Kind of sales-pitchy, but gets the point across

Docker is a container "platform" - the technology and tools to build, run, and manage containers and container architectures.

---

What is a container?

Straight off the website again

- Not VMs
- isolated (somewhat) - more info coming up next
- small, fast, effecient
- **repeatability**
- Lend themselves very well to automated orchestration

---

Containers aren't VMs, VMs aren't containers

Mike Coleman, a presenter at Dockercon, came up with a great metaphor

House:Apartment::VM:Container

VMs package _everything_, they are houses. Independent, don't share resources, etc

---

Containers package as little as possible.

Share resources, but are safely isolated

Containers are like apartments.

You and your neighbors share pipes, but you don't want to know when they flush.

---

VMs and Containers are typically used in different situations

VMs == old guard, workhorses.

Still definitely good for certain applications, long term, _highly_ customized, whole "appliances"

Containers solve problems devs and ops were experiencing.

Turn code into a shippable unit

Highly scalable, informed by/informing move towards microservices

Orchestration as code. DevOps

---

Despite their differences, VMs and containers can (and probably must) live in harmony

The container-folk aren't coming for your VMs with torches and pitchforks

Often the most appropriate orchestrations involve VMs **and** containers

Sometimes the containers are even running on VMs [gasp]

But this isn't a talk about VMs, so lets get on with it

---

Time for some demos

Best way to learn is to get your hands dirty

Give people a minute to get setup

Encourage people to help those next to them having trouble

---

Time to run our first container.

Does that say root in the prompt? Yes.

Can potentially do something drastic, like delete bin. 
Demonstrates that **this isn't the host**

Can demonstrate re-running, to show how fast it is when
you don't have to download the image. Serious difference from VMs.

---

Lets dissect this command, it has a lot of information jammed in there


- **Interactive containers are not necessarily the norm**
    - we're going to run a service next

---

Downloaded image from Dockerhub

Dockerhub is like a package manager

Might better be described as something like pypi - if you're a Python person

NOTE: The security note here is a valid one, but definitely outside of the
scope of this talk, be sure to re-iterate to examine images occasionally
(especially unofficial images, and especially if they are going to be in
contact with sensitive data)

---

Lets run something more service-y.

Services run continuously, perform a function (either continously or on demand)

Ping makes an acceptable stand in for a fully automated service.

We could run this with just -t and no -i, but then ^C just disconnects us from the container, and doesn't kill the ping.

This behavior could be desirable or not, keep thoughts like this in mind when running your containers

---

Alpine is a small linux distribution

Good for distribution, in case of duplication.

Lots of docker images use Alpine.

Alpine doesn't ship with a lot of other stuff that has to be "turned off"

**Fun Fact**: If you're using play-with-docker you're on Alpine right now!

---

Often times you're running more than one container, or want your container to be out of the way, so getting comfortable with these commands to "check up" on the internals of your container is good.

This is also what logging from all containers looks like, by default.

In your own code remember to allow for logging to stdout

---

These commands are just the tip of the iceberg.

We don't really have the time to go through all the administrative docker functionality in this talk, just the bare bones.

Docker provides a ton of functionality for managing containers and images.

---

I find keeping a "tidy" docker workspace is helpful, you only notice
you've left a mess around when you need to debug something.

Note: People might be be familiar with the bashism in the final command,
it executes the command inside of $() and replaces that portion of the
command with the output

---

Pinging google worked as a good stand-in for a service, but lets try out the real deal now.

This is also the first service we'll set up to be able to interact with outside of container-land, via port mapping.

---

Again, keeping a relatively clean environment can pay dividends when something goes wrong.

Resetting the "state" of a supporting service is as simple as stopping one, starting another

---

But, how about a real world example to demonstrate these principals in action

(give brief code overview in github)

---

Time permitting pause here, if people want to setup the demo themselves.

Can take the time to explan python virtual environments, demo after activation $ python is 3.x, not 2.7

---

How do we move forward, now that we've got some code?

The unspeakable option: Don't test.

Option 1: Mocking.

more moving parts, more chances for a mishap

Can miss "gotchas" (like r.get() returning bytes, not strs)

Option 2 without Docker:
- emails
- resources
- permissions
- configuration
- whose in charge of this?
- Whats the default state?
- Is testing going to put the resource in a weird state?
- etc

---

Docker to the rescue!

Docker makes all of those go away (mostly)

Easy to standardize default state via images

Easy to "factory reset" - delete and make a new one

Everyone can be tweaking to their hearts content, not stepping on toes

---

Processes is applicable to lots of services

Dockerhub is a tremendous resource, _lots_ of projects

Guess what we're going to be demoing next?

---

Deeper understanding of Containers, in order to build our own

OOP Metaphor:
- Images:Containers::Classes:Objects
    - "blueprints for instances"
    - one:many
- Dockerfile::Constructor
- Extensability
    - overloading
    - subclasses

---

Dockerfiles define a series of commands, which produce layers

Layers go on one after the other, like lines in a setup script

Image the result of all of the layers stacked up

---

The FROM line: Where we're "inheriting" from
- All layers _except_ ENTRYPOINT, CMD
- This probably has a Dockerfile too
- "scratch" == empty image, beginning 

COPY == copy from **host to container**
- RUN cp src dst == copies within the container

CMD, probably runs the service.

When CMD exits, container stops

---

Lets fire this Dockerfile up

Need to make an environment

The environment is important, if the Docker engine is remote it **all** gets packaged up and shipped out


---

Now that we built an image, lets run a container

Woohoo! We built a container and can see our content!

---

Keep thinking with layers, "clobbering" things with mounts is cool in dev, but you want to be able to ship with static setup for prod, usually.

Good candidates
* webservers
* dynamically reloading WSGI servers
* Programs watching a directory
* etc

---

Fly! Be free! Build useful containers!

You definitely want to read the docs, their is some interesting and powerful functionality which requires a bit of study
- ENTRYPOINT, CMD
- ARG, ENV
- what can be done at run time vs build time

---

Write a Dockerfile

Build

Run

Tweak

---

Python example is slightly more concrete

Notice sometimes we have to jump through some hoops to install dependencies different ways

---

The talks may be a bit lengthy, but are totally worth it

Look up the Dockercon youtube playlists, watch anything that looks interesting

Pycon also has some great talks, if you're into Python

Don't be afraid to follow along

---
