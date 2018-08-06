---
title     : "Equality: Equality and equational reasoning"
layout    : page
permalink : /Equality/
---

<pre class="Agda">{% raw %}<a id="121" class="Keyword">module</a> <a id="128" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}" class="Module">plfa.Equality</a> <a id="142" class="Keyword">where</a>{% endraw %}</pre>

Much of our reasoning has involved equality.  Given two terms `M`
and `N`, both of type `A`, we write `M ≡ N` to assert that `M` and `N`
are interchangeable.  So far we have treated equality as a primitive,
here we show how to define it as an inductive datatype.


## Imports

This chapter has no imports.  Every chapter in this book, and nearly
every module in the Agda, imports equality.  Since we define equality
here, any import would create a conflict.


## Equality

We declare equality as follows.
<pre class="Agda">{% raw %}<a id="678" class="Keyword">data</a> <a id="_≡_"></a><a id="683" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">_≡_</a> <a id="687" class="Symbol">{</a><a id="688" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#688" class="Bound">A</a> <a id="690" class="Symbol">:</a> <a id="692" class="PrimitiveType">Set</a><a id="695" class="Symbol">}</a> <a id="697" class="Symbol">(</a><a id="698" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#698" class="Bound">x</a> <a id="700" class="Symbol">:</a> <a id="702" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#688" class="Bound">A</a><a id="703" class="Symbol">)</a> <a id="705" class="Symbol">:</a> <a id="707" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#688" class="Bound">A</a> <a id="709" class="Symbol">→</a> <a id="711" class="PrimitiveType">Set</a> <a id="715" class="Keyword">where</a>
  <a id="_≡_.refl"></a><a id="723" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a> <a id="728" class="Symbol">:</a> <a id="730" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#698" class="Bound">x</a> <a id="732" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="734" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#698" class="Bound">x</a>{% endraw %}</pre>
In other words, for any type `A` and for any `x` of type `A`, the
constructor `refl` provides evidence that `x ≡ x`. Hence, every value
is equivalent to itself, and we have no other way of showing values
are equivalent.  The definition features an asymmetry, in that the
first argument to `_≡_` is given by the parameter `x : A`, while the
second is given by an index in `A → Set`.  This follows our policy
of using parameters wherever possible.  The first argument to `_≡_`
can be a parameter because it doesn't vary, while the second must be
an index, so it can be required to be equal to the first.

We declare the precedence of equivalence as follows.
<pre class="Agda">{% raw %}<a id="1416" class="Keyword">infix</a> <a id="1422" class="Number">4</a> <a id="1424" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">_≡_</a>{% endraw %}</pre>
We set the precedence of `_≡_` at level 4, the same as `_≤_`,
which means it binds less tightly than any arithmetic operator.
It associates neither to left nor right; writing `x ≡ y ≡ z`
is illegal.


## Equality is an equivalence relation

An equivalence relation is one which is reflexive, symmetric, and transitive.
Reflexivity is built-in to the definition of equivalence, via the
constructor `refl`.  It is straightforward to show symmetry.
<pre class="Agda">{% raw %}<a id="sym"></a><a id="1898" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#1898" class="Function">sym</a> <a id="1902" class="Symbol">:</a> <a id="1904" class="Symbol">∀</a> <a id="1906" class="Symbol">{</a><a id="1907" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#1907" class="Bound">A</a> <a id="1909" class="Symbol">:</a> <a id="1911" class="PrimitiveType">Set</a><a id="1914" class="Symbol">}</a> <a id="1916" class="Symbol">{</a><a id="1917" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#1917" class="Bound">x</a> <a id="1919" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#1919" class="Bound">y</a> <a id="1921" class="Symbol">:</a> <a id="1923" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#1907" class="Bound">A</a><a id="1924" class="Symbol">}</a>
  <a id="1928" class="Symbol">→</a> <a id="1930" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#1917" class="Bound">x</a> <a id="1932" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="1934" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#1919" class="Bound">y</a>
    <a id="1940" class="Comment">-----</a>
  <a id="1948" class="Symbol">→</a> <a id="1950" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#1919" class="Bound">y</a> <a id="1952" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="1954" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#1917" class="Bound">x</a>
<a id="1956" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#1898" class="Function">sym</a> <a id="1960" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a> <a id="1965" class="Symbol">=</a> <a id="1967" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a>{% endraw %}</pre>
How does this proof work? The argument to `sym` has type `x ≡ y`, but
on the left-hand side of the equation the argument has been
instantiated to the pattern `refl`, which requires that `x` and `y`
are the same.  Hence, for the right-hand side of the equation we need
a term of type `x ≡ x`, and `refl` will do.

It is instructive to develop `sym` interactively.  To start, we supply
a variable for the argument on the left, and a hole for the body on
the right:

    sym : ∀ {A : Set} {x y : A}
      → x ≡ y
        -----
      → y ≡ x
    sym e = {! !}

If we go into the hole and type `C-c C-,` then Agda reports:

    Goal: .y ≡ .x
    ————————————————————————————————————————————————————————————
    e  : .x ≡ .y
    .y : .A
    .x : .A
    .A : Set

If in the hole we type `C-c C-c e` then Agda will instantiate `e` to
all possible constructors, with one equation for each. There is only
one possible constructor:

    sym : ∀ {A : Set} {x y : A}
      → x ≡ y
        -----
      → y ≡ x
    sym refl = {! !}

If we go into the hole again and type `C-c C-,` then Agda now reports:

     Goal: .x ≡ .x
     ————————————————————————————————————————————————————————————
     .x : .A
     .A : Set

This is the key step---Agda has worked out that `x` and `y` must be
the same to match the pattern `refl`!

Finally, if we go back into the hole and type `C-c C-r` it will
instantiate the hole with the one constructor that yields a value of
the expected type.

    sym : ∀ {A : Set} {x y : A}
      → x ≡ y
        -----
      → y ≡ x
    sym refl = refl

This completes the definition as given above.

Transitivity is equally straightforward.
<pre class="Agda">{% raw %}<a id="trans"></a><a id="3642" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#3642" class="Function">trans</a> <a id="3648" class="Symbol">:</a> <a id="3650" class="Symbol">∀</a> <a id="3652" class="Symbol">{</a><a id="3653" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#3653" class="Bound">A</a> <a id="3655" class="Symbol">:</a> <a id="3657" class="PrimitiveType">Set</a><a id="3660" class="Symbol">}</a> <a id="3662" class="Symbol">{</a><a id="3663" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#3663" class="Bound">x</a> <a id="3665" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#3665" class="Bound">y</a> <a id="3667" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#3667" class="Bound">z</a> <a id="3669" class="Symbol">:</a> <a id="3671" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#3653" class="Bound">A</a><a id="3672" class="Symbol">}</a>
  <a id="3676" class="Symbol">→</a> <a id="3678" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#3663" class="Bound">x</a> <a id="3680" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="3682" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#3665" class="Bound">y</a>
  <a id="3686" class="Symbol">→</a> <a id="3688" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#3665" class="Bound">y</a> <a id="3690" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="3692" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#3667" class="Bound">z</a>
    <a id="3698" class="Comment">-----</a>
  <a id="3706" class="Symbol">→</a> <a id="3708" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#3663" class="Bound">x</a> <a id="3710" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="3712" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#3667" class="Bound">z</a>
<a id="3714" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#3642" class="Function">trans</a> <a id="3720" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a> <a id="3725" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a>  <a id="3731" class="Symbol">=</a>  <a id="3734" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a>{% endraw %}</pre>
Again, a useful exercise is to carry out an interactive development, checking
how Agda's knowledge changes as each of the two arguments is
instantiated.

## Congruence and substitution {#cong}

Equality satisfies *congruence*.  If two terms are equal,
they remain so after the same function is applied to both.
<pre class="Agda">{% raw %}<a id="cong"></a><a id="4074" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4074" class="Function">cong</a> <a id="4079" class="Symbol">:</a> <a id="4081" class="Symbol">∀</a> <a id="4083" class="Symbol">{</a><a id="4084" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4084" class="Bound">A</a> <a id="4086" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4086" class="Bound">B</a> <a id="4088" class="Symbol">:</a> <a id="4090" class="PrimitiveType">Set</a><a id="4093" class="Symbol">}</a> <a id="4095" class="Symbol">(</a><a id="4096" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4096" class="Bound">f</a> <a id="4098" class="Symbol">:</a> <a id="4100" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4084" class="Bound">A</a> <a id="4102" class="Symbol">→</a> <a id="4104" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4086" class="Bound">B</a><a id="4105" class="Symbol">)</a> <a id="4107" class="Symbol">{</a><a id="4108" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4108" class="Bound">x</a> <a id="4110" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4110" class="Bound">y</a> <a id="4112" class="Symbol">:</a> <a id="4114" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4084" class="Bound">A</a><a id="4115" class="Symbol">}</a>
  <a id="4119" class="Symbol">→</a> <a id="4121" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4108" class="Bound">x</a> <a id="4123" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="4125" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4110" class="Bound">y</a>
    <a id="4131" class="Comment">---------</a>
  <a id="4143" class="Symbol">→</a> <a id="4145" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4096" class="Bound">f</a> <a id="4147" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4108" class="Bound">x</a> <a id="4149" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="4151" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4096" class="Bound">f</a> <a id="4153" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4110" class="Bound">y</a>
<a id="4155" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4074" class="Function">cong</a> <a id="4160" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4160" class="Bound">f</a> <a id="4162" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a>  <a id="4168" class="Symbol">=</a>  <a id="4171" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a>{% endraw %}</pre>

Congruence of functions with two arguments is similar.
<pre class="Agda">{% raw %}<a id="cong₂"></a><a id="4256" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4256" class="Function">cong₂</a> <a id="4262" class="Symbol">:</a> <a id="4264" class="Symbol">∀</a> <a id="4266" class="Symbol">{</a><a id="4267" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4267" class="Bound">A</a> <a id="4269" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4269" class="Bound">B</a> <a id="4271" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4271" class="Bound">C</a> <a id="4273" class="Symbol">:</a> <a id="4275" class="PrimitiveType">Set</a><a id="4278" class="Symbol">}</a> <a id="4280" class="Symbol">(</a><a id="4281" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4281" class="Bound">f</a> <a id="4283" class="Symbol">:</a> <a id="4285" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4267" class="Bound">A</a> <a id="4287" class="Symbol">→</a> <a id="4289" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4269" class="Bound">B</a> <a id="4291" class="Symbol">→</a> <a id="4293" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4271" class="Bound">C</a><a id="4294" class="Symbol">)</a> <a id="4296" class="Symbol">{</a><a id="4297" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4297" class="Bound">u</a> <a id="4299" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4299" class="Bound">x</a> <a id="4301" class="Symbol">:</a> <a id="4303" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4267" class="Bound">A</a><a id="4304" class="Symbol">}</a> <a id="4306" class="Symbol">{</a><a id="4307" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4307" class="Bound">v</a> <a id="4309" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4309" class="Bound">y</a> <a id="4311" class="Symbol">:</a> <a id="4313" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4269" class="Bound">B</a><a id="4314" class="Symbol">}</a>
  <a id="4318" class="Symbol">→</a> <a id="4320" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4297" class="Bound">u</a> <a id="4322" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="4324" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4299" class="Bound">x</a>
  <a id="4328" class="Symbol">→</a> <a id="4330" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4307" class="Bound">v</a> <a id="4332" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="4334" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4309" class="Bound">y</a>
    <a id="4340" class="Comment">-------------</a>
  <a id="4356" class="Symbol">→</a> <a id="4358" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4281" class="Bound">f</a> <a id="4360" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4297" class="Bound">u</a> <a id="4362" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4307" class="Bound">v</a> <a id="4364" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="4366" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4281" class="Bound">f</a> <a id="4368" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4299" class="Bound">x</a> <a id="4370" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4309" class="Bound">y</a>
