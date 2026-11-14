---
theme: default
title: Future Evening
info: |
  ## Future Evening
  DSA Grovesmühle 2026
colorSchema: light
fonts:
  sans: PT Serif
  provider: none
transition: slide-left
class: text-center text-xl
layout: center
---

<v-clicks>

# Future Evening
How do I make decisions about my future?

</v-clicks>

---
layout: two-cols
---

::left::

<v-click at="1">

# Engineering Problems

</v-click>

<v-click at="3">

have little uncertainty about the goal:  
Roughly every problem you've solved so far.

</v-click>

<v-clicks at="4">

- have a clear goal
- look at what other people do
- do exactly that

</v-clicks>

::right::

<v-click at="2">

# Design Problems

</v-click>

<v-clicks at="7">

have a lot of uncertainty about the goal.

So how do you approach something like that?

</v-clicks>

<v-clicks depth="2" at="9">

- find out where (in our case: _who_) you are
- see in which direction things look best:
  - look at things and test them
  - and then: work your way forward, step by step.

</v-clicks>

---

# Design Problem: Your Life

<v-clicks>

- Which activities do you enjoy? Which don't you?
- What are you good at? What can you learn?
- What matters to you? (Wealth? Family? Impact? Being kind? 'Being successful'?)

</v-clicks>

<v-clicks>

-> That gives you assumptions.

_And you can test them._

</v-clicks>

---

# Testing Assumptions

| Assumption | Test |
|---------|------|
| <span v-click>The job will be good for me</span> | <span v-click>Talk to someone who does it; do an internship</span> |
| <span v-click>The degree will be good</span> | <span v-click>Talk to someone who studies it</span> |
| <span v-click>I'll enjoy the subject matter</span> | <span v-click>Take a look at the material beforehand</span> |
| <span v-click>That's not possible, it's too expensive</span> | <span v-click>Talk to someone who has done it</span> |

<v-click>

-> from these tests, you build your plan.

</v-click>


---
layout: center
---

# Recap: How do I make decisions about my future?

<v-clicks>

- your future is a design problem
- and you can't engineer design problems
- (well, you can, it'll just potentially turn out badly)

</v-clicks>
<v-clicks>

So:

Plan it like a product designer: form a hypothesis, think about how you can test it, and test it.

</v-clicks>

---
layout: center
---

## "Well, that won't happen to _me_."

---
class: text-center
---

<div class="pie-grid">

<div v-click>

### I studied the way the recommended study plan intended

<PieChart
  compact
  :data="[
    { label: 'yes', value: 1 },
    { label: 'no', value: 11 },
  ]"
  :colors="['#c3c2b7', '#2a78d6']"
/>

</div>

<div v-click>

### I'm now doing exactly what I imagined doing before I started studying

<PieChart
  compact
  :data="[
    { label: 'yes', value: 3 },
    { label: 'sort of, but it’s quite a bit different from what I imagined', value: 3 },
    { label: 'I’m doing something else now', value: 4 },
    { label: 'I had no idea that what I do now even existed', value: 2 },
  ]"
  :colors="['#86b6ef', '#5598e7', '#256abf', '#104281']"
/>

</div>

<div v-click>

### I know very precisely where I'll be in five years

<PieChart
  compact
  :data="[
    { label: 'yes', value: 3 },
    { label: 'no', value: 9 },
  ]"
  :colors="['#c3c2b7', '#2a78d6']"
/>

</div>

</div>

<style>
.pie-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2rem;
  height: 100%;
}
.pie-grid h3 {
  font-size: 1.15rem;
  line-height: 1.3;
  height: 6rem;
  margin: 0 0 0.5rem;
}
</style>
