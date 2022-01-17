class: center
name: title
count: false

# Implementing languages for fun and profit

.me[.grey[*by* **Nicholas Matsakis**]]
.page-center[.hugest[😊]]
.citation[`https://github.com/nikomatsakis/plmw-2022`<br>Press `p` for the "soundtrack"!]

???

Hi! My name is Nicholas Matsakis.

Thanks to the PLMW folks for having me to talk to you today,
and thanks to you for listening.

---

# Me

.p25[.center[![On the internet](content/images/2019.03.26.jpg)]]

* Co-lead of the Rust language design team
* Senior Principal Engineer at AWS

???

Let me tell you a little bit about myself. My primary
claim to fame is that I've been heavily involved in the design
of Rust. I've been working on it since 2011, so for over a decade
at this point. I'm the co-lead of the Rust language design team
and heavily in the architecture and design of its compiler
and other things. I also joined AWS about a year ago as a 
Senior Principal Engineer, where my main duty is to work on Rust.

---

# Also me

.p25[.center[.vert_align_top[![MIT Yearbook](content/images/MIT-Yearbook.jpeg)]¹]]

* Before that, Rust at Mozilla for about 10 years
* PhD with Thomas Gross at ETH Zurich for 7 years
* 3 years at a startup called DataPower² building an XSLT Compiler