<a id="4372" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4256" class="Function">cong₂</a> <a id="4378" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4378" class="Bound">f</a> <a id="4380" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a> <a id="4385" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a>  <a id="4391" class="Symbol">=</a>  <a id="4394" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a>{% endraw %}</pre>

Equality is also a congruence in the function position of an application.
If two functions are equal, then applying them to the same term
yields equal terms.
<pre class="Agda">{% raw %}<a id="cong-app"></a><a id="4582" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4582" class="Function">cong-app</a> <a id="4591" class="Symbol">:</a> <a id="4593" class="Symbol">∀</a> <a id="4595" class="Symbol">{</a><a id="4596" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4596" class="Bound">A</a> <a id="4598" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4598" class="Bound">B</a> <a id="4600" class="Symbol">:</a> <a id="4602" class="PrimitiveType">Set</a><a id="4605" class="Symbol">}</a> <a id="4607" class="Symbol">{</a><a id="4608" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4608" class="Bound">f</a> <a id="4610" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4610" class="Bound">g</a> <a id="4612" class="Symbol">:</a> <a id="4614" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4596" class="Bound">A</a> <a id="4616" class="Symbol">→</a> <a id="4618" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4598" class="Bound">B</a><a id="4619" class="Symbol">}</a>
  <a id="4623" class="Symbol">→</a> <a id="4625" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4608" class="Bound">f</a> <a id="4627" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="4629" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4610" class="Bound">g</a>
    <a id="4635" class="Comment">---------------------</a>
  <a id="4659" class="Symbol">→</a> <a id="4661" class="Symbol">∀</a> <a id="4663" class="Symbol">(</a><a id="4664" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4664" class="Bound">x</a> <a id="4666" class="Symbol">:</a> <a id="4668" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4596" class="Bound">A</a><a id="4669" class="Symbol">)</a> <a id="4671" class="Symbol">→</a> <a id="4673" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4608" class="Bound">f</a> <a id="4675" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4664" class="Bound">x</a> <a id="4677" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="4679" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4610" class="Bound">g</a> <a id="4681" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4664" class="Bound">x</a>
<a id="4683" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4582" class="Function">cong-app</a> <a id="4692" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a> <a id="4697" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4697" class="Bound">x</a> <a id="4699" class="Symbol">=</a> <a id="4701" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a>{% endraw %}</pre>

Equality also satisfies *substitution*.
If two values are equal and a predicate holds of the first then it also holds of the second.
<pre class="Agda">{% raw %}<a id="subst"></a><a id="4864" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4864" class="Function">subst</a> <a id="4870" class="Symbol">:</a> <a id="4872" class="Symbol">∀</a> <a id="4874" class="Symbol">{</a><a id="4875" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4875" class="Bound">A</a> <a id="4877" class="Symbol">:</a> <a id="4879" class="PrimitiveType">Set</a><a id="4882" class="Symbol">}</a> <a id="4884" class="Symbol">{</a><a id="4885" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4885" class="Bound">x</a> <a id="4887" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4887" class="Bound">y</a> <a id="4889" class="Symbol">:</a> <a id="4891" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4875" class="Bound">A</a><a id="4892" class="Symbol">}</a> <a id="4894" class="Symbol">(</a><a id="4895" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4895" class="Bound">P</a> <a id="4897" class="Symbol">:</a> <a id="4899" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4875" class="Bound">A</a> <a id="4901" class="Symbol">→</a> <a id="4903" class="PrimitiveType">Set</a><a id="4906" class="Symbol">)</a>
  <a id="4910" class="Symbol">→</a> <a id="4912" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4885" class="Bound">x</a> <a id="4914" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="4916" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4887" class="Bound">y</a>
    <a id="4922" class="Comment">---------</a>
  <a id="4934" class="Symbol">→</a> <a id="4936" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4895" class="Bound">P</a> <a id="4938" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4885" class="Bound">x</a> <a id="4940" class="Symbol">→</a> <a id="4942" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4895" class="Bound">P</a> <a id="4944" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4887" class="Bound">y</a>
<a id="4946" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4864" class="Function">subst</a> <a id="4952" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4952" class="Bound">P</a> <a id="4954" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a> <a id="4959" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4959" class="Bound">px</a> <a id="4962" class="Symbol">=</a> <a id="4964" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4959" class="Bound">px</a>{% endraw %}</pre>


## Chains of equations

Here we show how to support reasoning with chains of equations
as used throughout the book.  We package the declarations
into a module, named `≡-Reasoning`, to match the format used in Agda's
standard library.
<pre class="Agda">{% raw %}<a id="5227" class="Keyword">module</a> <a id="≡-Reasoning"></a><a id="5234" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5234" class="Module">≡-Reasoning</a> <a id="5246" class="Symbol">{</a><a id="5247" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5247" class="Bound">A</a> <a id="5249" class="Symbol">:</a> <a id="5251" class="PrimitiveType">Set</a><a id="5254" class="Symbol">}</a> <a id="5256" class="Keyword">where</a>

  <a id="5265" class="Keyword">infix</a>  <a id="5272" class="Number">1</a> <a id="5274" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5322" class="Function Operator">begin_</a>
  <a id="5283" class="Keyword">infixr</a> <a id="5290" class="Number">2</a> <a id="5292" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5402" class="Function Operator">_≡⟨⟩_</a> <a id="5298" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5487" class="Function Operator">_≡⟨_⟩_</a>
  <a id="5307" class="Keyword">infix</a>  <a id="5314" class="Number">3</a> <a id="5316" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5602" class="Function Operator">_∎</a>

  <a id="≡-Reasoning.begin_"></a><a id="5322" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5322" class="Function Operator">begin_</a> <a id="5329" class="Symbol">:</a> <a id="5331" class="Symbol">∀</a> <a id="5333" class="Symbol">{</a><a id="5334" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5334" class="Bound">x</a> <a id="5336" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5336" class="Bound">y</a> <a id="5338" class="Symbol">:</a> <a id="5340" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5247" class="Bound">A</a><a id="5341" class="Symbol">}</a>
    <a id="5347" class="Symbol">→</a> <a id="5349" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5334" class="Bound">x</a> <a id="5351" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="5353" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5336" class="Bound">y</a>
      <a id="5361" class="Comment">-----</a>
    <a id="5371" class="Symbol">→</a> <a id="5373" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5334" class="Bound">x</a> <a id="5375" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="5377" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5336" class="Bound">y</a>
  <a id="5381" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5322" class="Function Operator">begin</a> <a id="5387" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5387" class="Bound">x≡y</a>  <a id="5392" class="Symbol">=</a>  <a id="5395" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5387" class="Bound">x≡y</a>

  <a id="≡-Reasoning._≡⟨⟩_"></a><a id="5402" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5402" class="Function Operator">_≡⟨⟩_</a> <a id="5408" class="Symbol">:</a> <a id="5410" class="Symbol">∀</a> <a id="5412" class="Symbol">(</a><a id="5413" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5413" class="Bound">x</a> <a id="5415" class="Symbol">:</a> <a id="5417" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5247" class="Bound">A</a><a id="5418" class="Symbol">)</a> <a id="5420" class="Symbol">{</a><a id="5421" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5421" class="Bound">y</a> <a id="5423" class="Symbol">:</a> <a id="5425" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5247" class="Bound">A</a><a id="5426" class="Symbol">}</a>
    <a id="5432" class="Symbol">→</a> <a id="5434" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5413" class="Bound">x</a> <a id="5436" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="5438" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5421" class="Bound">y</a>
      <a id="5446" class="Comment">-----</a>
    <a id="5456" class="Symbol">→</a> <a id="5458" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5413" class="Bound">x</a> <a id="5460" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="5462" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5421" class="Bound">y</a>
  <a id="5466" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5466" class="Bound">x</a> <a id="5468" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5402" class="Function Operator">≡⟨⟩</a> <a id="5472" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5472" class="Bound">x≡y</a>  <a id="5477" class="Symbol">=</a>  <a id="5480" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5472" class="Bound">x≡y</a>

  <a id="≡-Reasoning._≡⟨_⟩_"></a><a id="5487" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5487" class="Function Operator">_≡⟨_⟩_</a> <a id="5494" class="Symbol">:</a> <a id="5496" class="Symbol">∀</a> <a id="5498" class="Symbol">(</a><a id="5499" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5499" class="Bound">x</a> <a id="5501" class="Symbol">:</a> <a id="5503" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5247" class="Bound">A</a><a id="5504" class="Symbol">)</a> <a id="5506" class="Symbol">{</a><a id="5507" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5507" class="Bound">y</a> <a id="5509" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5509" class="Bound">z</a> <a id="5511" class="Symbol">:</a> <a id="5513" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5247" class="Bound">A</a><a id="5514" class="Symbol">}</a>
    <a id="5520" class="Symbol">→</a> <a id="5522" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5499" class="Bound">x</a> <a id="5524" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="5526" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5507" class="Bound">y</a>
    <a id="5532" class="Symbol">→</a> <a id="5534" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5507" class="Bound">y</a> <a id="5536" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="5538" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5509" class="Bound">z</a>
      <a id="5546" class="Comment">-----</a>
    <a id="5556" class="Symbol">→</a> <a id="5558" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5499" class="Bound">x</a> <a id="5560" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="5562" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5509" class="Bound">z</a>
  <a id="5566" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5566" class="Bound">x</a> <a id="5568" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5487" class="Function Operator">≡⟨</a> <a id="5571" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5571" class="Bound">x≡y</a> <a id="5575" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5487" class="Function Operator">⟩</a> <a id="5577" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5577" class="Bound">y≡z</a>  <a id="5582" class="Symbol">=</a>  <a id="5585" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#3642" class="Function">trans</a> <a id="5591" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5571" class="Bound">x≡y</a> <a id="5595" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5577" class="Bound">y≡z</a>

  <a id="≡-Reasoning._∎"></a><a id="5602" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5602" class="Function Operator">_∎</a> <a id="5605" class="Symbol">:</a> <a id="5607" class="Symbol">∀</a> <a id="5609" class="Symbol">(</a><a id="5610" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5610" class="Bound">x</a> <a id="5612" class="Symbol">:</a> <a id="5614" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5247" class="Bound">A</a><a id="5615" class="Symbol">)</a>
      <a id="5623" class="Comment">-----</a>
    <a id="5633" class="Symbol">→</a> <a id="5635" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5610" class="Bound">x</a> <a id="5637" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="5639" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5610" class="Bound">x</a>
  <a id="5643" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5643" class="Bound">x</a> <a id="5645" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5602" class="Function Operator">∎</a>  <a id="5648" class="Symbol">=</a>  <a id="5651" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a>

