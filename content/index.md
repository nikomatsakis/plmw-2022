class: center
name: title
count: false

# Implementing languages for fun and profit

.me[.grey[*by* **Nicholas Matsakis**]]
.page-center[.hugest[😊]]
.citation[`https://github.com/nikomatsakis/plmw-2022`]

---

# Me

* Co-lead of the Rust language design team
* Formerly:
    * PhD student with Thomas Gross at ETH Zurich

---

# Me, continued

* Senior Principal Engineer at AWS
* Before that, Mozilla for about 10 years
* Before my PhD, worked at DataPower for several years building an XSLT compiler

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

# Struggles

* Not always easy
* I've struggled a lot emotionally
* Most of us do

---

# Dada

* Dada is an experimental programming language I've been working on
* I want to take you through the journey

---

# Iterate

* The key to everything is iterating.
* I learn this and relearn this.¹

.small[¹ See what I did there?]

---

# The basic cycle

* Scientific method
* Military calls it the [OODA loop](https://en.wikipedia.org/wiki/OODA_loop)
* Startups call it the lean-run cycle

```rust
let mut hypothesis = make_hypothesis();
loop {
    test_hypothesis(&hypothesis);
    refine_hypothesis(&mut hypothesis);    
}
```

---

# Outline

* Build something tangible (*)
* Use the right tool for the job
* Do unto others, as you would have them do unto you

.small[(*) I couldn't find a well-known quote for this one.]

---

# Build something tangible

* Where do ideas come from?
* My experience:
    * Feedback from others (synthesizing)
    * Dropping constraints (ah-ha!)

---
