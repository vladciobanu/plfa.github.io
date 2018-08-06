---
title     : "Induction: Proof by Induction"
layout    : page
permalink : /Induction/
---

<pre class="Agda">{% raw %}<a id="108" class="Keyword">module</a> <a id="115" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}" class="Module">plfa.Induction</a> <a id="130" class="Keyword">where</a>{% endraw %}</pre>

> Induction makes you feel guilty for getting something out of nothing
> ... but it is one of the greatest ideas of civilization.
> -- Herbert Wilf

Now that we've defined the naturals and operations upon them, our next
step is to learn how to prove properties that they satisfy.  As hinted
by their name, properties of _inductive datatypes_ are proved by
_induction_.


## Imports

We require equivality as in the previous chapter, plus the naturals
and some operations upon them.  We also import a couple of new operations,
`cong`, `sym`, and `_≡⟨_⟩_`, which are explained below.
<pre class="Agda">{% raw %}<a id="743" class="Keyword">import</a> <a id="750" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html" class="Module">Relation.Binary.PropositionalEquality</a> <a id="788" class="Symbol">as</a> <a id="791" class="Module">Eq</a>
<a id="794" class="Keyword">open</a> <a id="799" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html" class="Module">Eq</a> <a id="802" class="Keyword">using</a> <a id="808" class="Symbol">(</a><a id="809" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">_≡_</a><a id="812" class="Symbol">;</a> <a id="814" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a><a id="818" class="Symbol">;</a> <a id="820" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#1075" class="Function">cong</a><a id="824" class="Symbol">;</a> <a id="826" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.Core.html#560" class="Function">sym</a><a id="829" class="Symbol">)</a>
<a id="831" class="Keyword">open</a> <a id="836" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#3861" class="Module">Eq.≡-Reasoning</a> <a id="851" class="Keyword">using</a> <a id="857" class="Symbol">(</a><a id="858" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#3962" class="Function Operator">begin_</a><a id="864" class="Symbol">;</a> <a id="866" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">_≡⟨⟩_</a><a id="871" class="Symbol">;</a> <a id="873" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4079" class="Function Operator">_≡⟨_⟩_</a><a id="879" class="Symbol">;</a> <a id="881" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4260" class="Function Operator">_∎</a><a id="883" class="Symbol">)</a>
<a id="885" class="Keyword">open</a> <a id="890" class="Keyword">import</a> <a id="897" href="https://agda.github.io/agda-stdlib/Data.Nat.html" class="Module">Data.Nat</a> <a id="906" class="Keyword">using</a> <a id="912" class="Symbol">(</a><a id="913" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="914" class="Symbol">;</a> <a id="916" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a><a id="920" class="Symbol">;</a> <a id="922" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a><a id="925" class="Symbol">;</a> <a id="927" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">_+_</a><a id="930" class="Symbol">;</a> <a id="932" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#433" class="Primitive Operator">_*_</a><a id="935" class="Symbol">;</a> <a id="937" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#320" class="Primitive Operator">_∸_</a><a id="940" class="Symbol">)</a>{% endraw %}</pre>


## Properties of operators

Operators pop up all the time, and mathematicians have agreed
on names for some of the most common properties.

* _Identity_ Operator `+` has left identity `0` if `0 + n ≡ n`, and
  right identity `0` if `n + 0 ≡ n`, for all `n`. A value that is both
  a left and right identity is just called an identity. Identity is also
  sometimes called _unit_.

* _Associativity_ Operator `+` is associative if the location
  of parentheses does not matter: `(m + n) + p ≡ m + (n + p)`,
  for all `m`, `n`, and `p`.

* _Commutatitivity_ Operator `+` is commutative if order or
  arguments does not matter: `m + n ≡ n + m`, for all `m` and `n`.

* _Distributivity_ Operator `*` distributes over operator `+` from the
  left if `(m + n) * p ≡ (m * p) + (n * p)`, for all `m`, `n`, and `p`,
  and from the right if `m * (p + q) ≡ (m * p) + (m * q)`, for all `m`,
  `p`, and `q`.

Addition has identity `0` and multiplication has identity `1`;
addition and multiplication are both associative and commutative;
and multiplications distributes over addition.

If you ever bump into an operator at a party, you now know how
to make small talk, by asking whether it is has a unit and is
associative or commutative.  If you bump into two operators, you
might ask them if one distributes over the other.

Less frivolously, if you ever bump into an operator while reading a
technical paper, this gives you a way to orient yourself, by checking
whether or not it has an identity, is associative or commutative, or
distributes over another operator.  A careful author will ofter call
out these properties---or their lack---for instance by pointing out
that a newly introduced operator is associative but not commutative.

#### Exercise (`operators`)

Give another example of a pair of operators that have an identity
and are associative, commutative, and distribute over one another.

Give an example of an operator that has an identity and is
associative but is not commutative.


## Associativity

One property of addition is that it is _associative_, that is, that the
location of the parentheses does not matter:

    (m + n) + p ≡ m + (n + p)

Here `m`, `n`, and `p` are variables that range over all natural numbers.

We can test the proposition by choosing specific numbers for the three
variables.
<pre class="Agda">{% raw %}<a id="3279" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#3279" class="Function">_</a> <a id="3281" class="Symbol">:</a> <a id="3283" class="Symbol">(</a><a id="3284" class="Number">3</a> <a id="3286" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="3288" class="Number">4</a><a id="3289" class="Symbol">)</a> <a id="3291" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="3293" class="Number">5</a> <a id="3295" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">≡</a> <a id="3297" class="Number">3</a> <a id="3299" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="3301" class="Symbol">(</a><a id="3302" class="Number">4</a> <a id="3304" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="3306" class="Number">5</a><a id="3307" class="Symbol">)</a>
<a id="3309" class="Symbol">_</a> <a id="3311" class="Symbol">=</a>
  <a id="3315" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#3962" class="Function Operator">begin</a>
    <a id="3325" class="Symbol">(</a><a id="3326" class="Number">3</a> <a id="3328" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="3330" class="Number">4</a><a id="3331" class="Symbol">)</a> <a id="3333" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="3335" class="Number">5</a>
  <a id="3339" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="3347" class="Number">7</a> <a id="3349" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="3351" class="Number">5</a>
  <a id="3355" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="3363" class="Number">12</a>
  <a id="3368" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="3376" class="Number">3</a> <a id="3378" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="3380" class="Number">9</a>
  <a id="3384" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="3392" class="Number">3</a> <a id="3394" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="3396" class="Symbol">(</a><a id="3397" class="Number">4</a> <a id="3399" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="3401" class="Number">5</a><a id="3402" class="Symbol">)</a>
  <a id="3406" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4260" class="Function Operator">∎</a>{% endraw %}</pre>
Here we have displayed the computation as a chain of equations,
one term to a line.  It is often easiest to read such chains from the top down
until one reaches the simplest term (in this case, `12`), and
then from the bottom up until one reaches the same term.

The test reveals that associativity is perhaps not as obvious as first
it appears.  Why should `7 + 5` be the same as `3 + 9`?  We might want
to gather more evidence, testing the proposition by choosing other
numbers.  But---since there are an infinite number of
naturals---testing can never be complete.  Is there any way we can be
sure that associativity holds for _all_ the natural numbers?

The answer is yes! We can prove a property holds for all naturals using
_proof by induction_.


## Proof by induction

Recall the definition of natural numbers consists of a _base case_
which tells us that `zero` is a natural, and an _inductive case_
which tells us that if `m` is a natural then `suc m` is also a natural.

Proof by induction follows the structure of this definition.  To prove
a property of natural numbers by induction, we need prove two cases.
First is the _base case_, where we show the property holds for `zero`.
Second is the _inductive case_, where we assume the the property holds for
an arbitrary natural `m` (we call this the _inductive hypothesis_), and
then show that the property must also hold for `suc m`.

If we write `P m` for a property of `m`, then what we need to
demonstrate are the following two inference rules:

    ------
    P zero

    P m
    ---------
    P (suc m)

Let's unpack these rules.  The first rule is the base case, and
requires we show that property `P` holds for `zero`.  The second rule
is the inductive case, and requires we show that if we assume the
inductive hypothesis---namely that `P` holds for `m`---then it follows that
`P` also holds for `suc m`.

Why does this work?  Again, it can be explained by a creation story.
To start with, we know no properties.

    -- in the beginning, no properties are known

Now, we apply the two rules to all the properties we know about.  The
base case tells us that `P zero` holds, so we add it to the set of
known properties.  The inductive case tells us that if `P m` holds (on
the day before today) then `P (suc m)` also holds (today).  We didn't
know about any properties before today, so the inductive case doesn't
apply.

    -- on the first day, one property is known
    P zero

Then we repeat the process, so on the next day we know about all the
properties from the day before, plus any properties added by the rules.
The base case tells us that `P zero` holds, but we already
knew that. But now the inductive case tells us that since `P zero`
held yesterday, then `P (suc zero)` holds today.

    -- on the second day, two properties are known
    P zero
    P (suc zero)

And we repeat the process again. Now the inductive case
tells us that since `P zero` and `P (suc zero)` both hold, then
`P (suc zero)` and `P (suc (suc zero))` also hold. We already knew about
the first of these, but the second is new.

    -- on the third day, three properties are known
    P zero
    P (suc zero)
    P (suc (suc zero))

You've got the hang of it by now.

    -- on the fourth day, four properties are known
    P zero
    P (suc zero)
    P (suc (suc zero))
    P (suc (suc (suc zero)))

The process continues.  On the _n_'th day there will be _n_ distinct
properties that hold.  The property of every natural number will appear
on some given day.  In particular, the property `P n` first appears on
day _n+1_.


## Our first proof: associativity

To prove associativity, we take `P m` to be the property

    (m + n) + p ≡ m + (n + p)

Here `n` and `p` are arbitrary natural numbers, so if we can show the
equation holds for all `m` it will also hold for all `n` and `p`.
The appropriate instances of the inference rules are:

    -------------------------------
    (zero + n) + p ≡ zero + (n + p)

    (m + n) + p ≡ m + (n + p)
    ---------------------------------
    (suc m + n) + p ≡ suc m + (n + p)

If we can demonstrate both of these, then associativity of addition
follows by induction.

Here is the proposition's statement and proof.
<pre class="Agda">{% raw %}<a id="+-assoc"></a><a id="7645" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7645" class="Function">+-assoc</a> <a id="7653" class="Symbol">:</a> <a id="7655" class="Symbol">∀</a> <a id="7657" class="Symbol">(</a><a id="7658" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7658" class="Bound">m</a> <a id="7660" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7660" class="Bound">n</a> <a id="7662" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7662" class="Bound">p</a> <a id="7664" class="Symbol">:</a> <a id="7666" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="7667" class="Symbol">)</a> <a id="7669" class="Symbol">→</a> <a id="7671" class="Symbol">(</a><a id="7672" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7658" class="Bound">m</a> <a id="7674" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7676" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7660" class="Bound">n</a><a id="7677" class="Symbol">)</a> <a id="7679" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7681" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7662" class="Bound">p</a> <a id="7683" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">≡</a> <a id="7685" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7658" class="Bound">m</a> <a id="7687" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7689" class="Symbol">(</a><a id="7690" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7660" class="Bound">n</a> <a id="7692" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7694" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7662" class="Bound">p</a><a id="7695" class="Symbol">)</a>
<a id="7697" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7645" class="Function">+-assoc</a> <a id="7705" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="7710" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7710" class="Bound">n</a> <a id="7712" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7712" class="Bound">p</a> <a id="7714" class="Symbol">=</a>
  <a id="7718" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#3962" class="Function Operator">begin</a>
    <a id="7728" class="Symbol">(</a><a id="7729" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="7734" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7736" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7710" class="Bound">n</a><a id="7737" class="Symbol">)</a> <a id="7739" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7741" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7712" class="Bound">p</a>
  <a id="7745" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="7753" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7710" class="Bound">n</a> <a id="7755" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7757" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7712" class="Bound">p</a>
  <a id="7761" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
   <a id="7768" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="7773" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7775" class="Symbol">(</a><a id="7776" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7710" class="Bound">n</a> <a id="7778" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7780" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7712" class="Bound">p</a><a id="7781" class="Symbol">)</a>
  <a id="7785" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4260" class="Function Operator">∎</a>
<a id="7787" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7645" class="Function">+-assoc</a> <a id="7795" class="Symbol">(</a><a id="7796" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="7800" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7800" class="Bound">m</a><a id="7801" class="Symbol">)</a> <a id="7803" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7803" class="Bound">n</a> <a id="7805" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7805" class="Bound">p</a> <a id="7807" class="Symbol">=</a>
  <a id="7811" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#3962" class="Function Operator">begin</a>
    <a id="7821" class="Symbol">(</a><a id="7822" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="7826" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7800" class="Bound">m</a> <a id="7828" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7830" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7803" class="Bound">n</a><a id="7831" class="Symbol">)</a> <a id="7833" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7835" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7805" class="Bound">p</a>
  <a id="7839" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="7847" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="7851" class="Symbol">(</a><a id="7852" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7800" class="Bound">m</a> <a id="7854" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7856" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7803" class="Bound">n</a><a id="7857" class="Symbol">)</a> <a id="7859" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7861" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7805" class="Bound">p</a>
  <a id="7865" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="7873" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="7877" class="Symbol">((</a><a id="7879" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7800" class="Bound">m</a> <a id="7881" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7883" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7803" class="Bound">n</a><a id="7884" class="Symbol">)</a> <a id="7886" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7888" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7805" class="Bound">p</a><a id="7889" class="Symbol">)</a>
  <a id="7893" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4079" class="Function Operator">≡⟨</a> <a id="7896" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#1075" class="Function">cong</a> <a id="7901" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="7905" class="Symbol">(</a><a id="7906" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7645" class="Function">+-assoc</a> <a id="7914" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7800" class="Bound">m</a> <a id="7916" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7803" class="Bound">n</a> <a id="7918" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7805" class="Bound">p</a><a id="7919" class="Symbol">)</a> <a id="7921" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4079" class="Function Operator">⟩</a>
    <a id="7927" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="7931" class="Symbol">(</a><a id="7932" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7800" class="Bound">m</a> <a id="7934" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7936" class="Symbol">(</a><a id="7937" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7803" class="Bound">n</a> <a id="7939" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7941" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7805" class="Bound">p</a><a id="7942" class="Symbol">))</a>
  <a id="7947" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="7955" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="7959" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7800" class="Bound">m</a> <a id="7961" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7963" class="Symbol">(</a><a id="7964" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7803" class="Bound">n</a> <a id="7966" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="7968" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#7805" class="Bound">p</a><a id="7969" class="Symbol">)</a>
  <a id="7973" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4260" class="Function Operator">∎</a>{% endraw %}</pre>
We have named the proof `+-assoc`.  In Agda, identifiers can consist of
any sequence of characters not including spaces or the characters `@.(){};_`.

Let's unpack this code.  The signature states that we are
defining the identifier `+-assoc` which provide evidence for the
proposition:

    ∀ (m n p : ℕ) → (m + n) + p ≡ m + (n + p)

The upside down A is pronounced "for all", and the proposition
asserts that for all natural numbers `m`, `n`, and `p` that
the equation `(m + n) + p ≡ m + (n + p)` holds.  Evidence for the proposition
is a function that accepts three natural numbers, binds them to `m`, `n`, and `p`,
and returns evidence for the corresponding instance of the equation.

For the base case, we must show:

    (zero + n) + p ≡ zero + (n + p)

Simplifying both sides with the base case of addition yields the equation:

    n + p ≡ n + p

This holds trivially.  Reading the chain of equations in the base case of the proof,
the top and bottom of the chain match the two sides of the equation to
be shown, and reading down from the top and up from the bottom takes us to
`n + p` in the middle.  No justification other than simplification is required.

For the inductive case, we must show:

    (suc m + n) + p ≡ suc m + (n + p)

Simplifying both sides with the inductive case of addition yields the equation:

    suc ((m + n) + p) ≡ suc (m + (n + p))

This in turn follows by prefacing `suc` to both sides of the induction
hypothesis:

    (m + n) + p ≡ m + (n + p)

Reading the chain of equations in the inductive case of the proof, the
top and bottom of the chain match the two sides of the equation to be
shown, and reading down from the top and up from the bottom takes us
to the simplified equation above. The remaining equation, does not follow
from simplification alone, so we use an additional operator for chain
reasoning, `_≡⟨_⟩_`, where a justification for the equation appears
within angle brackets.  The justification given is:

    ⟨ cong suc (+-assoc m n p) ⟩

Here, the recursive invocation `+-assoc m n p` has as its type the
induction hypothesis, and `cong suc` prefaces `suc` to each side to
yield the needed equation.

A relation is said to be a _congruence_ for a given function if it is
preserved by applying that function.  If `e` is evidence that `x ≡ y`,
then `cong f e` is evidence that `f x ≡ f y`, for any function `f`.

Here the inductive hypothesis is not assumed, but instead proved by a
recursive invocation of the function we are defining, `+-assoc m n p`.
As with addition, this is well founded because associativity of
larger numbers is proved in terms of associativity of smaller numbers.
In this case, `assoc (suc m) n p` is proved using `assoc m n p`.
The correspondence between proof by induction and definition by
recursion is one of the most appealing aspects of Agda.


## Our second proof: commutativity

Another important property of addition is that it is _commutative_, that is,
that the order of the operands does not matter:

    m + n ≡ n + m

The proof requires that we first demonstrate two lemmas.

### The first lemma

The base case of the definition of addition states that zero
is a left-identity:

    zero + n ≡ n

Our first lemma states that zero is also a right-identity:

    m + zero ≡ m

Here is the lemma's statement and proof.
<pre class="Agda">{% raw %}<a id="+-identityʳ"></a><a id="11307" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#11307" class="Function">+-identityʳ</a> <a id="11319" class="Symbol">:</a> <a id="11321" class="Symbol">∀</a> <a id="11323" class="Symbol">(</a><a id="11324" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#11324" class="Bound">m</a> <a id="11326" class="Symbol">:</a> <a id="11328" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="11329" class="Symbol">)</a> <a id="11331" class="Symbol">→</a> <a id="11333" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#11324" class="Bound">m</a> <a id="11335" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="11337" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="11342" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">≡</a> <a id="11344" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#11324" class="Bound">m</a>
<a id="11346" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#11307" class="Function">+-identityʳ</a> <a id="11358" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="11363" class="Symbol">=</a>
  <a id="11367" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#3962" class="Function Operator">begin</a>
    <a id="11377" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="11382" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="11384" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>
  <a id="11391" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="11399" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>
  <a id="11406" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4260" class="Function Operator">∎</a>
<a id="11408" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#11307" class="Function">+-identityʳ</a> <a id="11420" class="Symbol">(</a><a id="11421" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="11425" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#11425" class="Bound">m</a><a id="11426" class="Symbol">)</a> <a id="11428" class="Symbol">=</a>
  <a id="11432" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#3962" class="Function Operator">begin</a>
    <a id="11442" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="11446" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#11425" class="Bound">m</a> <a id="11448" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="11450" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>
  <a id="11457" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="11465" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="11469" class="Symbol">(</a><a id="11470" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#11425" class="Bound">m</a> <a id="11472" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="11474" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a><a id="11478" class="Symbol">)</a>
  <a id="11482" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4079" class="Function Operator">≡⟨</a> <a id="11485" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#1075" class="Function">cong</a> <a id="11490" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="11494" class="Symbol">(</a><a id="11495" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#11307" class="Function">+-identityʳ</a> <a id="11507" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#11425" class="Bound">m</a><a id="11508" class="Symbol">)</a> <a id="11510" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4079" class="Function Operator">⟩</a>
    <a id="11516" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="11520" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#11425" class="Bound">m</a>
  <a id="11524" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4260" class="Function Operator">∎</a>{% endraw %}</pre>
The signature states that we are defining the identifier `+-identityʳ` which
provides evidence for the proposition:

    ∀ (m : ℕ) → m + zero ≡ m

Evidence for the proposition is a function that accepts a natural
number, binds it to `m`, and returns evidence for the corresponding
instance of the equation.  The proof is by induction on `m`.

For the base case, we must show:

    zero + zero ≡ zero

Simplifying with the base case of addition, this is straightforward.

For the inductive case, we must show:

    (suc m) + zero = suc m

Simplifying both sides with the inductive case of addition yields the equation:

    suc (m + zero) = suc m

This in turn follows by prefacing `suc` to both sides of the induction
hypothesis:

    m + zero ≡ m

Reading the chain of equations down from the top and up from the bottom
takes us to the simplified equation above.  The remaining
equation has the justification:

    ⟨ cong suc (+-identityʳ m) ⟩

Here, the recursive invocation `+-identityʳ m` has as its type the
induction hypothesis, and `cong suc` prefaces `suc` to each side to
yield the needed equation.  This completes the first lemma.

### The second lemma

The inductive case of the definition of addition pushes `suc` on the
first argument to the outside:

    suc m + n ≡ suc (m + n)

Our second lemma does the same for `suc` on the second argument:

    m + suc n ≡ suc (m + n)

Here is the lemma's statement and proof.
<pre class="Agda">{% raw %}<a id="+-suc"></a><a id="12980" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#12980" class="Function">+-suc</a> <a id="12986" class="Symbol">:</a> <a id="12988" class="Symbol">∀</a> <a id="12990" class="Symbol">(</a><a id="12991" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#12991" class="Bound">m</a> <a id="12993" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#12993" class="Bound">n</a> <a id="12995" class="Symbol">:</a> <a id="12997" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="12998" class="Symbol">)</a> <a id="13000" class="Symbol">→</a> <a id="13002" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#12991" class="Bound">m</a> <a id="13004" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="13006" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13010" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#12993" class="Bound">n</a> <a id="13012" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">≡</a> <a id="13014" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13018" class="Symbol">(</a><a id="13019" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#12991" class="Bound">m</a> <a id="13021" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="13023" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#12993" class="Bound">n</a><a id="13024" class="Symbol">)</a>
<a id="13026" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#12980" class="Function">+-suc</a> <a id="13032" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="13037" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13037" class="Bound">n</a> <a id="13039" class="Symbol">=</a>
  <a id="13043" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#3962" class="Function Operator">begin</a>
    <a id="13053" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="13058" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="13060" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13064" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13037" class="Bound">n</a>
  <a id="13068" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="13076" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13080" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13037" class="Bound">n</a>
  <a id="13084" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="13092" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13096" class="Symbol">(</a><a id="13097" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="13102" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="13104" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13037" class="Bound">n</a><a id="13105" class="Symbol">)</a>
  <a id="13109" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4260" class="Function Operator">∎</a>
<a id="13111" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#12980" class="Function">+-suc</a> <a id="13117" class="Symbol">(</a><a id="13118" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13122" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13122" class="Bound">m</a><a id="13123" class="Symbol">)</a> <a id="13125" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13125" class="Bound">n</a> <a id="13127" class="Symbol">=</a>
  <a id="13131" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#3962" class="Function Operator">begin</a>
    <a id="13141" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13145" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13122" class="Bound">m</a> <a id="13147" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="13149" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13153" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13125" class="Bound">n</a>
  <a id="13157" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="13165" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13169" class="Symbol">(</a><a id="13170" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13122" class="Bound">m</a> <a id="13172" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="13174" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13178" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13125" class="Bound">n</a><a id="13179" class="Symbol">)</a>
  <a id="13183" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4079" class="Function Operator">≡⟨</a> <a id="13186" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#1075" class="Function">cong</a> <a id="13191" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13195" class="Symbol">(</a><a id="13196" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#12980" class="Function">+-suc</a> <a id="13202" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13122" class="Bound">m</a> <a id="13204" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13125" class="Bound">n</a><a id="13205" class="Symbol">)</a> <a id="13207" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4079" class="Function Operator">⟩</a>
    <a id="13213" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13217" class="Symbol">(</a><a id="13218" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13222" class="Symbol">(</a><a id="13223" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13122" class="Bound">m</a> <a id="13225" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="13227" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13125" class="Bound">n</a><a id="13228" class="Symbol">))</a>
  <a id="13233" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="13241" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13245" class="Symbol">(</a><a id="13246" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13250" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13122" class="Bound">m</a> <a id="13252" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="13254" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#13125" class="Bound">n</a><a id="13255" class="Symbol">)</a>
  <a id="13259" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4260" class="Function Operator">∎</a>{% endraw %}</pre>
The signature states that we are defining the identifier `+-suc` which provides
evidence for the proposition:

    ∀ (m n : ℕ) → m + suc n ≡ suc (m + n)

Evidence for the proposition is a function that accepts two natural numbers,
binds them to `m` and `n`, and returns evidence for the corresponding instance
of the equation.  The proof is by induction on `m`.

For the base case, we must show:

    zero + suc n ≡ suc (zero + n)

Simplifying with the base case of addition, this is straightforward.

For the inductive case, we must show:

    suc m + suc n ≡ suc (suc m + n)

Simplifying both sides with the inductive case of addition yields the equation:

    suc (m + suc n) ≡ suc (suc (m + n))

This in turn follows by prefacing `suc` to both sides of the induction
hypothesis:

    m + suc n ≡ suc (m + n)

Reading the chain of equations down from the top and up from the bottom
takes us to the simplified equation in the middle.  The remaining
equation has the justification:

    ⟨ cong suc (+-suc m n) ⟩

Here, the recursive invocation `+-suc m n` has as its type the
induction hypothesis, and `cong suc` prefaces `suc` to each side to
yield the needed equation.  This completes the second lemma.

### The proposition

Finally, here is our proposition's statement and proof.
<pre class="Agda">{% raw %}<a id="+-comm"></a><a id="14569" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14569" class="Function">+-comm</a> <a id="14576" class="Symbol">:</a> <a id="14578" class="Symbol">∀</a> <a id="14580" class="Symbol">(</a><a id="14581" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14581" class="Bound">m</a> <a id="14583" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14583" class="Bound">n</a> <a id="14585" class="Symbol">:</a> <a id="14587" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="14588" class="Symbol">)</a> <a id="14590" class="Symbol">→</a> <a id="14592" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14581" class="Bound">m</a> <a id="14594" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="14596" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14583" class="Bound">n</a> <a id="14598" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">≡</a> <a id="14600" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14583" class="Bound">n</a> <a id="14602" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="14604" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14581" class="Bound">m</a>
<a id="14606" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14569" class="Function">+-comm</a> <a id="14613" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14613" class="Bound">m</a> <a id="14615" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="14620" class="Symbol">=</a>
  <a id="14624" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#3962" class="Function Operator">begin</a>
    <a id="14634" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14613" class="Bound">m</a> <a id="14636" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="14638" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>
  <a id="14645" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4079" class="Function Operator">≡⟨</a> <a id="14648" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#11307" class="Function">+-identityʳ</a> <a id="14660" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14613" class="Bound">m</a> <a id="14662" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4079" class="Function Operator">⟩</a>
    <a id="14668" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14613" class="Bound">m</a>
  <a id="14672" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="14680" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="14685" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="14687" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14613" class="Bound">m</a>
  <a id="14691" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4260" class="Function Operator">∎</a>
<a id="14693" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14569" class="Function">+-comm</a> <a id="14700" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14700" class="Bound">m</a> <a id="14702" class="Symbol">(</a><a id="14703" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="14707" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14707" class="Bound">n</a><a id="14708" class="Symbol">)</a> <a id="14710" class="Symbol">=</a>
  <a id="14714" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#3962" class="Function Operator">begin</a>
    <a id="14724" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14700" class="Bound">m</a> <a id="14726" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="14728" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="14732" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14707" class="Bound">n</a>
  <a id="14736" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4079" class="Function Operator">≡⟨</a> <a id="14739" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#12980" class="Function">+-suc</a> <a id="14745" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14700" class="Bound">m</a> <a id="14747" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14707" class="Bound">n</a> <a id="14749" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4079" class="Function Operator">⟩</a>
    <a id="14755" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="14759" class="Symbol">(</a><a id="14760" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14700" class="Bound">m</a> <a id="14762" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="14764" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14707" class="Bound">n</a><a id="14765" class="Symbol">)</a>
  <a id="14769" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4079" class="Function Operator">≡⟨</a> <a id="14772" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#1075" class="Function">cong</a> <a id="14777" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="14781" class="Symbol">(</a><a id="14782" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14569" class="Function">+-comm</a> <a id="14789" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14700" class="Bound">m</a> <a id="14791" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14707" class="Bound">n</a><a id="14792" class="Symbol">)</a> <a id="14794" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4079" class="Function Operator">⟩</a>
    <a id="14800" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="14804" class="Symbol">(</a><a id="14805" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14707" class="Bound">n</a> <a id="14807" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="14809" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14700" class="Bound">m</a><a id="14810" class="Symbol">)</a>
  <a id="14814" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4020" class="Function Operator">≡⟨⟩</a>
    <a id="14822" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="14826" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14707" class="Bound">n</a> <a id="14828" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="14830" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#14700" class="Bound">m</a>
  <a id="14834" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#4260" class="Function Operator">∎</a>{% endraw %}</pre>
The first line states that we are defining the identifier
`+-comm` which provides evidence for the proposition:

    ∀ (m n : ℕ) → m + n ≡ n + m

Evidence for the proposition is a function that accepts two
natural numbers, binds them to `m` and `n`, and returns evidence for the
corresponding instance of the equation.  The proof is by
induction on `n`.  (Not on `m` this time!)

For the base case, we must show:

    m + zero ≡ zero + m

Simplifying both sides with the base case of addition yields the equation:

    m + zero ≡ m

The the remaining equation has the justification `⟨ +-identityʳ m ⟩`,
which invokes the first lemma.

For the inductive case, we must show:

    m + suc n ≡ suc n + m

Simplifying both sides with the inductive case of addition yields the equation:

    m + suc n ≡ suc (n + m)

We show this in two steps.  First, we have:

    m + suc n ≡ suc (m + n)

which is justified by the second lemma, `⟨ +-suc m n ⟩`.  Then we
have

    suc (m + n) ≡ suc (n + m)

which is justified by congruence and the induction hypothesis,
`⟨ cong suc (+-comm m n) ⟩`.  This completes the proof.

Agda requires that identifiers are defined before they are used,
so we must present the lemmas before the main proposition, as we
have done above.  In practice, one will often attempt to prove
the main proposition first, and the equations required to do so
will suggest what lemmas to prove.


## Creation, one last time

Returning to the proof of associativity, it may be helpful to view the inductive
proof (or, equivalently, the recursive definition) as a creation story.  This
time we are concerned with judgements asserting associativity.

     -- in the beginning, we know nothing about associativity

Now, we apply the rules to all the judgements we know about.  The base
case tells us that `(zero + n) + p ≡ zero + (n + p)` for every natural
`n` and `p`.  The inductive case tells us that if `(m + n) + p ≡ m +
(n + p)` (on the day before today) then
`(suc m + n) + p ≡ suc m + (n + p)` (today).
We didn't know any judgments about associativity before today, so that
rule doesn't give us any new judgments.

    -- on the first day, we know about associativity of 0
    (0 + 0) + 0 ≡ 0 + (0 + 0)   ...   (0 + 4) + 5 ≡ 0 + (4 + 5)   ...

Then we repeat the process, so on the next day we know about all the
judgements from the day before, plus any judgements added by the rules.
The base case tells us nothing new, but now the inductive case adds
more judgements.

    -- on the second day, we know about associativity of 0 and 1
    (0 + 0) + 0 ≡ 0 + (0 + 0)   ...   (0 + 4) + 5 ≡ 0 + (4 + 5)   ...
    (1 + 0) + 0 ≡ 1 + (0 + 0)   ...   (1 + 4) + 5 ≡ 1 + (4 + 5)   ...

And we repeat the process again.

    -- on the third day, we know about associativity of 0, 1, and 2
    (0 + 0) + 0 ≡ 0 + (0 + 0)   ...   (0 + 4) + 5 ≡ 0 + (4 + 5)   ...
    (1 + 0) + 0 ≡ 1 + (0 + 0)   ...   (1 + 4) + 5 ≡ 1 + (4 + 5)   ...
    (2 + 0) + 0 ≡ 2 + (0 + 0)   ...   (2 + 4) + 5 ≡ 2 + (4 + 5)   ...

You've got the hang of it by now.

    -- on the fourth day, we know about associativity of 0, 1, 2, and 3
    (0 + 0) + 0 ≡ 0 + (0 + 0)   ...   (0 + 4) + 5 ≡ 0 + (4 + 5)   ...
    (1 + 0) + 0 ≡ 1 + (0 + 0)   ...   (1 + 4) + 5 ≡ 1 + (4 + 5)   ...
    (2 + 0) + 0 ≡ 2 + (0 + 0)   ...   (2 + 4) + 5 ≡ 2 + (4 + 5)   ...
    (3 + 0) + 0 ≡ 3 + (0 + 0)   ...   (3 + 4) + 5 ≡ 3 + (4 + 5)   ...

The process continues.  On the _m_'th day we will know all the
judgements where the first number is less than _m_.

There is also a completely finite approach to generating the same equations,
which is left as an exercise for the reader.

#### Exercise (`+-assoc-finite`)

Write out what is known about associativity on each of the first four
days using a finite story of creation, as
[earlier]({{ site.baseurl }}{% link out/plfa/Naturals.md %}#finite-creation).


## Associativity with rewrite

There is more than one way to skin a cat.  Here is a second proof of
associativity of addition in Agda, using `rewrite` rather than chains of
equations.
<pre class="Agda">{% raw %}<a id="+-assoc′"></a><a id="18896" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#18896" class="Function">+-assoc′</a> <a id="18905" class="Symbol">:</a> <a id="18907" class="Symbol">∀</a> <a id="18909" class="Symbol">(</a><a id="18910" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#18910" class="Bound">m</a> <a id="18912" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#18912" class="Bound">n</a> <a id="18914" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#18914" class="Bound">p</a> <a id="18916" class="Symbol">:</a> <a id="18918" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="18919" class="Symbol">)</a> <a id="18921" class="Symbol">→</a> <a id="18923" class="Symbol">(</a><a id="18924" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#18910" class="Bound">m</a> <a id="18926" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="18928" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#18912" class="Bound">n</a><a id="18929" class="Symbol">)</a> <a id="18931" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="18933" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#18914" class="Bound">p</a> <a id="18935" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">≡</a> <a id="18937" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#18910" class="Bound">m</a> <a id="18939" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="18941" class="Symbol">(</a><a id="18942" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#18912" class="Bound">n</a> <a id="18944" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="18946" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#18914" class="Bound">p</a><a id="18947" class="Symbol">)</a>
<a id="18949" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#18896" class="Function">+-assoc′</a> <a id="18958" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="18966" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#18966" class="Bound">n</a> <a id="18968" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#18968" class="Bound">p</a>                          <a id="18995" class="Symbol">=</a>  <a id="18998" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a>
<a id="19003" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#18896" class="Function">+-assoc′</a> <a id="19012" class="Symbol">(</a><a id="19013" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="19017" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#19017" class="Bound">m</a><a id="19018" class="Symbol">)</a> <a id="19020" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#19020" class="Bound">n</a> <a id="19022" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#19022" class="Bound">p</a>  <a id="19025" class="Keyword">rewrite</a> <a id="19033" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#18896" class="Function">+-assoc′</a> <a id="19042" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#19017" class="Bound">m</a> <a id="19044" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#19020" class="Bound">n</a> <a id="19046" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#19022" class="Bound">p</a>  <a id="19049" class="Symbol">=</a>  <a id="19052" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a>{% endraw %}</pre>

For the base case, we must show:

    (zero + n) + p ≡ zero + (n + p)

Simplifying both sides with the base case of addition yields the equation:

    n + p ≡ n + p

This holds trivially. The proof that a term is equal to itself is written `refl`.

For the inductive case, we must show:

    (suc m + n) + p ≡ suc m + (n + p)

Simplifying both sides with the inductive case of addition yields the equation:

    suc ((m + n) + p) ≡ suc (m + (n + p))

After rewriting with the inductive hypothesis these two terms are equal, and the
proof is again given by `refl`.  Rewriting by a given equation is indicated by
the keyword `rewrite` followed by a proof of that equation.  Rewriting avoids
not only chains of equations but also the need to invoke `cong`.


## Commutativity with rewrite

Here is a second proof of commutativity of addition, using `rewrite` rather than
chains of equations.
<pre class="Agda">{% raw %}<a id="+-identity′"></a><a id="19971" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#19971" class="Function">+-identity′</a> <a id="19983" class="Symbol">:</a> <a id="19985" class="Symbol">∀</a> <a id="19987" class="Symbol">(</a><a id="19988" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#19988" class="Bound">n</a> <a id="19990" class="Symbol">:</a> <a id="19992" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="19993" class="Symbol">)</a> <a id="19995" class="Symbol">→</a> <a id="19997" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#19988" class="Bound">n</a> <a id="19999" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="20001" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="20006" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">≡</a> <a id="20008" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#19988" class="Bound">n</a>
<a id="20010" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#19971" class="Function">+-identity′</a> <a id="20022" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="20027" class="Symbol">=</a> <a id="20029" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a>
<a id="20034" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#19971" class="Function">+-identity′</a> <a id="20046" class="Symbol">(</a><a id="20047" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="20051" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20051" class="Bound">n</a><a id="20052" class="Symbol">)</a> <a id="20054" class="Keyword">rewrite</a> <a id="20062" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#19971" class="Function">+-identity′</a> <a id="20074" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20051" class="Bound">n</a> <a id="20076" class="Symbol">=</a> <a id="20078" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a>

<a id="+-suc′"></a><a id="20084" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20084" class="Function">+-suc′</a> <a id="20091" class="Symbol">:</a> <a id="20093" class="Symbol">∀</a> <a id="20095" class="Symbol">(</a><a id="20096" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20096" class="Bound">m</a> <a id="20098" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20098" class="Bound">n</a> <a id="20100" class="Symbol">:</a> <a id="20102" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="20103" class="Symbol">)</a> <a id="20105" class="Symbol">→</a> <a id="20107" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20096" class="Bound">m</a> <a id="20109" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="20111" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="20115" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20098" class="Bound">n</a> <a id="20117" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">≡</a> <a id="20119" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="20123" class="Symbol">(</a><a id="20124" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20096" class="Bound">m</a> <a id="20126" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="20128" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20098" class="Bound">n</a><a id="20129" class="Symbol">)</a>
<a id="20131" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20084" class="Function">+-suc′</a> <a id="20138" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="20143" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20143" class="Bound">n</a> <a id="20145" class="Symbol">=</a> <a id="20147" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a>
<a id="20152" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20084" class="Function">+-suc′</a> <a id="20159" class="Symbol">(</a><a id="20160" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="20164" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20164" class="Bound">m</a><a id="20165" class="Symbol">)</a> <a id="20167" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20167" class="Bound">n</a> <a id="20169" class="Keyword">rewrite</a> <a id="20177" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20084" class="Function">+-suc′</a> <a id="20184" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20164" class="Bound">m</a> <a id="20186" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20167" class="Bound">n</a> <a id="20188" class="Symbol">=</a> <a id="20190" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a>

<a id="+-comm′"></a><a id="20196" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20196" class="Function">+-comm′</a> <a id="20204" class="Symbol">:</a> <a id="20206" class="Symbol">∀</a> <a id="20208" class="Symbol">(</a><a id="20209" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20209" class="Bound">m</a> <a id="20211" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20211" class="Bound">n</a> <a id="20213" class="Symbol">:</a> <a id="20215" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="20216" class="Symbol">)</a> <a id="20218" class="Symbol">→</a> <a id="20220" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20209" class="Bound">m</a> <a id="20222" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="20224" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20211" class="Bound">n</a> <a id="20226" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">≡</a> <a id="20228" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20211" class="Bound">n</a> <a id="20230" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="20232" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20209" class="Bound">m</a>
<a id="20234" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20196" class="Function">+-comm′</a> <a id="20242" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20242" class="Bound">m</a> <a id="20244" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="20249" class="Keyword">rewrite</a> <a id="20257" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#19971" class="Function">+-identity′</a> <a id="20269" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20242" class="Bound">m</a> <a id="20271" class="Symbol">=</a> <a id="20273" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a>
<a id="20278" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20196" class="Function">+-comm′</a> <a id="20286" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20286" class="Bound">m</a> <a id="20288" class="Symbol">(</a><a id="20289" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="20293" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20293" class="Bound">n</a><a id="20294" class="Symbol">)</a> <a id="20296" class="Keyword">rewrite</a> <a id="20304" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20084" class="Function">+-suc′</a> <a id="20311" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20286" class="Bound">m</a> <a id="20313" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20293" class="Bound">n</a> <a id="20315" class="Symbol">|</a> <a id="20317" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20196" class="Function">+-comm′</a> <a id="20325" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20286" class="Bound">m</a> <a id="20327" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Induction.md %}{% raw %}#20293" class="Bound">n</a> <a id="20329" class="Symbol">=</a> <a id="20331" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a>{% endraw %}</pre>
In the final line, rewriting with two equations is
indicated by separating the two proofs of the relevant equations by a
vertical bar; the rewrite on the left is performed before that on the
right.


## Building proofs interactively

It is instructive to see how to build the alternative proof of
associativity using the interactive features of Agda in Emacs.
Begin by typing

    +-assoc′ : ∀ (m n p : ℕ) → (m + n) + p ≡ m + (n + p)
    +-assoc′ m n p = ?

The question mark indicates that you would like Agda to help with
filling in that part of the code.  If you type `C-c C-l` (control-c
followed by control-l), the question mark will be replaced.

    +-assoc′ : ∀ (m n p : ℕ) → (m + n) + p ≡ m + (n + p)
    +-assoc′ m n p = { }0

The empty braces are called a _hole_, and 0 is a number used for
referring to the hole.  The hole may display highlighted in green.
Emacs will also create a new window at the bottom of the screen
displaying the text

    ?0 : ((m + n) + p) ≡ (m + (n + p))

This indicates that hole 0 is to be filled in with a proof of
the stated judgement.

We wish to prove the proposition by induction on `m`.  Move
the cursor into the hole and type `C-c C-c`.  You will be given
the prompt:

    pattern variables to case (empty for split on result):

Typing `m` will cause a split on that variable, resulting
in an update to the code.

    +-assoc′ : ∀ (m n p : ℕ) → (m + n) + p ≡ m + (n + p)
    +-assoc′ zero n p = { }0
    +-assoc′ (suc m) n p = { }1

There are now two holes, and the window at the bottom tells you what
each is required to prove:

    ?0 : ((zero + n) + p) ≡ (zero + (n + p))
    ?1 : ((suc m + n) + p) ≡ (suc m + (n + p))

Going into hole 0 and typing `C-c C-,` will display the text:

    Goal: (n + p) ≡ (n + p)
    ————————————————————————————————————————————————————————————
    p : ℕ
    n : ℕ

This indicates that after simplification the goal for hole 0 is as
stated, and that variables `p` and `n` of the stated types are
available to use in the proof.  The proof of the given goal is
trivial, and going into the goal and typing `C-c C-r` will fill it in,
renumbering the remaining hole to 0:

    +-assoc′ : ∀ (m n p : ℕ) → (m + n) + p ≡ m + (n + p)
    +-assoc′ zero n p = refl
    +-assoc′ (suc m) n p = { }0

Going into the new hole 0 and typing `C-c C-,` will display the text:

    Goal: suc ((m + n) + p) ≡ suc (m + (n + p))
    ————————————————————————————————————————————————————————————
    p : ℕ
    n : ℕ
    m : ℕ

Again, this gives the simplified goal and the available variables.
In this case, we need to rewrite by the induction
hypothesis, so let's edit the text accordingly:

    +-assoc′ : ∀ (m n p : ℕ) → (m + n) + p ≡ m + (n + p)
    +-assoc′ zero n p = refl
    +-assoc′ (suc m) n p rewrite +-assoc′ m n p = { }0

Going into the remaining hole and typing `C-c C-,` will display the text:

    Goal: suc (m + (n + p)) ≡ suc (m + (n + p))
    ————————————————————————————————————————————————————————————
    p : ℕ
    n : ℕ
    m : ℕ

The proof of the given goal is trivial, and going into the goal and
typing `C-c C-r` will fill it in, completing the proof:

    +-assoc′ : ∀ (m n p : ℕ) → (m + n) + p ≡ m + (n + p)
    +-assoc′ zero n p = refl
    +-assoc′ (suc m) n p rewrite +-assoc′ m n p = refl


#### Exercise (`+-swap`)

Show

    m + (n + p) ≡ n + (m + p)

for all naturals `m`, `n`, and `p`. No induction is needed,
just apply the previous results which show addition
is associative and commutative.  You may need to use
the following function from the standard library:

    sym : ∀ {m n : ℕ} → m ≡ n → n ≡ m

#### Exercise (`*-distrib-+`)

Show multiplication distributes over addition, that is,

    (m + n) * p ≡ m * p + n * p

for all naturals `m`, `n`, and `p`.

#### Exercise (`*-assoc`)

Show multiplication is associative, that is,

    (m * n) * p ≡ m * (n * p)

for all naturals `m`, `n`, and `p`.

#### Exercise (`*-comm`)

Show multiplication is commutative, that is,

    m * n ≡ n * m

for all naturals `m` and `n`.  As with commutativity of addition,
you will need to formulate and prove suitable lemmas.

#### Exercise (`0∸n≡0`)

Show

    zero ∸ n ≡ zero

for all naturals `n`. Did your proof require induction?

#### Exercise (`∸-+-assoc`)

Show that monus associates with addition, that is,

    m ∸ n ∸ p ≡ m ∸ (n + p)

for all naturals `m`, `n`, and `p`.

## Standard library

Definitions similar to those in this chapter can be found in the standard library.
<pre class="Agda">{% raw %}<a id="24827" class="Keyword">import</a> <a id="24834" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html" class="Module">Data.Nat.Properties</a> <a id="24854" class="Keyword">using</a> <a id="24860" class="Symbol">(</a><a id="24861" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#7782" class="Function">+-assoc</a><a id="24868" class="Symbol">;</a> <a id="24870" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#7938" class="Function">+-identityʳ</a><a id="24881" class="Symbol">;</a> <a id="24883" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#7679" class="Function">+-suc</a><a id="24888" class="Symbol">;</a> <a id="24890" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#8115" class="Function">+-comm</a><a id="24896" class="Symbol">)</a>{% endraw %}</pre>

## Unicode

This chapter uses the following unicode.

    ∀  U+2200  FOR ALL (\forall)
    ʳ  U+02B3  MODIFIER LETTER SMALL R (\^r)
    ′  U+2032  PRIME (\')
    ″  U+2033  DOUBLE PRIME (\')
    ‴  U+2034  TRIPLE PRIME (\')
    ⁗  U+2057  QUADRUPLE PRIME (\')

Similar to `\r`, the command `\^r` gives access to a variety of
superscript rightward arrows, and also a superscript letter `r`.
The command `\'` gives access to a range of primes (`′ ″ ‴ ⁗`).
