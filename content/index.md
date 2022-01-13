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

# Step 1: Variations on Rust's type system

### Pick the right IR

Can't say enough good things about [PLT Redex](https://redex.racket-lang.org/)

---

# Step 2: Writing the tutorial

### Pick the right IR

---

# Step 3: Incremental system of my dreams

### Pick the right IR

---

# Step 4: Implementing the compiler

---