<a id="5657" class="Keyword">open</a> <a id="5662" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5234" class="Module">≡-Reasoning</a>{% endraw %}</pre>
This is our first use of a nested module. It consists of the keyword
`module` followed by the module name and any parameters, explicit or
implicit, the keyword `where`, and the contents of the module indented.
Modules may contain any sort of declaration, including nested modules.
Nested modules are similar to the top-level modules that constitute
each chapter of this book, save that the body of a top-level module
need not be indented.  Opening the module makes all of the definitions
available in the current environment.

As an example, let's look at a proof of transitivity
as a chain of equations.
<pre class="Agda">{% raw %}<a id="trans′"></a><a id="6303" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6303" class="Function">trans′</a> <a id="6310" class="Symbol">:</a> <a id="6312" class="Symbol">∀</a> <a id="6314" class="Symbol">{</a><a id="6315" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6315" class="Bound">A</a> <a id="6317" class="Symbol">:</a> <a id="6319" class="PrimitiveType">Set</a><a id="6322" class="Symbol">}</a> <a id="6324" class="Symbol">{</a><a id="6325" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6325" class="Bound">x</a> <a id="6327" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6327" class="Bound">y</a> <a id="6329" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6329" class="Bound">z</a> <a id="6331" class="Symbol">:</a> <a id="6333" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6315" class="Bound">A</a><a id="6334" class="Symbol">}</a>
  <a id="6338" class="Symbol">→</a> <a id="6340" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6325" class="Bound">x</a> <a id="6342" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="6344" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6327" class="Bound">y</a>
  <a id="6348" class="Symbol">→</a> <a id="6350" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6327" class="Bound">y</a> <a id="6352" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="6354" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6329" class="Bound">z</a>
    <a id="6360" class="Comment">-----</a>
  <a id="6368" class="Symbol">→</a> <a id="6370" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6325" class="Bound">x</a> <a id="6372" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="6374" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6329" class="Bound">z</a>
<a id="6376" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6303" class="Function">trans′</a> <a id="6383" class="Symbol">{</a><a id="6384" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6384" class="Bound">A</a><a id="6385" class="Symbol">}</a> <a id="6387" class="Symbol">{</a><a id="6388" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6388" class="Bound">x</a><a id="6389" class="Symbol">}</a> <a id="6391" class="Symbol">{</a><a id="6392" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6392" class="Bound">y</a><a id="6393" class="Symbol">}</a> <a id="6395" class="Symbol">{</a><a id="6396" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6396" class="Bound">z</a><a id="6397" class="Symbol">}</a> <a id="6399" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6399" class="Bound">x≡y</a> <a id="6403" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6403" class="Bound">y≡z</a> <a id="6407" class="Symbol">=</a>
  <a id="6411" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5322" class="Function Operator">begin</a>
    <a id="6421" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6388" class="Bound">x</a>
  <a id="6425" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5487" class="Function Operator">≡⟨</a> <a id="6428" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6399" class="Bound">x≡y</a> <a id="6432" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5487" class="Function Operator">⟩</a>
    <a id="6438" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6392" class="Bound">y</a>
  <a id="6442" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5487" class="Function Operator">≡⟨</a> <a id="6445" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6403" class="Bound">y≡z</a> <a id="6449" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5487" class="Function Operator">⟩</a>
    <a id="6455" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#6396" class="Bound">z</a>
  <a id="6459" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5602" class="Function Operator">∎</a>{% endraw %}</pre>
According to the fixity declarations, the body parses as follows:

    begin (x ≡⟨ x≡y ⟩ (y ≡⟨ y≡z ⟩ (z ∎)))

The application of `begin` is purely cosmetic, as it simply returns
its argument.  That argument consists of `_≡⟨_⟩_` applied to `x`,
`x≡y`, and `y ≡⟨ y≡z ⟩ (z ∎)`.  The first argument is a term, `x`,
while the second and third arguments are both proofs of equations, in
particular proofs of `x ≡ y` and `y ≡ z` respectively, which are
combined by `trans` in the body of `_≡⟨_⟩_` to yield a proof of `x ≡
z`.  The proof of `y ≡ z` consists of `_≡⟨_⟩_` applied to `y`, `y≡z`,
and `z ∎`.  The first argument is a term, `y`, while the second and
third arguments are both proofs of equations, in particular proofs of
`y ≡ z` and `z ≡ z` respectively, which are combined by `trans` in the
body of `_≡⟨_⟩_` to yield a proof of `y ≡ z`.  Finally, the proof of
`z ≡ z` consists of `_∎` applied to the term `z`, which yields `refl`.
After simplification, the body is equivalent to the term:

    trans x≡y (trans y≡z refl)

We could replace any use of a chain of equations by a chain of
applications of `trans`; the result would be more compact but harder
to read.  The trick behind `∎` means that a chain of equalities
simplifies to a chain of applications of `trans` than ends in `trans e
refl`, where `e` is a term that proves some equality, even though `e`
alone would do.


## Chains of equations, another example

As a second example of chains of equations, we repeat the proof that addition
is commutative.  We first repeat the definitions of naturals and addition.
We cannot import them because (as noted at the beginning of this chapter)
it would cause a conflict.
<pre class="Agda">{% raw %}<a id="8160" class="Keyword">data</a> <a id="ℕ"></a><a id="8165" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a> <a id="8167" class="Symbol">:</a> <a id="8169" class="PrimitiveType">Set</a> <a id="8173" class="Keyword">where</a>
  <a id="ℕ.zero"></a><a id="8181" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8181" class="InductiveConstructor">zero</a> <a id="8186" class="Symbol">:</a> <a id="8188" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a>
  <a id="ℕ.suc"></a><a id="8192" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8192" class="InductiveConstructor">suc</a>  <a id="8197" class="Symbol">:</a> <a id="8199" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a> <a id="8201" class="Symbol">→</a> <a id="8203" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a>

<a id="_+_"></a><a id="8206" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">_+_</a> <a id="8210" class="Symbol">:</a> <a id="8212" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a> <a id="8214" class="Symbol">→</a> <a id="8216" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a> <a id="8218" class="Symbol">→</a> <a id="8220" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a>
<a id="8222" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8181" class="InductiveConstructor">zero</a>    <a id="8230" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="8232" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8232" class="Bound">n</a>  <a id="8235" class="Symbol">=</a>  <a id="8238" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8232" class="Bound">n</a>
<a id="8240" class="Symbol">(</a><a id="8241" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8192" class="InductiveConstructor">suc</a> <a id="8245" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8245" class="Bound">m</a><a id="8246" class="Symbol">)</a> <a id="8248" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="8250" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8250" class="Bound">n</a>  <a id="8253" class="Symbol">=</a>  <a id="8256" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8192" class="InductiveConstructor">suc</a> <a id="8260" class="Symbol">(</a><a id="8261" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8245" class="Bound">m</a> <a id="8263" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="8265" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8250" class="Bound">n</a><a id="8266" class="Symbol">)</a>{% endraw %}</pre>

To save space we postulate (rather than prove in full) two lemmas.
<pre class="Agda">{% raw %}<a id="8360" class="Keyword">postulate</a>
  <a id="+-identity"></a><a id="8372" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8372" class="Postulate">+-identity</a> <a id="8383" class="Symbol">:</a> <a id="8385" class="Symbol">∀</a> <a id="8387" class="Symbol">(</a><a id="8388" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8388" class="Bound">m</a> <a id="8390" class="Symbol">:</a> <a id="8392" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a><a id="8393" class="Symbol">)</a> <a id="8395" class="Symbol">→</a> <a id="8397" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8388" class="Bound">m</a> <a id="8399" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="8401" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8181" class="InductiveConstructor">zero</a> <a id="8406" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="8408" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8388" class="Bound">m</a>
  <a id="+-suc"></a><a id="8412" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8412" class="Postulate">+-suc</a> <a id="8418" class="Symbol">:</a> <a id="8420" class="Symbol">∀</a> <a id="8422" class="Symbol">(</a><a id="8423" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8423" class="Bound">m</a> <a id="8425" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8425" class="Bound">n</a> <a id="8427" class="Symbol">:</a> <a id="8429" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a><a id="8430" class="Symbol">)</a> <a id="8432" class="Symbol">→</a> <a id="8434" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8423" class="Bound">m</a> <a id="8436" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="8438" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8192" class="InductiveConstructor">suc</a> <a id="8442" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8425" class="Bound">n</a> <a id="8444" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="8446" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8192" class="InductiveConstructor">suc</a> <a id="8450" class="Symbol">(</a><a id="8451" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8423" class="Bound">m</a> <a id="8453" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="8455" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8425" class="Bound">n</a><a id="8456" class="Symbol">)</a>{% endraw %}</pre>
