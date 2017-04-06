+++
title = "Magical Flowers"
weight = 20
draft = true

+++
I’ve spent a huge number of hours in Minecraft doing any number of things from simple mining to building enormous factories that cause even the mightiest of servers to creak under the load of them.

{screenshot of factory}

Hosting heavily modded Minecraft has been a stellar hobby as I expand my understanding of automated systems and software.
As a server admin I’ve learned:

How to write and maintain containers
Managing host networking and compute resources
Resolving complex and poorly documented dependencies
Just how long you can stretch a single Java thread until it snaps.

While I think all of that is valuable experience in the practical realities of software deployment there’s a lesson in playing that wasn’t immediately apparent to me.

# It’s all about Magical Flowers.

## The Game
Minecraft, best explanation: http://minemum.com/what-is-minecraft


## The Mods

### Open Computers http://ocdoc.cil.li/
>OpenComputers is a mod that adds computers and robots into the game, which can be >programmed in Lua 5.3. It takes ideas from a couple of other mods such as ComputerCraft, >StevesCarts and Modular Powersuits to create something new and interesting.

### Botania http://botaniamod.net/
>Botania is a tech mod themed around natural magic. The main concept is to create magical >flowers and devices utilizing the power of the earth, in the form of Mana.

## The Point

Botania and Open Computers are not attempts at feature replication. Instead they are distinct approaches to a similar set of problems that the vanilla Minecraft player may encounter. I don’t think one is superior in some absolute, objective way.

Instead I believe that Botania represents better design. It is an extension of the metaphor that feels like an extension of the game. The movement through space and time remains important. Your skillfulness at navigating terrain, building new structures, gathering resources and manipulating the environment remain the primary methods used to achieve your goals. It builds on you as a user.


The extension of the Botania controls and methods into the broader Minecraft play environment makes it easier to design, test and execute many of its functions while keeping the player within the same context that they’re already fluent in. I don’t consider that fluency trivial. If you’re cracking open the Lexica Botania for the first time you’ve already mastered most of the basic commands you’ll need to use the mod.



In contrast the interface paradigm of Open Computers is closer in execution to the experience many of us have at the terminal level. It isn’t necessarily a more useful tool in Minecraft. It is a design that offers a great deal of flexibility at the cost of ease of use. For the uninitiated a blinking cursor on a black screen is an opaque and seemingly hostile interface method. For the initiated it can be an invitation to great power. Still, using this mod is a fundamental break in your conception of space, gameplay and if you’re not already comfortable with a scripting language;  a major break in your fluency. Instead of being an augmented player with new powers and a grasp of some new mechanics you are player in the grips of readmes and mysterious runtime errors.



This understanding extends to looking at tools that I’ve encountered in software development. While it’s a straightforward matter to learn git command line functions and it’s remarkably well documented and stable, I prefer to use the Github Desktop client. The primary reason for this isn’t a quality issue, it’s usability. I remain within the same context that I’ve been working in (a MacOS desktop environment) to make my commits. Rapidly switching between these contexts can cause

## The Lesson

Breaking a user’s paradigm is an expensive moment. Rapid paradigm switching is mentally fatiguing.  Once a user has developed fluency in the basics of the software interface we have a language in common to expand on. Do so. When other developers start to extend your interface they should have guidelines provided to them that explain the design language clearly and show how it can be extended.

Even better is if your language is refined enough that the extension is obvious.
