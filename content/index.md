class: center
name: title
count: false

# Implementing languages for fun and profit

.me[.grey[*by* **Nicholas Matsakis**]]
.page-center[.hugest[😊]]
.citation[`https://github.com/nikomatsakis/plmw-2022`]

---

# Me

.p25[.center[![On the internet](content/images/2019.03.26.jpg)]]

* Co-lead of the Rust language design team
* Senior Principal Engineer at AWS

---

# Also me

.p25[.center[.vert_align_top[![MIT Yearbook](content/images/MIT-Yearbook.jpeg)]¹]]

* Before that, Rust at Mozilla for about 10 years
* PhD with Thomas Gross at ETH Zurich for 7 years
* 3 years at a startup called DataPower² building an XSLT Compiler

.small[¹ "You... went to MIT in 1974?" -- a friend, upon seeing this picture]<br>
.small[² Since acquired by IBM #humblebrag]

---

# My goal(s) with this talk

* Encourage you to build languages
--

* Encourage you to **enjoy it**

---

# Enjoying it

* I'm having the most fun I've ever had.
* I want to share the way I'm working now.

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

* Saw a therapist in PhD days, mostly we talked about me feeling ineffective
* Same issues have haunted me since
* Only in the last year do I feel I've started to make progress on accepting myself

--

.center[.huge[🫂]]

---

# Dada

* Dada is an experimental programming language I've been working on
* I want to take you through the journey

Quick demo on the [Dada playground](https://dada-lang.org/playground/)

---

# Two basic lessons

After programming for 35 years (😱) here are two things I've learned:

--

* Loops, loops, and more loops
--

* Pick the right IR (Intermediate Representation)

---

# Loops, loops, and more loops

* The key to everything is iterating.
* I learn this and relearn this.¹

.small[¹ See what I did there?]

---

# The basic cycle

* Scientific method
* Military calls it the [OODA loop](https://en.wikipedia.org/wiki/OODA_loop)
* Startup people call it the "build-measure-learn loop"¹

```rust
let mut hypothesis = make_hypothesis();
loop {
    test_hypothesis(&hypothesis);
    refine_hypothesis(&mut hypothesis);    
}
```

.small[¹ I'm forced to admit I kind of like this one best.]

---

# Core idea

* Get yourself to something tangible as quickly as you can
* Remember what you're trying to achieve; the route will change

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

Without an implementation, it was hard for me.

---
template:tutorial

.tutorial-0[![tutorial](content/images/Arrow.png)]

---
template:tutorial

.tutorial-1[![tutorial](content/images/Arrow.png)]

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

The biggest, and not one that I started with, was that the environment and the language were intertwined. I wanted to make a language with a strong type system, but one that felt interactive.

---
template: hypotheses

### Concrete first, abstract later

???

I wanted to blend compile time and runtime. I wanted to 

---
template: hypotheses

### Values, not places

???

xx

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

# Compilers a la Salsa



---

# Step 4: Implementing the compiler

---