This is our first use of a _postulate_.  A postulate specifies a
signature for an identifier but no definition.  Here we postulate
something proved earlier to save space.  Postulates must be used with
caution.  If we postulate something false then we could use Agda to
prove anything whatsoever.

We then repeat the proof of commutativity.
<pre class="Agda">{% raw %}<a id="+-comm"></a><a id="8822" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8822" class="Function">+-comm</a> <a id="8829" class="Symbol">:</a> <a id="8831" class="Symbol">∀</a> <a id="8833" class="Symbol">(</a><a id="8834" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8834" class="Bound">m</a> <a id="8836" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8836" class="Bound">n</a> <a id="8838" class="Symbol">:</a> <a id="8840" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a><a id="8841" class="Symbol">)</a> <a id="8843" class="Symbol">→</a> <a id="8845" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8834" class="Bound">m</a> <a id="8847" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="8849" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8836" class="Bound">n</a> <a id="8851" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="8853" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8836" class="Bound">n</a> <a id="8855" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="8857" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8834" class="Bound">m</a>
<a id="8859" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8822" class="Function">+-comm</a> <a id="8866" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8866" class="Bound">m</a> <a id="8868" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8181" class="InductiveConstructor">zero</a> <a id="8873" class="Symbol">=</a>
  <a id="8877" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5322" class="Function Operator">begin</a>
    <a id="8887" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8866" class="Bound">m</a> <a id="8889" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="8891" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8181" class="InductiveConstructor">zero</a>
  <a id="8898" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5487" class="Function Operator">≡⟨</a> <a id="8901" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8372" class="Postulate">+-identity</a> <a id="8912" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8866" class="Bound">m</a> <a id="8914" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5487" class="Function Operator">⟩</a>
    <a id="8920" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8866" class="Bound">m</a>
  <a id="8924" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5402" class="Function Operator">≡⟨⟩</a>
    <a id="8932" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8181" class="InductiveConstructor">zero</a> <a id="8937" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="8939" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8866" class="Bound">m</a>
  <a id="8943" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5602" class="Function Operator">∎</a>
<a id="8945" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8822" class="Function">+-comm</a> <a id="8952" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8952" class="Bound">m</a> <a id="8954" class="Symbol">(</a><a id="8955" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8192" class="InductiveConstructor">suc</a> <a id="8959" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8959" class="Bound">n</a><a id="8960" class="Symbol">)</a> <a id="8962" class="Symbol">=</a>
  <a id="8966" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5322" class="Function Operator">begin</a>
    <a id="8976" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8952" class="Bound">m</a> <a id="8978" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="8980" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8192" class="InductiveConstructor">suc</a> <a id="8984" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8959" class="Bound">n</a>
  <a id="8988" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5487" class="Function Operator">≡⟨</a> <a id="8991" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8412" class="Postulate">+-suc</a> <a id="8997" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8952" class="Bound">m</a> <a id="8999" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8959" class="Bound">n</a> <a id="9001" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5487" class="Function Operator">⟩</a>
    <a id="9007" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8192" class="InductiveConstructor">suc</a> <a id="9011" class="Symbol">(</a><a id="9012" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8952" class="Bound">m</a> <a id="9014" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="9016" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8959" class="Bound">n</a><a id="9017" class="Symbol">)</a>
  <a id="9021" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5487" class="Function Operator">≡⟨</a> <a id="9024" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4074" class="Function">cong</a> <a id="9029" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8192" class="InductiveConstructor">suc</a> <a id="9033" class="Symbol">(</a><a id="9034" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8822" class="Function">+-comm</a> <a id="9041" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8952" class="Bound">m</a> <a id="9043" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8959" class="Bound">n</a><a id="9044" class="Symbol">)</a> <a id="9046" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5487" class="Function Operator">⟩</a>
    <a id="9052" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8192" class="InductiveConstructor">suc</a> <a id="9056" class="Symbol">(</a><a id="9057" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8959" class="Bound">n</a> <a id="9059" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="9061" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8952" class="Bound">m</a><a id="9062" class="Symbol">)</a>
  <a id="9066" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5402" class="Function Operator">≡⟨⟩</a>
    <a id="9074" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8192" class="InductiveConstructor">suc</a> <a id="9078" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8959" class="Bound">n</a> <a id="9080" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="9082" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8952" class="Bound">m</a>
  <a id="9086" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#5602" class="Function Operator">∎</a>{% endraw %}</pre>
The reasoning here is similar to that in the
preceding section, the one addition being the use of
`_≡⟨⟩_`, which we use when no justification is required.
One can think of occurrences of `≡⟨⟩` as an equivalent
to `≡⟨ refl ⟩`.

Agda always treats a term as equivalent to its
simplified term.  The reason that one can write

      suc (n + m)
    ≡⟨⟩
      suc n + m

is because Agda treats both terms as the same.
This also means that one could instead interchange
the lines and write

      suc n + m
    ≡⟨⟩
      suc (n + m)

and Agda would not object. Agda only checks that the terms separated
by `≡⟨⟩` have the same simplified form; it's up to us to write them in
an order that will make sense to the reader.


## Rewriting

Consider a property of natural numbers, such as being even.
We repeat the earlier definition.
<pre class="Agda">{% raw %}<a id="9935" class="Keyword">data</a> <a id="even"></a><a id="9940" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9940" class="Datatype">even</a> <a id="9945" class="Symbol">:</a> <a id="9947" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a> <a id="9949" class="Symbol">→</a> <a id="9951" class="PrimitiveType">Set</a>
<a id="9955" class="Keyword">data</a> <a id="odd"></a><a id="9960" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9960" class="Datatype">odd</a>  <a id="9965" class="Symbol">:</a> <a id="9967" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a> <a id="9969" class="Symbol">→</a> <a id="9971" class="PrimitiveType">Set</a>

<a id="9976" class="Keyword">data</a> <a id="9981" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9940" class="Datatype">even</a> <a id="9986" class="Keyword">where</a>

  <a id="even.even-zero"></a><a id="9995" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9995" class="InductiveConstructor">even-zero</a> <a id="10005" class="Symbol">:</a> <a id="10007" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9940" class="Datatype">even</a> <a id="10012" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8181" class="InductiveConstructor">zero</a>

  <a id="even.even-suc"></a><a id="10020" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10020" class="InductiveConstructor">even-suc</a> <a id="10029" class="Symbol">:</a> <a id="10031" class="Symbol">∀</a> <a id="10033" class="Symbol">{</a><a id="10034" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10034" class="Bound">n</a> <a id="10036" class="Symbol">:</a> <a id="10038" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a><a id="10039" class="Symbol">}</a>
    <a id="10045" class="Symbol">→</a> <a id="10047" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9960" class="Datatype">odd</a> <a id="10051" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10034" class="Bound">n</a>
      <a id="10059" class="Comment">------------</a>
    <a id="10076" class="Symbol">→</a> <a id="10078" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9940" class="Datatype">even</a> <a id="10083" class="Symbol">(</a><a id="10084" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8192" class="InductiveConstructor">suc</a> <a id="10088" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10034" class="Bound">n</a><a id="10089" class="Symbol">)</a>

<a id="10092" class="Keyword">data</a> <a id="10097" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9960" class="Datatype">odd</a> <a id="10101" class="Keyword">where</a>
  <a id="odd.odd-suc"></a><a id="10109" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10109" class="InductiveConstructor">odd-suc</a> <a id="10117" class="Symbol">:</a> <a id="10119" class="Symbol">∀</a> <a id="10121" class="Symbol">{</a><a id="10122" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10122" class="Bound">n</a> <a id="10124" class="Symbol">:</a> <a id="10126" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a><a id="10127" class="Symbol">}</a>
    <a id="10133" class="Symbol">→</a> <a id="10135" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9940" class="Datatype">even</a> <a id="10140" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10122" class="Bound">n</a>
      <a id="10148" class="Comment">-----------</a>
    <a id="10164" class="Symbol">→</a> <a id="10166" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9960" class="Datatype">odd</a> <a id="10170" class="Symbol">(</a><a id="10171" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8192" class="InductiveConstructor">suc</a> <a id="10175" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10122" class="Bound">n</a><a id="10176" class="Symbol">)</a>{% endraw %}</pre>