.small[¹ "You... went to MIT in 1974?" -- a friend, upon seeing this picture]<br>
.small[² Since acquired by IBM #humblebrag]

???

Before joining AWS, I did a few other things, but my entire
professional career has been spent building compilers and languages.
I credit that all to Saman Amarasinghe, a professor at MIT:
I took his course on compilers and just fell in love. I've never looked
back.

---

# My goal(s) with this talk

* Encourage you to build languages and compilers

???

My purpose here is to encourage you to build languages
and compilers too, at least if that's what you want to do.

(click)

But I'm also here for another reason: I want you to enjoy it.
What do I mean by that?


--

* Encourage you to **enjoy it**

---

# Enjoying it

* I'm having the most fun I've ever had.
* I want to share the way I'm working now.

???

I'm really having a lot of fun these days.
I've found ways to work that feel really good.
I want to spread the news.

(click)

I want to emphasize that I am not trying to tell you how
to develop your project, and especially not what technologies
to use. But I do hope that something in what I'm sharing
will be useful to you and shape the way you work.

--
* Will it work for you? I don't know!
    * But hopefully you'll find something useful.

---

# It wasn't always like this

* Not always easy
* I've struggled a lot emotionally 😔
* Both during PhD and after
* Most of us do

???

The fact is, I wasn't always having such a good time.
I've always struggled with feeling ineffective, inadequate, like
an imposter. I think a lot of people do, though we don't always
talk about it.

Honestly, it's only in the last year or so that I've started
to make progress on accepting myself and really feeling
my own worth as a human being, independent from the work that I
produce.

(click)

So, if you've struggled with those feelings, let me just
say, you're not alone. I can't tell you that it will get
better, but I can say that it *can* get better.

--

.center[.huge[🫂]]

---

# Dada

* Dada is an experimental programming language I've been working on
* I want to take you through the journey

Quick demo on the [Dada playground](https://dada-lang.org/playground/)

???

Anyway, enough mushy stuff. I'm going to be talking to you today
about a new programming language that I've been playing with
lately, that I call Dada, after the art movement. Dada is an experimental
language meant to combine the benefits of Rust with a feeling closer to 
Java or JavaScript. Unlike Rust, it doesn't aim to cover low-level, 
embedded applications. Really, Dada is an attempt to capture a hypothesis
I have, that people learn best by interactivity. But I'm getting a bit ahead
of myself.

In any case, my goal here today isn't really to talk about *Dada*, but rather
the process I've been using to develop it, and its compiler architecture, which
I'm (so far) fairly satisfied with.

Let me start by clicking on this playground link, just to give you a very brief
feel for Dada.

---

# Two basic lessons

After programming for 35 years (😱) here are two things I've learned:

* Loops, loops, and more loops
* Pick the right IR (Intermediate Representation)

???

Right, so there are two main lessons I want to impart. They derive from
programming but I kind of think they apply more broadly.

First, it's often said that programs spend most of their time in loops -- the same is true
of humans, so it's important to think about your loops.

Second, when you're programming, it's amazing what a difference the choice of IR
can make. I've often found that when something seems too hard, it's because I need
to introduce a primary transformation into some form that makes the problem
easier. Well, likewise, as we do research, we should think about the medium
and tools we are using and make sure they're a good match for our goals.

---

# Loops, loops, and more loops

* The key to everything is iterating.
* I learn this and relearn this.¹

.small[¹ See what I did there?]

???

OK, so let's talk about loops. The fact is that research
involves a ton of iteration. You have an initial idea, and
you refine it over and over again.

---

# The basic cycle

```rust
let mut hypothesis = make_hypothesis();
loop {
    test_hypothesis(&hypothesis);
    refine_hypothesis(&mut hypothesis);    
}
```

* Scientific method
* Military calls it the [OODA loop](https://en.wikipedia.org/wiki/OODA_loop)
* Startup people call it the "build-measure-learn loop"¹

.small[¹ I'm forced to admit I kind of like this one best.]

???

This is not, in fact, unique to academia. It's an idea that comes
up everywhere. My favorite expression, oddly, is from a book
about how to run a startup, which called it the "build-measure-learn loop".

The reason I love this so much is that it starts with *build*.
All too often, when I was going around this loop, I spent a lot of time
in my head. It made sense, I thought: I can imagine and think things through
much faster than I can build. And that's very true!

And yet, I cannot tell you how much more quickly research and evolution
happens once you can get things to some kind of prototype. It doesn't even have to be
a very good prototype! 

*Anything* you can do to get the research into a state
where people can play with it and imagine what it will be like to use it
will immediately spark tons of ideas for how to do it better.

There's a reason I started this talk with the Dada playground. I really, really
wanted to get Dada to a state where I can interactively play with the ideas,
so that's the very first thing I tried to build. I literally had the playground
up and going before I had the `+` operator working.

But even so, the playground is fairly late in the evolution of Dada. So let me
start there.

---

# Getting started with Dada

🔥 Spark:

> It took me weeks to understand what a variable was.
> <br>
> <br>
> .right-justify[-- Talented Java programmer learning Rust]

???

For me, the spark for Dada was a conversation I had with a talented Java programmer using Rust here at AWS. Although they were happily using Rust now, they had had a rough start,and I was trying to understand what made it difficult. Among various other things, they mentioned that the concept of local variables was really different in Rust from Java. My reaection to this statement was (click) yeah, it was an eye opener.

I thought I had a pretty good idea what they meant. It's kind of like how people say Java has no pointers when really almost every value in Java is a pointer. What they mean is that, in Java, there is no way to take a pointer *into your stack frame* and give it to someone else. You can share an object, but that's it.

--

.center[.huge[😯]]

---

# Step 1: Variations on Rust's type system

### Pick the right IR

Can't say enough good things about [PLT Redex](https://redex.racket-lang.org/)

???

As it happens, I had some ideas for how to tweak the Rust type system to make it feel more natural, both in this dimension and a few others. So I started playing with them.

At first I was just thinking things through in my head and sketching a bit on paper, but I realized quickly I needed a way to make the ideas more concrete. I couldn't keep enough of the state in my head.

---

# Diversion: reading the Programmer's Brain

.center[.p60[![programmers-brain](content/images/programmers-brain.png)]]

[read more at Felienne Hermans' page](https://www.felienne.com/book)

???

I've recently read the Programmer's Brain and quite enjoyed it. The book pretty clearly explains what's going on here -- your brain can only hold about 6 "things" in its working memory at a time, so having notation and tools lets you 'export' some of that. 

Similarly, the more compact a notation is -- the more precisely it captures the thing you're trying to work with -- the more you are able to lump a bunch of stuff into 1 "thing". Basically why *abstraction* is so effective at scaling programs up.

---

# PLT Redex ftw

For me, the best tool for sketching type systems is [PLT Redex]:

* Write your type rules roughly as you would in a paper.
* Run them on unit tests and visualize the output.

[PLT Redex]: https://redex.racket-lang.org/

???

Once I realized I needed a tool, I started using PLT Redex. It's a great way to sketch out rules and play with them. I made a lot of progress that way, but after a while, I realized that I had skipped a step.

---

# Step 2: Writing the tutorial

### Pick the right IR

![Tutorial opening page](./content/images/screenshot-tutorial-open.png)

???

My goal was to think about how it feels as a user, and sketching type rules didn't let me do that. So I backed up, and I started writing tutorials for a programming language that didn't exist. This was very instructive. 

Writing a tutorial forced me to answer questions like:

* Can I introduce the concepts I need in a linear order?
* How independent are they?

I went through a lot of iterations. As I worked, I realized that a key part of
the project was going to be the interface. I envisioned that I would have a web
interface where you could put the cursor and get a visualization of the permissions
and object structure.

---
name:tutorial

![Tutorial opening page](./content/images/screenshot-tutorial-perms.png)

???

I realized though that I could approximate the "feeling" of how the implementation
should work without actually building anything. 

---
template:tutorial

.tutorial-0[![tutorial](content/images/Arrow.png)]

---
template:tutorial

.tutorial-1[![tutorial](content/images/Arrow.png)]

[asciiflow.com](https://asciiflow.com/): best tool ever

---

# Shoutout: Pernos.co

.center[![Pernosco](content/images/pernosco.png)]

???

Totally unrelated, but I was very inspired by pernos.co, this nifty debugging
project. The idea is that you capture a trace of the code you are running and
you are able to step back and forth through time to explore what happened.
You can do things like click in the stdout to see the state of the program when
that byte was written. You can also do things like click in a buffer and jump
backwards to the point where that bufer was written, which might not be easy to find
otherwise. Very cool, totally check it out.

---

# Step 3: Feedback

.left-justify[.large[
    𝄆¹
    Me: 👀? 🙏 <br>
    Them: 🙆‍♀️² 👀 ⏳ <br>
    Them: 💬 <br>
    Me: 🤔 ✏️
    𝄇
]]

.small[¹ Emoji: modern heiroglyphics]<br>
.small[² Hint: this apparently means "OK". Maybe from a sport?]

???

* Armed with this tutorial, I started sending it to people for feedback.
* People would read it and ask me questions. "Makes sense, but what would this program do?"
* I adjusted the language in response to what they said, and as I did so, I would propagate the changes throughout the tutorial.
* This was like a unit test: sometimes, I was able to simplify the ordering, since having changed how one bit worked meant I could introduce a concept later. That was cool and indicated I was making progress.

---
name: hypotheses

# Dada hypotheses

Over time I evolved a few hypotheses for Dada:

---
template: hypotheses

### Live feedback is key

???

The biggest, and not one that I started with, was that the environment and the language were intertwined. 

I wanted to make a language with a strong type system, but where learning felt interactive.

---
template: hypotheses

### Concrete first, abstract later

???

I wanted to blend compile time and runtime. I wanted every program to be runnable, and I wanted you to be able to easily see the state at each point as it ran, especially for small programs.

I wanted you to start by observing runtime errors, and then add types, and see how those types prevent the runtime errors -- and I wanted the error messages from the type system to be presented *as if* they were runtime errors, so that they present counterexamples or other details. Realizing this is definitely still a work-in-progress. 

---
template: hypotheses

### Values, not places

???

To loop back to my original motivations, I wanted Dada to *feel* like a "normal GC language". I realized along the way that the best way to manage this was to model and implement it that way, and then to show that the semantics were equivalent when I made various systems-level optimization (like removing pointers).

---

# Step 3: Incremental system of my dreams

???

Eventually, I was no longer getting useful feedback on the tutorial. I felt the next step was to try it out. But I had a problem. I was actually doing two kinds of research in one 
here, because I didn't just want to play with the language, I wanted to experiment
on *how one builds a compiler*.

---

# Step 3: Incremental system of my dreams

.center[![Yak Shave](content/images/yak-shave.gif)]

???

That's right, it was time for a yak shave.

---
name: salsa-0

# Compilers a la Dragon Book

![Salsa-0](content/images/Salsa-0.drawio.svg)

.citation[Tool to make this diagram: [draw.io](https://draw.io). It's great.]

???

The traditional compiler is a "batch compiler". It proceeds in a very linear fashion.

(click) It starts by reading in the sources.

(click) The lexer then converts those bytes into tokens which are fed into the parser.

(click) The parser produces a data structure representing the program. For example, it might start with a module that has an entry for each top-level symbol. This is often called a 'symbol table'.

(click) For each symbol, like a function, we have a data structure with its details.

(click) Finally, to represent the body itself, we have an AST with the parsed representation of the text. This usually excludes comments and things.

---
template: salsa-0

.salsa-0-0[![Arrow](content/images/Arrow.png)]

---
template: salsa-0

.salsa-0-1[![Arrow](content/images/Arrow.png)]

---
template: salsa-0

.salsa-0-2[![Arrow](content/images/Arrow.png)]

---
template: salsa-0

.salsa-0-3[![Arrow](content/images/Arrow.png)]

---
template: salsa-0

.salsa-0-4[![Arrow](content/images/Arrow.png)]

---

# Compilers a la Dragon Book

![Salsa-1](content/images/Salsa-1.drawio.svg)

???

After parsing, the compiler goes through several phases:

* The type checker issues warnings and errors.
* The optimizer transforms the AST, possibly introducing new representations.
* The code generator creates the final executable.

---

# Responsive compilers

???

This dragon-book style architecture is exactly how the Rust compiler was written
initially. It's a tried and true strategy, and it works, but it's not well suited
to a modern coding environment. In an IDE, you want to be able to give live feedback.
You want to be able to do autocompletion. And, even in a batch compiler, you want to be
able to compile incrementally, so you're not starting from scratch every time.

---

# Responsive compilers

.center[.p25[
    ![github pic of eddyb](content/images/eddyb.png)
    ![github pic of michaelwoerister](content/images/mw.png)<br>
    ![github pic of matthewhammer](content/images/matthewhammer.png)
    ![github pic of matklad](content/images/matklad.png)
]]

???

At some point we decided to extend the Rust compiler with incremental compilation.
We wanted a scheme that would make it as easy to build an incremental, responsive
compiler as it was to build a dragon-book-style one. At this point, I want to give
a shout-out to these four people, who have tremendously impacted my thinking on this;
most every good idea that follows came from one of them.

---

# Working backwards

???

Let's talk through the goal. We want something that would get you live feedback as quickly as possible. If you were just testing one function, ideally, we would just compile that function plus the things it calls, and nothing else. We can do that in the background. You can see that there is no way to do that in the traditional dragon book architecture.

---
name: working-backwards

# Working backwards

![Salsa-2](content/images/Salsa-2.drawio.svg)

---
template: working-backwards

.wb-0-1[![Arrow](content/images/Arrow.png)]

???

So what we want is to start by saying "we need the code for the `main` function".

---
template: working-backwards

.wb-0-2[![Arrow](content/images/Arrow.png)]

???

That would in turn say "we need to type check `main`"

---
template: working-backwards

.wb-0-3[![Arrow](content/images/Arrow.png)]

???

Which would in turn say "we have to parse `main`".

---
template: working-backwards

.wb-0-4[![Arrow](content/images/Arrow.png)]

???

The parser would read the raw bytes...

---
template: working-backwards

.wb-0-5[![Arrow](content/images/Arrow.png)]

???

...and use them to allocate the AST for `main`.
Unlike before, though, we wouldn't create the AST for the other functions,
at least not yet.

---
template: working-backwards

.wb-0-6[![Arrow](content/images/Arrow.png)]

???

The type checker would return the function body.
If `main` calls the `helper` function, it would request just the
data that is needed to complete the type check -- the function signature,
for example.

Producing that would again consult the raw bytes.

---

# Incremental compilation

![Salsa-2](content/images/Salsa-2.drawio.svg)

???

In addition to doing the minimum work to produce a result,
this setup is also interesting because it tells you when work
needs to be recomputed.

For example, we can see that the parser needs to re-execute 
when the input changes. But if the parser produces the same
AST as it did before -- e.g., if they only added a comment --
then its *output* hasn't changed, which means that the type 
checker doesn't need to be re-executed.

---

# Compilers a la Salsa

.center[.p25[![Salsa Logo](content/images/salsa-logo.png)]]

* [Salsa]: a Rust package for building "working backwards" compilers
* You can [read about Salsa here][salsa] and the [new system here](https://hackmd.io/@nikomatsakis/BJ32_WZOF).
* If this interests you, join the [Salsa Zulip] and ping me (`nikomatsakis`)!

[salsa]: https://salsa-rs.github.io/salsa/
[salsa zulip]: https://salsa.zulipchat.com/

---

# Step 4: Implementing the compiler

* The last month or two I've been working on implementing Dada itself in Rust.

---

# Conclusions

Here are things I would recommend:

* [Don't trust anyone over 30](https://en.wikipedia.org/wiki/Jack_Weinberg) ✌️¹
* Build yourself a prototype
    * Something you can do in a few months
    * If that's not possible, cut corners until it is
* Whenever possible, share it
* Architect your compiler in an 'IDE-friendly' way
    * It's easier than you think, if you do it up front

.small[¹ Including me. Seriously, these are my ideas, and they work for me, but you should trust your instincts.]

???
