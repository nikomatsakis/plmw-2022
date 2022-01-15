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
.small[² Since acquired by IBM]

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
* I've struggled a lot emotionally
* Both during PhD and after
* Most of us do

???

* Saw a therapist in PhD days, mostly we talked about me feeling ineffective
* Same issues have haunted me since
* Only in the last year do I feel I've started to make progress on accepting myself

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
>
> .right-justify[-- Talented Java programmer learning Rust]

---

.page-center[.hugest[😊]]

???

That got me thinking...I felt like I had a pretty good idea what they meant.

In Rust, you have the ability to take a pointer into the stack, and other functions can modify the values of your local variables. That's just not possible in Java.

---

# Step 1: Variations on Rust's type system

### Pick the right IR

Can't say enough good things about [PLT Redex](https://redex.racket-lang.org/)

???

As it happens, I had some ideas for how to tweak the Rust type system to make it feel more natural, both in this dimension and a few others. I started playing with them.

Very quickly, I realized I needed a way to make the idea concrete. I spent a while hacking
in [PLT Redex]. 

[PLT Redex]: https://redex.racket-lang.org/

---

# PLT Redex

If you've not used it, [PLT Redex] is a fantastic tool.

You get to write type rules in roughly the same state as you would in a paper, and then run them. It's very interactive and all about visualization and seeing what happens.

---

# Step 2: Writing the tutorial

### Pick the right IR

![Tutorial opening page](./content/images/screenshot-tutorial-open.png)

???

As I worked on the type rules, though, I realized that I was starting a bit from the wrong end. And so I backed up and just started writing tutorials for a programming language that didn't exist. This was very instructive. 

Writing a tutorial forced me to answer questions like:

* Can I introduce the concepts I need in a linear order?
* How independent are they?

---

![Tutorial opening page](./content/images/screenshot-tutorial-perms.png)

???

I went through a lot of iterations. As I worked, I realized that a key part of
the project was going to be the interface. I envisioned that I would have a web
interface where you could put the cursor and get a visualization of the permissions
and object structure.

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

# Step 3: Incremental system of my dreams

???

At around this point, I realized it was time for me to try implementing the
compiler. But I had a problem. I was actually doing two kinds of research in one 
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