In the previous section, we proved addition is commutative.  Given
evidence that `even (m + n)` holds, we ought also to be able to take
that as evidence that `even (n + m)` holds.

Agda includes special notation to support just this kind of reasoning.
To enable this notation, we use pragmas to tell Agda which type
corresponds to equivalence.
<pre class="Agda">{% raw %}<a id="10546" class="Symbol">{-#</a> <a id="10550" class="Keyword">BUILTIN</a> EQUALITY <a id="10567" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">_≡_</a> <a id="10571" class="Symbol">#-}</a>{% endraw %}</pre>

We can then prove the desired property as follows.
<pre class="Agda">{% raw %}<a id="even-comm"></a><a id="10651" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10651" class="Function">even-comm</a> <a id="10661" class="Symbol">:</a> <a id="10663" class="Symbol">∀</a> <a id="10665" class="Symbol">(</a><a id="10666" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10666" class="Bound">m</a> <a id="10668" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10668" class="Bound">n</a> <a id="10670" class="Symbol">:</a> <a id="10672" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a><a id="10673" class="Symbol">)</a>
  <a id="10677" class="Symbol">→</a> <a id="10679" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9940" class="Datatype">even</a> <a id="10684" class="Symbol">(</a><a id="10685" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10666" class="Bound">m</a> <a id="10687" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="10689" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10668" class="Bound">n</a><a id="10690" class="Symbol">)</a>
    <a id="10696" class="Comment">------------</a>
  <a id="10711" class="Symbol">→</a> <a id="10713" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9940" class="Datatype">even</a> <a id="10718" class="Symbol">(</a><a id="10719" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10668" class="Bound">n</a> <a id="10721" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="10723" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10666" class="Bound">m</a><a id="10724" class="Symbol">)</a>
<a id="10726" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10651" class="Function">even-comm</a> <a id="10736" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10736" class="Bound">m</a> <a id="10738" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10738" class="Bound">n</a> <a id="10740" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10740" class="Bound">ev</a>  <a id="10744" class="Keyword">rewrite</a> <a id="10752" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8822" class="Function">+-comm</a> <a id="10759" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10736" class="Bound">m</a> <a id="10761" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10738" class="Bound">n</a>  <a id="10764" class="Symbol">=</a>  <a id="10767" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#10740" class="Bound">ev</a>{% endraw %}</pre>
Here `ev` ranges over evidence that `even (m + n)` holds, and we show
that it is also provides evidence that `even (n + m)` holds.  In
general, the keyword `rewrite` is followed by evidence of an
equivalence, and that equivalence is used to rewrite the type of the
goal and of any variable in scope.

It is instructive to develop `even-comm` interactively.  To start, we
supply variables for the arguments on the left, and a hole for the
body on the right:

    even-comm : ∀ (m n : ℕ)
      → even (m + n)
        ------------
      → even (n + m)
    even-comm m n ev = {! !}

If we go into the hole and type `C-c C-,` then Agda reports:

    Goal: even (n + m)
    ————————————————————————————————————————————————————————————
    ev : even (m + n)
    n  : ℕ
    m  : ℕ

Now we add the rewrite.

    even-comm : ∀ (m n : ℕ)
      → even (m + n)
        ------------
      → even (n + m)
    even-comm m n ev rewrite +-comm m n = {! !}

If we go into the hole again and type `C-c C-,` then Agda now reports:

    Goal: even (n + m)
    ————————————————————————————————————————————————————————————
    ev : even (n + m)
    n  : ℕ
    m  : ℕ

The arguments have been swapped in the goal.  Now it is trivial to see
that `ev` satisfies the goal, and typing `C-c C-a` in the hole causes
it to be filled with `ev`.


## Multiple rewrites

One may perform multiple rewrites, each separated by a vertical bar.  For instance,
here is a second proof that addition is commutative, relying on rewrites rather
than chains of equalities.
<pre class="Agda">{% raw %}<a id="+-comm′"></a><a id="12321" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12321" class="Function">+-comm′</a> <a id="12329" class="Symbol">:</a> <a id="12331" class="Symbol">∀</a> <a id="12333" class="Symbol">(</a><a id="12334" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12334" class="Bound">m</a> <a id="12336" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12336" class="Bound">n</a> <a id="12338" class="Symbol">:</a> <a id="12340" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a><a id="12341" class="Symbol">)</a> <a id="12343" class="Symbol">→</a> <a id="12345" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12334" class="Bound">m</a> <a id="12347" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="12349" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12336" class="Bound">n</a> <a id="12351" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="12353" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12336" class="Bound">n</a> <a id="12355" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="12357" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12334" class="Bound">m</a>
<a id="12359" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12321" class="Function">+-comm′</a> <a id="12367" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8181" class="InductiveConstructor">zero</a>    <a id="12375" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12375" class="Bound">n</a>  <a id="12378" class="Keyword">rewrite</a> <a id="12386" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8372" class="Postulate">+-identity</a> <a id="12397" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12375" class="Bound">n</a>            <a id="12410" class="Symbol">=</a>  <a id="12413" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a>
<a id="12418" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12321" class="Function">+-comm′</a> <a id="12426" class="Symbol">(</a><a id="12427" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8192" class="InductiveConstructor">suc</a> <a id="12431" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12431" class="Bound">m</a><a id="12432" class="Symbol">)</a> <a id="12434" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12434" class="Bound">n</a>  <a id="12437" class="Keyword">rewrite</a> <a id="12445" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8412" class="Postulate">+-suc</a> <a id="12451" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12434" class="Bound">n</a> <a id="12453" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12431" class="Bound">m</a> <a id="12455" class="Symbol">|</a> <a id="12457" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8822" class="Function">+-comm</a> <a id="12464" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12431" class="Bound">m</a> <a id="12466" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#12434" class="Bound">n</a>  <a id="12469" class="Symbol">=</a>  <a id="12472" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a>{% endraw %}</pre>
This is far more compact.  Among other things, whereas the previous
proof required `cong suc (+-comm m n)` as the justification to invoke
the inductive hypothesis, here it is sufficient to rewrite with
`+-comm m n`, as rewriting automatically takes congruence into
account.  Although proofs with rewriting are shorter, proofs as chains
of equalities are easier to follow, and we will stick with the latter
when feasible.


## Rewriting expanded

The `rewrite` notation is in fact shorthand for an appropriate use of `with`
abstraction.
<pre class="Agda">{% raw %}<a id="even-comm′"></a><a id="13037" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#13037" class="Function">even-comm′</a> <a id="13048" class="Symbol">:</a> <a id="13050" class="Symbol">∀</a> <a id="13052" class="Symbol">(</a><a id="13053" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#13053" class="Bound">m</a> <a id="13055" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#13055" class="Bound">n</a> <a id="13057" class="Symbol">:</a> <a id="13059" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a><a id="13060" class="Symbol">)</a>
  <a id="13064" class="Symbol">→</a> <a id="13066" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9940" class="Datatype">even</a> <a id="13071" class="Symbol">(</a><a id="13072" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#13053" class="Bound">m</a> <a id="13074" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="13076" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#13055" class="Bound">n</a><a id="13077" class="Symbol">)</a>
    <a id="13083" class="Comment">------------</a>
  <a id="13098" class="Symbol">→</a> <a id="13100" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9940" class="Datatype">even</a> <a id="13105" class="Symbol">(</a><a id="13106" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#13055" class="Bound">n</a> <a id="13108" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="13110" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#13053" class="Bound">m</a><a id="13111" class="Symbol">)</a>
<a id="13113" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#13037" class="Function">even-comm′</a> <a id="13124" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#13124" class="Bound">m</a> <a id="13126" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#13126" class="Bound">n</a> <a id="13128" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#13128" class="Bound">ev</a> <a id="13131" class="Keyword">with</a>   <a id="13138" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#13124" class="Bound">m</a> <a id="13140" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="13142" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#13126" class="Bound">n</a>  <a id="13145" class="Symbol">|</a> <a id="13147" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8822" class="Function">+-comm</a> <a id="13154" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#13124" class="Bound">m</a> <a id="13156" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#13126" class="Bound">n</a>
<a id="13158" class="Symbol">...</a>                  <a id="13179" class="Symbol">|</a> <a id="13181" class="DottedPattern Symbol">.(</a><a id="13183" class="DottedPattern Bound">n</a> <a id="13185" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="DottedPattern Function Operator">+</a> <a id="13187" class="DottedPattern Bound">m</a><a id="13188" class="DottedPattern Symbol">)</a> <a id="13190" class="Symbol">|</a> <a id="13192" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a>        <a id="13204" class="Symbol">=</a> <a id="13206" class="Bound">ev</a>{% endraw %}</pre>
The first clause asserts that `m + n` and `n + m` are identical, and
the second clause justifies that assertion with evidence of the
appropriate equivalence.  Note the use of the _dot pattern_, `.(n +
m)`.  A dot pattern consists of a dot followed by an expression, and
is used when other information forces the value matched to be equal to
the value of the expression in the dot pattern.  In this case, the
identification of `m + n` and `n + m` is justified by the subsequent
matching of `+-comm m n` against `refl`.  One might think that the
first clause is redundant as the information is inherent in the second
clause, but in fact Agda is rather picky on this point: omitting the
first clause or reversing the order of the clauses will cause Agda to
report an error.  (Try it and see!)

In this case, we can avoid rewrite by simply applying substitution.
<pre class="Agda">{% raw %}<a id="even-comm″"></a><a id="14092" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#14092" class="Function">even-comm″</a> <a id="14103" class="Symbol">:</a> <a id="14105" class="Symbol">∀</a> <a id="14107" class="Symbol">(</a><a id="14108" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#14108" class="Bound">m</a> <a id="14110" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#14110" class="Bound">n</a> <a id="14112" class="Symbol">:</a> <a id="14114" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8165" class="Datatype">ℕ</a><a id="14115" class="Symbol">)</a>
  <a id="14119" class="Symbol">→</a> <a id="14121" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9940" class="Datatype">even</a> <a id="14126" class="Symbol">(</a><a id="14127" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#14108" class="Bound">m</a> <a id="14129" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="14131" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#14110" class="Bound">n</a><a id="14132" class="Symbol">)</a>
    <a id="14138" class="Comment">------------</a>
  <a id="14153" class="Symbol">→</a> <a id="14155" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9940" class="Datatype">even</a> <a id="14160" class="Symbol">(</a><a id="14161" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#14110" class="Bound">n</a> <a id="14163" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8206" class="Function Operator">+</a> <a id="14165" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#14108" class="Bound">m</a><a id="14166" class="Symbol">)</a>
