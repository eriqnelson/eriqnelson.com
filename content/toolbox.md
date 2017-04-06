+++
title = "Toolbox"
weight = 20
draft = true

+++
I laid out an issue with a Ruby script that I tried to use in [this post](#tumblr-to-docker-to-hugo) and I’d like to talk more about my approach to these sorts of problems.

## This is a Class Of Problems

This is a *pattern* I keep running into
```
for Problem in Problems:
    with One Tool Try to solve this Problem
        Installing One Tool requires Other Tools
            Installing Other Tools causes Problems
            Append Problem to Problems
    Make Tea Instead
```

## The Lesson: Never Install Locally

I keep hoping that things will “just work” and this has almost never been true with these kinds of tools. When you walk towards the edge of Userland and into Development, there’s a steep cliff waiting for you. Package managers make this a softer descent into madness but you’re still falling and eventually you’ll find yourself face to face with the same pile of skulls. I don’t want to be down here with the skulls.

Local installations often carry invisible parameters that cannot be accounted for. A `.file` that didn’t get make it into version control, a library version that didn't make it into the dependency manager or just Windows character encoding nonsense. There are a thousand sorts of pain of this kind and almost all of them come down to one statement “Well it works on my machine!”

It does not work on my machine.

## A Container For Everything and Everything In A Container

While working on a [different project](owlsovernight.com) I’ve gotten into few habits that have saved me a tremendous amount of work and I try to carry them with me into new projects:

* I create Dockerfiles for every service I need to develop and deploy the site
* I check my Dockerfiles into [version control](https://github.com/eriqnelson/owlsovernight.com/tree/master/dockerfiles)
* I document build and deployment notes for the containers

I’ve already seen some pretty great benefits to this strategy. Chief among them is that I have a portable environment so work performed on any workstation is done with the same tooling. Everything runs on my machine because “my machine” is the _same_ machine everywhere I go.

**I want more open source projects to post their runtime environment.** If it ran on your machine, please give me a copy of your machine so I can run it.

## A General Purpose Toolkit

I can’t rely on everything having it’s environment posted with the code and I need a toolkit that can be rapidly adapted to fit these conditions. Whether it’s deploying the kind of data migration tools I’m attempting here or developing my own tools I think these could stand me in good stead. All the pieces are there in Docker and there’s some good precedent in approaches like the Anaconda Distribution. I’m going to dedicate some time to formalizing the approach and hopefully saving others some of the time I’ve sunk into setting up a containerized development environment.

### What does a toolkit look like?

There’s a [really great post](https://developers.redhat.com/blog/2016/02/24/10-things-to-avoid-in-docker-containers/) on the Red Hat dev blog calling out all of the best practices in container creation and how it works; mostly in production. In fact, there are a huge number of extremely well written examples of best practices in production. That’s not what I’m on about.

A lean container makes the most sense in production but in early development it can be more frustrating than is worthwhile. So many times I’ve tried to `$nano somefile` just to see update _just one_ ENV variable only to be rebuffed by a too lean environment. If your tools are getting in the way of productivity then it’s time to consider if that’s really a tool you want to use.

#### General Purpose
Overall the design of a development container should be broad. It does not attempt the laser sharp focus of a production container. It is instead happy to be the Swiss Army Knife. To be a practical tool for running applications that you may only ever run once. To be available for running Weird Ideas to check their viability.

In many ways it acts as a shield against the constant coral-reef growth of dependencies on your local machine and so it must be easier to use than your own machine. It should be the first tool you reach for.

#### Maybe Not For You:
These are not tools for working in an established ecosystem. Like all tools they are not Universal in their applicability to All Problems. They are designed for solving a particular class of problems that I’ve run into across the huge spread of languages, frameworks and applications out in the world.
If you work on a production system and you’re not developing with it then… maybe don’t do that? Read the next section.

#### It’s Not Production:
Really. Not at all. Not in any way should a development container be shipped. Paint the thing with a giant yellow and black striped band around it that just says “Not For Production”.

Realistically they should be built consistently with “dev” tags and naming conventions. If you use [semantic versioning](http://semver.org/#spec-item-4) keep any and all development containers below a version 1.

#### Batteries Included:
Borrowing liberally from the [Pythonic](https://www.python.org/dev/peps/pep-0206/) gospel, I believe that development containers should come ready to do meaningful work. They should include the tools you need to get started and make it easy for you to change your mind later.
A development container should be able to run pretty much anything in its language or framework with minimal configuration from the defaults. When selecting for a language ensure that the package manager for that language is included if there’s one available. If there’s no package manager for the language you want to use please return to your time machine. If you’re building from that point forward, target 80% of your use cases and write a development container version that meets those needs.

#### Isolation:
I think that containers should strive to be lean. Even early in development when you’re oscillating more broadly between approaches you will be served by some restraint. I don’t think that a development container must _always_ run only one process. Sometimes it makes sense to use it to run a single service. Sometimes it needs to be many things in order to achieve progress. This must be weighed against the priorities of your work. This is the other side of the Batteries Included scale. You must achieve the correct balance between Not Enough Features and Too Many Features.

#### Impermanence:
Like All Things Container, a development container should be ready to explode at any given time for any or no reason at all. You must let go of the container. It will be reborn. If you need to persist data between containers use volume mapping. It’s easier to manage rapidly moving data structures in the early stages and it helps to remember that _The Container Is Not For Storage_.

#### It Still Goes In Version Control
Everything goes in version control. If you’ve made a particularly useful container and you find yourself reusing it it’s nice to be able to clone it instead of rewriting it. Going to run a test more than once? Make a new container to run that test. Keep in mind that adding layers on top of an existing Dockerfile is extremely inexpensive in time and compute. Iterate away but always check in your changes.

If you can check _yourself_ into version control then maybe do that too. Then tell us how you did it.

### My Toolbox

These are the languages that I use often enough to want a container handy for testing something out.

Java
Python
Ruby
Javascript

Frameworks