<a id="14168" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#14092" class="Function">even-comm″</a> <a id="14179" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#14179" class="Bound">m</a> <a id="14181" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#14181" class="Bound">n</a>  <a id="14184" class="Symbol">=</a>  <a id="14187" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4864" class="Function">subst</a> <a id="14193" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#9940" class="Datatype">even</a> <a id="14198" class="Symbol">(</a><a id="14199" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#8822" class="Function">+-comm</a> <a id="14206" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#14179" class="Bound">m</a> <a id="14208" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#14181" class="Bound">n</a><a id="14209" class="Symbol">)</a>{% endraw %}</pre>
Nonetheless, rewrite is a vital part of the Agda toolkit,
as earlier examples have shown.


## Leibniz equality

The form of asserting equivalence that we have used is due to Martin
Löf, and was published in 1975.  An older form is due to Leibniz, and
was published in 1686.  Leibniz asserted the _identity of
indiscernibles_: two objects are equal if and only if they satisfy the
same properties. This principle sometimes goes by the name Leibniz'
Law, and is closely related to Spock's Law, "A difference that makes
no difference is no difference".  Here we define Leibniz equality,
and show that two terms satisfy Leibniz equality if and only if they
satisfy Martin Löf equivalence.

Leibniz equality is usually formalised to state that `x ≐ y`
holds if every property `P` that holds of `x` also holds of
`y`.  Perhaps surprisingly, this definition is
sufficient to also ensure the converse, that every property `P` that
holds of `y` also holds of `x`.

Let `x` and `y` be objects of type `A`. We say that `x ≐ y` holds if
for every predicate `P` over type `A` we have that `P x` implies `P y`.
<pre class="Agda">{% raw %}<a id="_≐_"></a><a id="15333" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15333" class="Function Operator">_≐_</a> <a id="15337" class="Symbol">:</a> <a id="15339" class="Symbol">∀</a> <a id="15341" class="Symbol">{</a><a id="15342" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15342" class="Bound">A</a> <a id="15344" class="Symbol">:</a> <a id="15346" class="PrimitiveType">Set</a><a id="15349" class="Symbol">}</a> <a id="15351" class="Symbol">(</a><a id="15352" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15352" class="Bound">x</a> <a id="15354" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15354" class="Bound">y</a> <a id="15356" class="Symbol">:</a> <a id="15358" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15342" class="Bound">A</a><a id="15359" class="Symbol">)</a> <a id="15361" class="Symbol">→</a> <a id="15363" class="PrimitiveType">Set₁</a>
<a id="15368" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15333" class="Function Operator">_≐_</a> <a id="15372" class="Symbol">{</a><a id="15373" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15373" class="Bound">A</a><a id="15374" class="Symbol">}</a> <a id="15376" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15376" class="Bound">x</a> <a id="15378" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15378" class="Bound">y</a> <a id="15380" class="Symbol">=</a> <a id="15382" class="Symbol">∀</a> <a id="15384" class="Symbol">(</a><a id="15385" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15385" class="Bound">P</a> <a id="15387" class="Symbol">:</a> <a id="15389" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15373" class="Bound">A</a> <a id="15391" class="Symbol">→</a> <a id="15393" class="PrimitiveType">Set</a><a id="15396" class="Symbol">)</a> <a id="15398" class="Symbol">→</a> <a id="15400" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15385" class="Bound">P</a> <a id="15402" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15376" class="Bound">x</a> <a id="15404" class="Symbol">→</a> <a id="15406" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15385" class="Bound">P</a> <a id="15408" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15378" class="Bound">y</a>{% endraw %}</pre>
We cannot write the left-hand side of the equation as `x ≐ y`,
and instead we write `_≐_ {A} x y` to provide access to the implicit
parameter `A` which appears on the right-hand side.

This is our first use of _levels_.  We cannot assign `Set` the type
`Set`, since this would lead to contradictions such as Russel's
Paradox and Girard's Paradox.  Instead, there is a hierarchy of types,
where `Set : Set₁`, `Set₁ : Set₂`, and so on.  In fact, `Set` itself
is just an abbreviation for `Set₀`.  Since the equation defining `_≐_`
mentions `Set` on the right-hand side, the corresponding signature
must use `Set₁`.  We say a bit more about levels below.

Leibniz equality is reflexive and transitive,
where the first follows by a variant of the identity function
and the second by a variant of function composition.
<pre class="Agda">{% raw %}<a id="refl-≐"></a><a id="16247" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16247" class="Function">refl-≐</a> <a id="16254" class="Symbol">:</a> <a id="16256" class="Symbol">∀</a> <a id="16258" class="Symbol">{</a><a id="16259" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16259" class="Bound">A</a> <a id="16261" class="Symbol">:</a> <a id="16263" class="PrimitiveType">Set</a><a id="16266" class="Symbol">}</a> <a id="16268" class="Symbol">{</a><a id="16269" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16269" class="Bound">x</a> <a id="16271" class="Symbol">:</a> <a id="16273" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16259" class="Bound">A</a><a id="16274" class="Symbol">}</a>
  <a id="16278" class="Symbol">→</a> <a id="16280" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16269" class="Bound">x</a> <a id="16282" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15333" class="Function Operator">≐</a> <a id="16284" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16269" class="Bound">x</a>
<a id="16286" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16247" class="Function">refl-≐</a> <a id="16293" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16293" class="Bound">P</a> <a id="16295" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16295" class="Bound">Px</a>  <a id="16299" class="Symbol">=</a>  <a id="16302" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16295" class="Bound">Px</a>

<a id="trans-≐"></a><a id="16306" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16306" class="Function">trans-≐</a> <a id="16314" class="Symbol">:</a> <a id="16316" class="Symbol">∀</a> <a id="16318" class="Symbol">{</a><a id="16319" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16319" class="Bound">A</a> <a id="16321" class="Symbol">:</a> <a id="16323" class="PrimitiveType">Set</a><a id="16326" class="Symbol">}</a> <a id="16328" class="Symbol">{</a><a id="16329" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16329" class="Bound">x</a> <a id="16331" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16331" class="Bound">y</a> <a id="16333" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16333" class="Bound">z</a> <a id="16335" class="Symbol">:</a> <a id="16337" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16319" class="Bound">A</a><a id="16338" class="Symbol">}</a>
  <a id="16342" class="Symbol">→</a> <a id="16344" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16329" class="Bound">x</a> <a id="16346" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15333" class="Function Operator">≐</a> <a id="16348" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16331" class="Bound">y</a>
  <a id="16352" class="Symbol">→</a> <a id="16354" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16331" class="Bound">y</a> <a id="16356" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15333" class="Function Operator">≐</a> <a id="16358" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16333" class="Bound">z</a>
    <a id="16364" class="Comment">-----</a>
  <a id="16372" class="Symbol">→</a> <a id="16374" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16329" class="Bound">x</a> <a id="16376" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15333" class="Function Operator">≐</a> <a id="16378" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16333" class="Bound">z</a>
<a id="16380" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16306" class="Function">trans-≐</a> <a id="16388" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16388" class="Bound">x≐y</a> <a id="16392" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16392" class="Bound">y≐z</a> <a id="16396" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16396" class="Bound">P</a> <a id="16398" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16398" class="Bound">Px</a>  <a id="16402" class="Symbol">=</a>  <a id="16405" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16392" class="Bound">y≐z</a> <a id="16409" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16396" class="Bound">P</a> <a id="16411" class="Symbol">(</a><a id="16412" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16388" class="Bound">x≐y</a> <a id="16416" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16396" class="Bound">P</a> <a id="16418" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16398" class="Bound">Px</a><a id="16420" class="Symbol">)</a>{% endraw %}</pre>

Symmetry is less obvious.  We have to show that if `P x` implies `P y`
for all predicates `P`, then the implication holds the other way round
as well.  Given a specific `P` and a proof `Py` of `P y`, we have to
construct a proof of `P x` given `x ≐ y`.  To do so, we instantiate
the equality with a predicate `Q` such that `Q z` holds if `P z`
implies `P x`.  The property `Q x` is trivial by reflexivity, and
hence `Q y` follows from `x ≐ y`.  But `Q y` is exactly a proof of
what we require, that `P y` implies `P x`.
<pre class="Agda">{% raw %}<a id="sym-≐"></a><a id="16967" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16967" class="Function">sym-≐</a> <a id="16973" class="Symbol">:</a> <a id="16975" class="Symbol">∀</a> <a id="16977" class="Symbol">{</a><a id="16978" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16978" class="Bound">A</a> <a id="16980" class="Symbol">:</a> <a id="16982" class="PrimitiveType">Set</a><a id="16985" class="Symbol">}</a> <a id="16987" class="Symbol">{</a><a id="16988" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16988" class="Bound">x</a> <a id="16990" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16990" class="Bound">y</a> <a id="16992" class="Symbol">:</a> <a id="16994" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16978" class="Bound">A</a><a id="16995" class="Symbol">}</a>
  <a id="16999" class="Symbol">→</a> <a id="17001" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16988" class="Bound">x</a> <a id="17003" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15333" class="Function Operator">≐</a> <a id="17005" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16990" class="Bound">y</a>
    <a id="17011" class="Comment">-----</a>
  <a id="17019" class="Symbol">→</a> <a id="17021" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16990" class="Bound">y</a> <a id="17023" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15333" class="Function Operator">≐</a> <a id="17025" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16988" class="Bound">x</a>
<a id="17027" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16967" class="Function">sym-≐</a> <a id="17033" class="Symbol">{</a><a id="17034" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17034" class="Bound">A</a><a id="17035" class="Symbol">}</a> <a id="17037" class="Symbol">{</a><a id="17038" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17038" class="Bound">x</a><a id="17039" class="Symbol">}</a> <a id="17041" class="Symbol">{</a><a id="17042" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17042" class="Bound">y</a><a id="17043" class="Symbol">}</a> <a id="17045" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17045" class="Bound">x≐y</a> <a id="17049" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17049" class="Bound">P</a>  <a id="17052" class="Symbol">=</a>  <a id="17055" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17137" class="Function">Qy</a>
  <a id="17060" class="Keyword">where</a>
    <a id="17070" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17070" class="Function">Q</a> <a id="17072" class="Symbol">:</a> <a id="17074" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17034" class="Bound">A</a> <a id="17076" class="Symbol">→</a> <a id="17078" class="PrimitiveType">Set</a>
    <a id="17086" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17070" class="Function">Q</a> <a id="17088" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17088" class="Bound">z</a> <a id="17090" class="Symbol">=</a> <a id="17092" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17049" class="Bound">P</a> <a id="17094" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17088" class="Bound">z</a> <a id="17096" class="Symbol">→</a> <a id="17098" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17049" class="Bound">P</a> <a id="17100" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17038" class="Bound">x</a>
    <a id="17106" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17106" class="Function">Qx</a> <a id="17109" class="Symbol">:</a> <a id="17111" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17070" class="Function">Q</a> <a id="17113" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17038" class="Bound">x</a>
    <a id="17119" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17106" class="Function">Qx</a> <a id="17122" class="Symbol">=</a> <a id="17124" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#16247" class="Function">refl-≐</a> <a id="17131" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17049" class="Bound">P</a>
    <a id="17137" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17137" class="Function">Qy</a> <a id="17140" class="Symbol">:</a> <a id="17142" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17070" class="Function">Q</a> <a id="17144" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17042" class="Bound">y</a>
    <a id="17150" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17137" class="Function">Qy</a> <a id="17153" class="Symbol">=</a> <a id="17155" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17045" class="Bound">x≐y</a> <a id="17159" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17070" class="Function">Q</a> <a id="17161" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17106" class="Function">Qx</a>{% endraw %}</pre>

We now show that Martin Löf equivalence implies
Leibniz equality, and vice versa.  In the forward direction, if we know
`x ≡ y` we need for any `P` to take evidence of `P x` to evidence of `P y`,
which is easy since equivalence of `x` and `y` implies that any proof
of `P x` is also a proof of `P y`.
<pre class="Agda">{% raw %}<a id="≡-implies-≐"></a><a id="17490" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17490" class="Function">≡-implies-≐</a> <a id="17502" class="Symbol">:</a> <a id="17504" class="Symbol">∀</a> <a id="17506" class="Symbol">{</a><a id="17507" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17507" class="Bound">A</a> <a id="17509" class="Symbol">:</a> <a id="17511" class="PrimitiveType">Set</a><a id="17514" class="Symbol">}</a> <a id="17516" class="Symbol">{</a><a id="17517" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17517" class="Bound">x</a> <a id="17519" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17519" class="Bound">y</a> <a id="17521" class="Symbol">:</a> <a id="17523" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17507" class="Bound">A</a><a id="17524" class="Symbol">}</a>
  <a id="17528" class="Symbol">→</a> <a id="17530" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17517" class="Bound">x</a> <a id="17532" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="17534" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17519" class="Bound">y</a>
    <a id="17540" class="Comment">-----</a>
  <a id="17548" class="Symbol">→</a> <a id="17550" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17517" class="Bound">x</a> <a id="17552" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15333" class="Function Operator">≐</a> <a id="17554" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17519" class="Bound">y</a>
<a id="17556" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17490" class="Function">≡-implies-≐</a> <a id="17568" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17568" class="Bound">x≡y</a> <a id="17572" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17572" class="Bound">P</a> <a id="17574" class="Symbol">=</a> <a id="17576" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#4864" class="Function">subst</a> <a id="17582" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17572" class="Bound">P</a> <a id="17584" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#17568" class="Bound">x≡y</a>{% endraw %}</pre>
This direction follows from substitution, which we showed earlier.

In the reverse direction, given that for any `P` we can take a proof of `P x`
to a proof of `P y` we need to show `x ≡ y`. The proof is similar to that
for symmetry of Leibniz equality. We take `Q`
to be the predicate that holds of `z` if `x ≡ z`. Then `Q x` is trivial
by reflexivity of Martin Löf equivalence, and hence `Q y` follows from
`x ≐ y`.  But `Q y` is exactly a proof of what we require, that `x ≡ y`.
<pre class="Agda">{% raw %}<a id="≐-implies-≡"></a><a id="18094" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18094" class="Function">≐-implies-≡</a> <a id="18106" class="Symbol">:</a> <a id="18108" class="Symbol">∀</a> <a id="18110" class="Symbol">{</a><a id="18111" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18111" class="Bound">A</a> <a id="18113" class="Symbol">:</a> <a id="18115" class="PrimitiveType">Set</a><a id="18118" class="Symbol">}</a> <a id="18120" class="Symbol">{</a><a id="18121" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18121" class="Bound">x</a> <a id="18123" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18123" class="Bound">y</a> <a id="18125" class="Symbol">:</a> <a id="18127" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18111" class="Bound">A</a><a id="18128" class="Symbol">}</a>
  <a id="18132" class="Symbol">→</a> <a id="18134" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18121" class="Bound">x</a> <a id="18136" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#15333" class="Function Operator">≐</a> <a id="18138" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18123" class="Bound">y</a>
    <a id="18144" class="Comment">-----</a>
  <a id="18152" class="Symbol">→</a> <a id="18154" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18121" class="Bound">x</a> <a id="18156" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="18158" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18123" class="Bound">y</a>
<a id="18160" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18094" class="Function">≐-implies-≡</a> <a id="18172" class="Symbol">{</a><a id="18173" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18173" class="Bound">A</a><a id="18174" class="Symbol">}</a> <a id="18176" class="Symbol">{</a><a id="18177" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18177" class="Bound">x</a><a id="18178" class="Symbol">}</a> <a id="18180" class="Symbol">{</a><a id="18181" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18181" class="Bound">y</a><a id="18182" class="Symbol">}</a> <a id="18184" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18184" class="Bound">x≐y</a> <a id="18188" class="Symbol">=</a> <a id="18190" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18264" class="Function">Qy</a>
  <a id="18195" class="Keyword">where</a>
    <a id="18205" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18205" class="Function">Q</a> <a id="18207" class="Symbol">:</a> <a id="18209" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18173" class="Bound">A</a> <a id="18211" class="Symbol">→</a> <a id="18213" class="PrimitiveType">Set</a>
    <a id="18221" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18205" class="Function">Q</a> <a id="18223" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18223" class="Bound">z</a> <a id="18225" class="Symbol">=</a> <a id="18227" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18177" class="Bound">x</a> <a id="18229" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#683" class="Datatype Operator">≡</a> <a id="18231" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18223" class="Bound">z</a>
    <a id="18237" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18237" class="Function">Qx</a> <a id="18240" class="Symbol">:</a> <a id="18242" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18205" class="Function">Q</a> <a id="18244" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18177" class="Bound">x</a>
    <a id="18250" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18237" class="Function">Qx</a> <a id="18253" class="Symbol">=</a> <a id="18255" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#723" class="InductiveConstructor">refl</a>
    <a id="18264" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18264" class="Function">Qy</a> <a id="18267" class="Symbol">:</a> <a id="18269" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18205" class="Function">Q</a> <a id="18271" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18181" class="Bound">y</a>
    <a id="18277" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18264" class="Function">Qy</a> <a id="18280" class="Symbol">=</a> <a id="18282" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18184" class="Bound">x≐y</a> <a id="18286" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18205" class="Function">Q</a> <a id="18288" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#18237" class="Function">Qx</a>{% endraw %}</pre>

(Parts of this section are adapted from *≐≃≡: Leibniz Equality is
Isomorphic to Martin-Löf Identity, Parametrically*, by Andreas Abel,
Jesper Cockx, Dominique Devries, Andreas Nuyts, and Philip Wadler,
draft, 2017.)


## Universe polymorphism {#unipoly}

As we have seen, not every type belongs to `Set`, but instead every
type belongs somewhere in the hierarchy `Set₀`, `Set₁`, `Set₂`, and so on,
where `Set` abbreviates `Set₀`, and `Set₀ : Set₁`, `Set₁ : Set₂`, and so on.
The definition of equality given above is fine if we want to compare two
values of a type that belongs to `Set`, but what if we want to compare
two values of a type that belongs to `Set ℓ` for some arbitrary level `ℓ`?

The answer is _universe polymorphism_, where a definition is made
with respect to an arbitrary level `ℓ`. To make use of levels, we
first import the following.
<pre class="Agda">{% raw %}<a id="19171" class="Keyword">open</a> <a id="19176" class="Keyword">import</a> <a id="19183" href="https://agda.github.io/agda-stdlib/Level.html" class="Module">Level</a> <a id="19189" class="Keyword">using</a> <a id="19195" class="Symbol">(</a><a id="19196" href="https://agda.github.io/agda-stdlib/Agda.Primitive.html#408" class="Postulate">Level</a><a id="19201" class="Symbol">;</a> <a id="19203" href="https://agda.github.io/agda-stdlib/Agda.Primitive.html#657" class="Primitive Operator">_⊔_</a><a id="19206" class="Symbol">)</a> <a id="19208" class="Keyword">renaming</a> <a id="19217" class="Symbol">(</a><a id="19218" href="https://agda.github.io/agda-stdlib/Agda.Primitive.html#627" class="Primitive">suc</a> <a id="19222" class="Symbol">to</a> <a id="19225" href="https://agda.github.io/agda-stdlib/Agda.Primitive.html#627" class="Primitive">lsuc</a><a id="19229" class="Symbol">;</a> <a id="19231" href="https://agda.github.io/agda-stdlib/Agda.Primitive.html#611" class="Primitive">zero</a> <a id="19236" class="Symbol">to</a> <a id="19239" href="https://agda.github.io/agda-stdlib/Agda.Primitive.html#611" class="Primitive">lzero</a><a id="19244" class="Symbol">)</a>{% endraw %}</pre>
Levels are isomorphic to natural numbers, and have similar constructors:

    lzero : Level
    lsuc  : Level → Level

The names `Set₀`, `Set₁`, `Set₂`, and so on, are abbreviations for

    Set lzero
    Set (lsuc lzero)
    Set (lsuc (lsuc lzero))

and so on. There is also an operator

    _⊔_ : Level → Level → Level

that given two levels returns the larger of the two.

Here is the definition of equality, generalised to an arbitrary level.
<pre class="Agda">{% raw %}<a id="19717" class="Keyword">data</a> <a id="_≡′_"></a><a id="19722" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19722" class="Datatype Operator">_≡′_</a> <a id="19727" class="Symbol">{</a><a id="19728" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19728" class="Bound">ℓ</a> <a id="19730" class="Symbol">:</a> <a id="19732" href="https://agda.github.io/agda-stdlib/Agda.Primitive.html#408" class="Postulate">Level</a><a id="19737" class="Symbol">}</a> <a id="19739" class="Symbol">{</a><a id="19740" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19740" class="Bound">A</a> <a id="19742" class="Symbol">:</a> <a id="19744" class="PrimitiveType">Set</a> <a id="19748" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19728" class="Bound">ℓ</a><a id="19749" class="Symbol">}</a> <a id="19751" class="Symbol">:</a> <a id="19753" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19740" class="Bound">A</a> <a id="19755" class="Symbol">→</a> <a id="19757" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19740" class="Bound">A</a> <a id="19759" class="Symbol">→</a> <a id="19761" class="PrimitiveType">Set</a> <a id="19765" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19728" class="Bound">ℓ</a> <a id="19767" class="Keyword">where</a>
  <a id="_≡′_.refl′"></a><a id="19775" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19775" class="InductiveConstructor">refl′</a> <a id="19781" class="Symbol">:</a> <a id="19783" class="Symbol">∀</a> <a id="19785" class="Symbol">{</a><a id="19786" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19786" class="Bound">x</a> <a id="19788" class="Symbol">:</a> <a id="19790" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19740" class="Bound">A</a><a id="19791" class="Symbol">}</a> <a id="19793" class="Symbol">→</a> <a id="19795" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19786" class="Bound">x</a> <a id="19797" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19722" class="Datatype Operator">≡′</a> <a id="19800" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19786" class="Bound">x</a>{% endraw %}</pre>
Similarly, here is the generalised definition of symmetry.
<pre class="Agda">{% raw %}<a id="sym′"></a><a id="19885" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19885" class="Function">sym′</a> <a id="19890" class="Symbol">:</a> <a id="19892" class="Symbol">∀</a> <a id="19894" class="Symbol">{</a><a id="19895" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19895" class="Bound">ℓ</a> <a id="19897" class="Symbol">:</a> <a id="19899" href="https://agda.github.io/agda-stdlib/Agda.Primitive.html#408" class="Postulate">Level</a><a id="19904" class="Symbol">}</a> <a id="19906" class="Symbol">{</a><a id="19907" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19907" class="Bound">A</a> <a id="19909" class="Symbol">:</a> <a id="19911" class="PrimitiveType">Set</a> <a id="19915" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19895" class="Bound">ℓ</a><a id="19916" class="Symbol">}</a> <a id="19918" class="Symbol">{</a><a id="19919" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19919" class="Bound">x</a> <a id="19921" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19921" class="Bound">y</a> <a id="19923" class="Symbol">:</a> <a id="19925" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19907" class="Bound">A</a><a id="19926" class="Symbol">}</a> <a id="19928" class="Symbol">→</a>  <a id="19931" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19919" class="Bound">x</a> <a id="19933" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19722" class="Datatype Operator">≡′</a> <a id="19936" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19921" class="Bound">y</a> <a id="19938" class="Symbol">→</a> <a id="19940" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19921" class="Bound">y</a> <a id="19942" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19722" class="Datatype Operator">≡′</a> <a id="19945" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19919" class="Bound">x</a>
<a id="19947" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19885" class="Function">sym′</a> <a id="19952" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19775" class="InductiveConstructor">refl′</a> <a id="19958" class="Symbol">=</a> <a id="19960" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#19775" class="InductiveConstructor">refl′</a>{% endraw %}</pre>
For simplicity, we avoid universe polymorphism in the definitions given in
the text, but most definitions in the standard library, including those for
equality, are generalised to arbitrary levels as above.

Here is the generalised definition of Leibniz equality.
<pre class="Agda">{% raw %}<a id="_≐′_"></a><a id="20254" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20254" class="Function Operator">_≐′_</a> <a id="20259" class="Symbol">:</a> <a id="20261" class="Symbol">∀</a> <a id="20263" class="Symbol">{</a><a id="20264" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20264" class="Bound">ℓ</a> <a id="20266" class="Symbol">:</a> <a id="20268" href="https://agda.github.io/agda-stdlib/Agda.Primitive.html#408" class="Postulate">Level</a><a id="20273" class="Symbol">}</a> <a id="20275" class="Symbol">{</a><a id="20276" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20276" class="Bound">A</a> <a id="20278" class="Symbol">:</a> <a id="20280" class="PrimitiveType">Set</a> <a id="20284" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20264" class="Bound">ℓ</a><a id="20285" class="Symbol">}</a> <a id="20287" class="Symbol">(</a><a id="20288" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20288" class="Bound">x</a> <a id="20290" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20290" class="Bound">y</a> <a id="20292" class="Symbol">:</a> <a id="20294" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20276" class="Bound">A</a><a id="20295" class="Symbol">)</a> <a id="20297" class="Symbol">→</a> <a id="20299" class="PrimitiveType">Set</a> <a id="20303" class="Symbol">(</a><a id="20304" href="https://agda.github.io/agda-stdlib/Agda.Primitive.html#627" class="Primitive">lsuc</a> <a id="20309" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20264" class="Bound">ℓ</a><a id="20310" class="Symbol">)</a>
<a id="20312" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20254" class="Function Operator">_≐′_</a> <a id="20317" class="Symbol">{</a><a id="20318" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20318" class="Bound">ℓ</a><a id="20319" class="Symbol">}</a> <a id="20321" class="Symbol">{</a><a id="20322" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20322" class="Bound">A</a><a id="20323" class="Symbol">}</a> <a id="20325" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20325" class="Bound">x</a> <a id="20327" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20327" class="Bound">y</a> <a id="20329" class="Symbol">=</a> <a id="20331" class="Symbol">∀</a> <a id="20333" class="Symbol">(</a><a id="20334" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20334" class="Bound">P</a> <a id="20336" class="Symbol">:</a> <a id="20338" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20322" class="Bound">A</a> <a id="20340" class="Symbol">→</a> <a id="20342" class="PrimitiveType">Set</a> <a id="20346" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20318" class="Bound">ℓ</a><a id="20347" class="Symbol">)</a> <a id="20349" class="Symbol">→</a> <a id="20351" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20334" class="Bound">P</a> <a id="20353" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20325" class="Bound">x</a> <a id="20355" class="Symbol">→</a> <a id="20357" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20334" class="Bound">P</a> <a id="20359" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Equality.md %}{% raw %}#20327" class="Bound">y</a>{% endraw %}</pre>
Before the signature used `Set₁` as the type of a term that includes
`Set`, whereas here the signature uses `Set (suc ℓ)` as the type of a
term that includes `Set ℓ`.

Further information on levels can be found in
the [Agda Wiki][wiki].

[wiki]: http://wiki.portal.chalmers.se/agda/pmwiki.php?n=ReferenceManual.UniversePolymorphism


## Standard library

Definitions similar to those in this chapter can be found in the
standard library.
<pre class="Agda">{% raw %}<a id="20823" class="Comment">-- import Relation.Binary.PropositionalEquality as Eq</a>
<a id="20877" class="Comment">-- open Eq using (_≡_; refl; trans; sym; cong; cong-app; subst)</a>
<a id="20941" class="Comment">-- open Eq.≡-Reasoning using (begin_; _≡⟨⟩_; _≡⟨_⟩_; _∎)</a>{% endraw %}</pre>
Here the imports are shown as comments rather than code to avoid
collisions, as mentioned in the introduction.


## Unicode

This chapter uses the following unicode.

    ≡  U+2261  IDENTICAL TO (\==)
    ⟨  U+27E8  MATHEMATICAL LEFT ANGLE BRACKET (\<)
    ⟩  U+27E9  MATHEMATICAL RIGHT ANGLE BRACKET (\>)
    ∎  U+220E  END OF PROOF (\qed)
    ≐  U+2250  APPROACHES THE LIMIT (\.=)
    ℓ  U+2113  SCRIPT SMALL L (\ell)
