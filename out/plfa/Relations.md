---
title     : "Relations: Inductive definition of relations"
layout    : page
permalink : /Relations/
---

<pre class="Agda">{% raw %}<a id="123" class="Keyword">module</a> <a id="130" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}" class="Module">plfa.Relations</a> <a id="145" class="Keyword">where</a>{% endraw %}</pre>

After having defined operations such as addition and multiplication,
the next step is to define relations, such as _less than or equal_.

## Imports

<pre class="Agda">{% raw %}<a id="326" class="Keyword">import</a> <a id="333" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html" class="Module">Relation.Binary.PropositionalEquality</a> <a id="371" class="Symbol">as</a> <a id="374" class="Module">Eq</a>
<a id="377" class="Keyword">open</a> <a id="382" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html" class="Module">Eq</a> <a id="385" class="Keyword">using</a> <a id="391" class="Symbol">(</a><a id="392" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">_≡_</a><a id="395" class="Symbol">;</a> <a id="397" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a><a id="401" class="Symbol">;</a> <a id="403" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#1075" class="Function">cong</a><a id="407" class="Symbol">;</a> <a id="409" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.Core.html#560" class="Function">sym</a><a id="412" class="Symbol">)</a>
<a id="414" class="Keyword">open</a> <a id="419" class="Keyword">import</a> <a id="426" href="https://agda.github.io/agda-stdlib/Data.Nat.html" class="Module">Data.Nat</a> <a id="435" class="Keyword">using</a> <a id="441" class="Symbol">(</a><a id="442" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="443" class="Symbol">;</a> <a id="445" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a><a id="449" class="Symbol">;</a> <a id="451" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a><a id="454" class="Symbol">;</a> <a id="456" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">_+_</a><a id="459" class="Symbol">;</a> <a id="461" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#433" class="Primitive Operator">_*_</a><a id="464" class="Symbol">;</a> <a id="466" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#320" class="Primitive Operator">_∸_</a><a id="469" class="Symbol">)</a>
<a id="471" class="Keyword">open</a> <a id="476" class="Keyword">import</a> <a id="483" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html" class="Module">Data.Nat.Properties</a> <a id="503" class="Keyword">using</a> <a id="509" class="Symbol">(</a><a id="510" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#8115" class="Function">+-comm</a><a id="516" class="Symbol">;</a> <a id="518" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#7679" class="Function">+-suc</a><a id="523" class="Symbol">)</a>{% endraw %}</pre>


## Defining relations

The relation _less than or equal_ has an infinite number of
instances.  Here are a few of them:

    0 ≤ 0     0 ≤ 1     0 ≤ 2     0 ≤ 3     ...
              1 ≤ 1     1 ≤ 2     1 ≤ 3     ...
                        2 ≤ 2     2 ≤ 3     ...
                                  3 ≤ 3     ...
                                            ...

And yet, we can write a finite definition that encompasses
all of these instances in just a few lines.  Here is the
definition as a pair of inference rules:

    z≤n --------
        zero ≤ n

        m ≤ n
    s≤s -------------
        suc m ≤ suc n

And here is the definition in Agda:
<pre class="Agda">{% raw %}<a id="1200" class="Keyword">data</a> <a id="_≤_"></a><a id="1205" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">_≤_</a> <a id="1209" class="Symbol">:</a> <a id="1211" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="1213" class="Symbol">→</a> <a id="1215" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="1217" class="Symbol">→</a> <a id="1219" class="PrimitiveType">Set</a> <a id="1223" class="Keyword">where</a>

  <a id="_≤_.z≤n"></a><a id="1232" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a> <a id="1236" class="Symbol">:</a> <a id="1238" class="Symbol">∀</a> <a id="1240" class="Symbol">{</a><a id="1241" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1241" class="Bound">n</a> <a id="1243" class="Symbol">:</a> <a id="1245" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="1246" class="Symbol">}</a>
      <a id="1254" class="Comment">--------</a>
    <a id="1267" class="Symbol">→</a> <a id="1269" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="1274" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="1276" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1241" class="Bound">n</a>

  <a id="_≤_.s≤s"></a><a id="1281" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="1285" class="Symbol">:</a> <a id="1287" class="Symbol">∀</a> <a id="1289" class="Symbol">{</a><a id="1290" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1290" class="Bound">m</a> <a id="1292" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1292" class="Bound">n</a> <a id="1294" class="Symbol">:</a> <a id="1296" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="1297" class="Symbol">}</a>
    <a id="1303" class="Symbol">→</a> <a id="1305" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1290" class="Bound">m</a> <a id="1307" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="1309" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1292" class="Bound">n</a>
      <a id="1317" class="Comment">-------------</a>
    <a id="1335" class="Symbol">→</a> <a id="1337" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="1341" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1290" class="Bound">m</a> <a id="1343" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="1345" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="1349" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1292" class="Bound">n</a>{% endraw %}</pre>
Here `z≤n` and `s≤s` (with no spaces) are constructor names, while
`zero ≤ m`, and `m ≤ n` and `suc m ≤ suc n` (with spaces) are types.
This is our first use of an _indexed_ datatype, where the type `m ≤ n`
is indexed by two naturals, `m` and `n`.  In Agda any line beginning
with two or more dashes is a comment, and here we have exploited that
convention to write our Agda code in a form that resembles the
corresponding inference rules, a trick we will use often from now on.

Both definitions above tell us the same two things:

* _Base case_: for all naturals `n`, the proposition `zero ≤ n` holds
* _Inductive case_: for all naturals `m` and `n`, if the proposition
  `m ≤ n` holds, then the proposition `suc m ≤ suc n` holds.

In fact, they each give us a bit more detail:

* _Base case_: for all naturals `n`, the constructor `z≤n`
  produces evidence that `zero ≤ n` holds.
* _Inductive case_: for all naturals `m` and `n`, the constructor
  `s≤s` takes evidence that `m ≤ n` holds into evidence that
  `suc m ≤ suc n` holds.

For example, here in inference rule notation is the proof that
`2 ≤ 4`.

      z≤n -----
          0 ≤ 2
     s≤s -------
          1 ≤ 3
    s≤s ---------
          2 ≤ 4

And here is the corresponding Agda proof.
<pre class="Agda">{% raw %}<a id="2626" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#2626" class="Function">_</a> <a id="2628" class="Symbol">:</a> <a id="2630" class="Number">2</a> <a id="2632" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="2634" class="Number">4</a>
<a id="2636" class="Symbol">_</a> <a id="2638" class="Symbol">=</a> <a id="2640" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="2644" class="Symbol">(</a><a id="2645" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="2649" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a><a id="2652" class="Symbol">)</a>{% endraw %}</pre>


## Implicit arguments

This is our first use of implicit arguments.  In the definition of
inequality, the two lines defining the constructors use `∀`, very
similar to our use of `∀` in propositions such as:

    +-comm : ∀ (m n : ℕ) → m + n ≡ n + m

However, here the declarations are surrounded by curly braces `{ }`
rather than parentheses `( )`.  This means that the arguments are
_implicit_ and need not be written explicitly; instead, they are
_inferred_ by Agda's typechecker. Thus, we write `+-comm m n` for the
proof that `m + n ≡ n + m`, but `z≤n` for the proof that `zero ≤ n`,
leaving `n` implicit.  Similarly, if `m≤n` is evidence that `m ≤ n`,
we write `s≤s m≤n` for evidence that `suc m ≤ suc n`, leaving both `m`
and `n` implicit.

If we wish, it is possible to provide implicit arguments explicitly by
writing the arguments inside curly braces.  For instance, here is the
Agda proof that `2 ≤ 4` repeated, with the implicit arguments made
explicit.
<pre class="Agda">{% raw %}<a id="3645" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#3645" class="Function">_</a> <a id="3647" class="Symbol">:</a> <a id="3649" class="Number">2</a> <a id="3651" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="3653" class="Number">4</a>
<a id="3655" class="Symbol">_</a> <a id="3657" class="Symbol">=</a> <a id="3659" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="3663" class="Symbol">{</a><a id="3664" class="Number">1</a><a id="3665" class="Symbol">}</a> <a id="3667" class="Symbol">{</a><a id="3668" class="Number">3</a><a id="3669" class="Symbol">}</a> <a id="3671" class="Symbol">(</a><a id="3672" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="3676" class="Symbol">{</a><a id="3677" class="Number">0</a><a id="3678" class="Symbol">}</a> <a id="3680" class="Symbol">{</a><a id="3681" class="Number">2</a><a id="3682" class="Symbol">}</a> <a id="3684" class="Symbol">(</a><a id="3685" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a> <a id="3689" class="Symbol">{</a><a id="3690" class="Number">2</a><a id="3691" class="Symbol">}))</a>{% endraw %}</pre>
One may also identify implicit arguments by name.
<pre class="Agda">{% raw %}<a id="3769" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#3769" class="Function">_</a> <a id="3771" class="Symbol">:</a> <a id="3773" class="Number">2</a> <a id="3775" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="3777" class="Number">4</a>
<a id="3779" class="Symbol">_</a> <a id="3781" class="Symbol">=</a> <a id="3783" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="3787" class="Symbol">{</a><a id="3788" class="Argument">m</a> <a id="3790" class="Symbol">=</a> <a id="3792" class="Number">1</a><a id="3793" class="Symbol">}</a> <a id="3795" class="Symbol">{</a><a id="3796" class="Argument">n</a> <a id="3798" class="Symbol">=</a> <a id="3800" class="Number">3</a><a id="3801" class="Symbol">}</a> <a id="3803" class="Symbol">(</a><a id="3804" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="3808" class="Symbol">{</a><a id="3809" class="Argument">m</a> <a id="3811" class="Symbol">=</a> <a id="3813" class="Number">0</a><a id="3814" class="Symbol">}</a> <a id="3816" class="Symbol">{</a><a id="3817" class="Argument">n</a> <a id="3819" class="Symbol">=</a> <a id="3821" class="Number">2</a><a id="3822" class="Symbol">}</a> <a id="3824" class="Symbol">(</a><a id="3825" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a> <a id="3829" class="Symbol">{</a><a id="3830" class="Argument">n</a> <a id="3832" class="Symbol">=</a> <a id="3834" class="Number">2</a><a id="3835" class="Symbol">}))</a>{% endraw %}</pre>
In the latter format, you may only supply some implicit arguments.
<pre class="Agda">{% raw %}<a id="3930" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#3930" class="Function">_</a> <a id="3932" class="Symbol">:</a> <a id="3934" class="Number">2</a> <a id="3936" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="3938" class="Number">4</a>
<a id="3940" class="Symbol">_</a> <a id="3942" class="Symbol">=</a> <a id="3944" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="3948" class="Symbol">{</a><a id="3949" class="Argument">n</a> <a id="3951" class="Symbol">=</a> <a id="3953" class="Number">3</a><a id="3954" class="Symbol">}</a> <a id="3956" class="Symbol">(</a><a id="3957" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="3961" class="Symbol">{</a><a id="3962" class="Argument">n</a> <a id="3964" class="Symbol">=</a> <a id="3966" class="Number">2</a><a id="3967" class="Symbol">}</a> <a id="3969" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a><a id="3972" class="Symbol">)</a>{% endraw %}</pre>
It is not permitted to swap implicit arguments, even when named.


## Precedence

We declare the precedence for comparison as follows.
<pre class="Agda">{% raw %}<a id="4133" class="Keyword">infix</a> <a id="4139" class="Number">4</a> <a id="4141" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">_≤_</a>{% endraw %}</pre>
We set the precedence of `_≤_` at level 4, so it binds less tightly
that `_+_` at level 6 and hence `1 + 2 ≤ 3` parses as `(1 + 2) ≤ 3`.
We write `infix` to indicate that the operator does not associate to
either the left or right, as it makes no sense to parse `1 ≤ 2 ≤ 3` as
either `(1 ≤ 2) ≤ 3` or `1 ≤ (2 ≤ 3)`.


## Decidability

Given two numbers, it is straightforward to compute whether or not the
first is less than or equal to the second.  We don't give the code for
doing so here, but will return to this point in
Chapter [Decidable]({{ site.baseurl }}{% link out/plfa/Decidable.md %}).


## Properties of ordering relations

Relations pop up all the time, and mathematicians have agreed
on names for some of the most common properties.

* _Reflexive_ For all `n`, the relation `n ≤ n` holds.
* _Transitive_ For all `m`, `n`, and `p`, if `m ≤ n` and
`n ≤ p` hold, then `m ≤ p` holds.
* _Anti-symmetric_ For all `m` and `n`, if both `m ≤ n` and
`n ≤ m` hold, then `m ≡ n` holds.
* _Total_ For all `m` and `n`, either `m ≤ n` or `n ≤ m`
holds.

The relation `_≤_` satisfies all four of these properties.

There are also names for some combinations of these properties.

* _Preorder_ Any relation that is reflexive and transitive.
* _Partial order_ Any preorder that is also anti-symmetric.
* _Total order_ Any partial order that is also total.

If you ever bump into a relation at a party, you now know how
to make small talk, by asking it whether it is reflexive, transitive,
anti-symmetric, and total. Or instead you might ask whether it is a
preorder, partial order, or total order.

Less frivolously, if you ever bump into a relation while reading a
technical paper, this gives you a way to orient yourself, by checking
whether or not it is a preorder, partial order, or total order.  A
careful author will often call out these properties---or their
lack---for instance by saying that a newly introduced relation is a
a partial order but not a total order.


#### Exercise (`orderings`).

Give an example of a preorder that is not a partial order.

Give an example of a partial order that is not a preorder.


## Reflexivity

The first property to prove about comparison is that it is reflexive:
for any natural `n`, the relation `n ≤ n` holds.  We follow the
convention in the standard library and make the argument implicit,
as that will make it easier to invoke reflection.
<pre class="Agda">{% raw %}<a id="≤-refl"></a><a id="6559" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6559" class="Function">≤-refl</a> <a id="6566" class="Symbol">:</a> <a id="6568" class="Symbol">∀</a> <a id="6570" class="Symbol">{</a><a id="6571" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6571" class="Bound">n</a> <a id="6573" class="Symbol">:</a> <a id="6575" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="6576" class="Symbol">}</a>
    <a id="6582" class="Comment">-----</a>
  <a id="6590" class="Symbol">→</a> <a id="6592" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6571" class="Bound">n</a> <a id="6594" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="6596" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6571" class="Bound">n</a>
<a id="6598" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6559" class="Function">≤-refl</a> <a id="6605" class="Symbol">{</a><a id="6606" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a><a id="6610" class="Symbol">}</a>   <a id="6614" class="Symbol">=</a>  <a id="6617" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a>
<a id="6621" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6559" class="Function">≤-refl</a> <a id="6628" class="Symbol">{</a><a id="6629" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="6633" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6633" class="Bound">n</a><a id="6634" class="Symbol">}</a>  <a id="6637" class="Symbol">=</a>  <a id="6640" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="6644" class="Symbol">(</a><a id="6645" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6559" class="Function">≤-refl</a> <a id="6652" class="Symbol">{</a><a id="6653" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6633" class="Bound">n</a><a id="6654" class="Symbol">})</a>{% endraw %}</pre>
The proof is a straightforward induction on the implicit argument `n`.
In the base case, `zero ≤ zero` holds by `z≤n`.  In the inductive
case, the inductive hypothesis `≤-refl {n}` gives us a proof of `n ≤
n`, and applying `s≤s` to that yields a proof of `suc n ≤ suc n`.

It is a good exercise to prove reflexivity interactively in Emacs,
using holes and the `C-c C-c`, `C-c C-,`, and `C-c C-r` commands.


## Transitivity

The second property to prove about comparison is that it is
transitive: for any naturals `m`, `n`, and `p`, if `m ≤ n` and `n ≤ p`
hold, then `m ≤ p` holds.  Again, `m`, `n`, and `p` are implicit.
<pre class="Agda">{% raw %}<a id="≤-trans"></a><a id="7303" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7303" class="Function">≤-trans</a> <a id="7311" class="Symbol">:</a> <a id="7313" class="Symbol">∀</a> <a id="7315" class="Symbol">{</a><a id="7316" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7316" class="Bound">m</a> <a id="7318" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7318" class="Bound">n</a> <a id="7320" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7320" class="Bound">p</a> <a id="7322" class="Symbol">:</a> <a id="7324" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="7325" class="Symbol">}</a>
  <a id="7329" class="Symbol">→</a> <a id="7331" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7316" class="Bound">m</a> <a id="7333" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="7335" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7318" class="Bound">n</a>
  <a id="7339" class="Symbol">→</a> <a id="7341" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7318" class="Bound">n</a> <a id="7343" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="7345" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7320" class="Bound">p</a>
    <a id="7351" class="Comment">-----</a>
  <a id="7359" class="Symbol">→</a> <a id="7361" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7316" class="Bound">m</a> <a id="7363" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="7365" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7320" class="Bound">p</a>
<a id="7367" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7303" class="Function">≤-trans</a> <a id="7375" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a>       <a id="7385" class="Symbol">_</a>          <a id="7396" class="Symbol">=</a>  <a id="7399" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a>
<a id="7403" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7303" class="Function">≤-trans</a> <a id="7411" class="Symbol">(</a><a id="7412" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="7416" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7416" class="Bound">m≤n</a><a id="7419" class="Symbol">)</a> <a id="7421" class="Symbol">(</a><a id="7422" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="7426" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7426" class="Bound">n≤p</a><a id="7429" class="Symbol">)</a>  <a id="7432" class="Symbol">=</a>  <a id="7435" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="7439" class="Symbol">(</a><a id="7440" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7303" class="Function">≤-trans</a> <a id="7448" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7416" class="Bound">m≤n</a> <a id="7452" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7426" class="Bound">n≤p</a><a id="7455" class="Symbol">)</a>{% endraw %}</pre>
Here the proof is by induction on the _evidence_ that `m ≤ n`.  In the
base case, the first inequality holds by `z≤n` and must show `zero ≤
p`, which follows immediately by `z≤n`.  In this case, the fact that
`n ≤ p` is irrelevant, and we write `_` as the pattern to indicate
that the corresponding evidence is unused.

In the inductive case, the first inequality holds by `s≤s m≤n`
and the second inequality by `s≤s n≤p`, and so we are given
`suc m ≤ suc n` and `suc n ≤ suc p`, and must show `suc m ≤ suc p`.
The inductive hypothesis `≤-trans m≤n n≤p` establishes
that `m ≤ p`, and our goal follows by applying `s≤s`.

The case `≤-trans (s≤s m≤n) z≤n` cannot arise, since the first
inequality implies the middle value is `suc n` while the second
inequality implies that it is `zero`.  Agda can determine that such a
case cannot arise, and does not require (or permit) it to be listed.

Alternatively, we could make the implicit parameters explicit.
<pre class="Agda">{% raw %}<a id="≤-trans′"></a><a id="8432" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8432" class="Function">≤-trans′</a> <a id="8441" class="Symbol">:</a> <a id="8443" class="Symbol">∀</a> <a id="8445" class="Symbol">(</a><a id="8446" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8446" class="Bound">m</a> <a id="8448" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8448" class="Bound">n</a> <a id="8450" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8450" class="Bound">p</a> <a id="8452" class="Symbol">:</a> <a id="8454" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="8455" class="Symbol">)</a>
  <a id="8459" class="Symbol">→</a> <a id="8461" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8446" class="Bound">m</a> <a id="8463" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="8465" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8448" class="Bound">n</a>
  <a id="8469" class="Symbol">→</a> <a id="8471" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8448" class="Bound">n</a> <a id="8473" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="8475" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8450" class="Bound">p</a>
    <a id="8481" class="Comment">-----</a>
  <a id="8489" class="Symbol">→</a> <a id="8491" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8446" class="Bound">m</a> <a id="8493" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="8495" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8450" class="Bound">p</a>
<a id="8497" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8432" class="Function">≤-trans′</a> <a id="8506" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="8514" class="Symbol">_</a>       <a id="8522" class="Symbol">_</a>       <a id="8530" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a>       <a id="8540" class="Symbol">_</a>          <a id="8551" class="Symbol">=</a>  <a id="8554" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a>
<a id="8558" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8432" class="Function">≤-trans′</a> <a id="8567" class="Symbol">(</a><a id="8568" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="8572" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8572" class="Bound">m</a><a id="8573" class="Symbol">)</a> <a id="8575" class="Symbol">(</a><a id="8576" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="8580" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8580" class="Bound">n</a><a id="8581" class="Symbol">)</a> <a id="8583" class="Symbol">(</a><a id="8584" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="8588" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8588" class="Bound">p</a><a id="8589" class="Symbol">)</a> <a id="8591" class="Symbol">(</a><a id="8592" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="8596" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8596" class="Bound">m≤n</a><a id="8599" class="Symbol">)</a> <a id="8601" class="Symbol">(</a><a id="8602" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="8606" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8606" class="Bound">n≤p</a><a id="8609" class="Symbol">)</a>  <a id="8612" class="Symbol">=</a>  <a id="8615" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="8619" class="Symbol">(</a><a id="8620" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8432" class="Function">≤-trans′</a> <a id="8629" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8572" class="Bound">m</a> <a id="8631" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8580" class="Bound">n</a> <a id="8633" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8588" class="Bound">p</a> <a id="8635" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8596" class="Bound">m≤n</a> <a id="8639" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8606" class="Bound">n≤p</a><a id="8642" class="Symbol">)</a>{% endraw %}</pre>
One might argue that this is clearer or one might argue that the extra
length obscures the essence of the proof.  We will usually opt for
shorter proofs.

The technique of induction on evidence that a property holds (e.g.,
inducting on evidence that `m ≤ n`)---rather than induction on 
values of which the property holds (e.g., inducting on `m`)---will turn
out to be immensely valuable, and one that we use often.

Again, it is a good exercise to prove transitivity interactively in Emacs,
using holes and the `C-c C-c`, `C-c C-,`, and `C-c C-r` commands.


## Anti-symmetry

The third property to prove about comparison is that it is
antisymmetric: for all naturals `m` and `n`, if both `m ≤ n` and `n ≤
m` hold, then `m ≡ n` holds.
<pre class="Agda">{% raw %}<a id="≤-antisym"></a><a id="9404" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9404" class="Function">≤-antisym</a> <a id="9414" class="Symbol">:</a> <a id="9416" class="Symbol">∀</a> <a id="9418" class="Symbol">{</a><a id="9419" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9419" class="Bound">m</a> <a id="9421" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9421" class="Bound">n</a> <a id="9423" class="Symbol">:</a> <a id="9425" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="9426" class="Symbol">}</a>
  <a id="9430" class="Symbol">→</a> <a id="9432" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9419" class="Bound">m</a> <a id="9434" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="9436" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9421" class="Bound">n</a>
  <a id="9440" class="Symbol">→</a> <a id="9442" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9421" class="Bound">n</a> <a id="9444" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="9446" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9419" class="Bound">m</a>
    <a id="9452" class="Comment">-----</a>
  <a id="9460" class="Symbol">→</a> <a id="9462" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9419" class="Bound">m</a> <a id="9464" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">≡</a> <a id="9466" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9421" class="Bound">n</a>
<a id="9468" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9404" class="Function">≤-antisym</a> <a id="9478" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a>       <a id="9488" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a>        <a id="9499" class="Symbol">=</a>  <a id="9502" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a>
<a id="9507" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9404" class="Function">≤-antisym</a> <a id="9517" class="Symbol">(</a><a id="9518" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="9522" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9522" class="Bound">m≤n</a><a id="9525" class="Symbol">)</a> <a id="9527" class="Symbol">(</a><a id="9528" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="9532" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9532" class="Bound">n≤m</a><a id="9535" class="Symbol">)</a>  <a id="9538" class="Symbol">=</a>  <a id="9541" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#1075" class="Function">cong</a> <a id="9546" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="9550" class="Symbol">(</a><a id="9551" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9404" class="Function">≤-antisym</a> <a id="9561" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9522" class="Bound">m≤n</a> <a id="9565" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9532" class="Bound">n≤m</a><a id="9568" class="Symbol">)</a>{% endraw %}</pre>
Again, the proof is by induction over the evidence that `m ≤ n`
and `n ≤ m` hold.

In the base case, both inequalities hold by `z≤n`, and so we are given
`zero ≤ zero` and `zero ≤ zero` and must show `zero ≡ zero`, which
follows by reflexivity.  (Reflexivity of equality, that is, not
reflexivity of inequality.)

In the inductive case, the first inequality holds by `s≤s m≤n` and the
second inequality holds by `s≤s n≤m`, and so we are given `suc m ≤ suc n`
and `suc n ≤ suc m` and must show `suc m ≡ suc n`.  The inductive
hypothesis `≤-antisym m≤n n≤m` establishes that `m ≡ n`, and our goal
follows by congruence.

#### Exercise (`≤-antisym-cases`)

The above proof omits cases where one argument is `z≤n` and one
argument is `s≤s`.  Why is it ok to omit them?


## Total

The fourth property to prove about comparison is that it is total:
for any naturals `m` and `n` either `m ≤ n` or `n ≤ m`, or both if
`m` and `n` are equal.

We specify what it means for inequality to be total.
<pre class="Agda">{% raw %}<a id="10582" class="Keyword">data</a> <a id="Total"></a><a id="10587" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10587" class="Datatype">Total</a> <a id="10593" class="Symbol">(</a><a id="10594" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10594" class="Bound">m</a> <a id="10596" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10596" class="Bound">n</a> <a id="10598" class="Symbol">:</a> <a id="10600" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="10601" class="Symbol">)</a> <a id="10603" class="Symbol">:</a> <a id="10605" class="PrimitiveType">Set</a> <a id="10609" class="Keyword">where</a>

  <a id="Total.forward"></a><a id="10618" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10618" class="InductiveConstructor">forward</a> <a id="10626" class="Symbol">:</a>
      <a id="10634" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10594" class="Bound">m</a> <a id="10636" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="10638" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10596" class="Bound">n</a>
      <a id="10646" class="Comment">---------</a>
    <a id="10660" class="Symbol">→</a> <a id="10662" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10587" class="Datatype">Total</a> <a id="10668" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10594" class="Bound">m</a> <a id="10670" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10596" class="Bound">n</a>

  <a id="Total.flipped"></a><a id="10675" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10675" class="InductiveConstructor">flipped</a> <a id="10683" class="Symbol">:</a>
      <a id="10691" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10596" class="Bound">n</a> <a id="10693" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="10695" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10594" class="Bound">m</a>
      <a id="10703" class="Comment">---------</a>
    <a id="10717" class="Symbol">→</a> <a id="10719" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10587" class="Datatype">Total</a> <a id="10725" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10594" class="Bound">m</a> <a id="10727" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10596" class="Bound">n</a>{% endraw %}</pre>
Evidence that `Total m n` holds is either of the form
`forward m≤n` or `flipped n≤m`, where `m≤n` and `n≤m` are
evidence of `m ≤ n` and `n ≤ m` respectively.

This is our first use of a datatype with _parameters_,
in this case `m` and `n`.  It is equivalent to the following
indexed datatype.
<pre class="Agda">{% raw %}<a id="11046" class="Keyword">data</a> <a id="Total′"></a><a id="11051" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11051" class="Datatype">Total′</a> <a id="11058" class="Symbol">:</a> <a id="11060" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="11062" class="Symbol">→</a> <a id="11064" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="11066" class="Symbol">→</a> <a id="11068" class="PrimitiveType">Set</a> <a id="11072" class="Keyword">where</a>

  <a id="Total′.forward′"></a><a id="11081" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11081" class="InductiveConstructor">forward′</a> <a id="11090" class="Symbol">:</a> <a id="11092" class="Symbol">∀</a> <a id="11094" class="Symbol">{</a><a id="11095" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11095" class="Bound">m</a> <a id="11097" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11097" class="Bound">n</a> <a id="11099" class="Symbol">:</a> <a id="11101" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="11102" class="Symbol">}</a>
    <a id="11108" class="Symbol">→</a> <a id="11110" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11095" class="Bound">m</a> <a id="11112" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="11114" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11097" class="Bound">n</a>
      <a id="11122" class="Comment">----------</a>
    <a id="11137" class="Symbol">→</a> <a id="11139" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11051" class="Datatype">Total′</a> <a id="11146" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11095" class="Bound">m</a> <a id="11148" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11097" class="Bound">n</a>

  <a id="Total′.flipped′"></a><a id="11153" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11153" class="InductiveConstructor">flipped′</a> <a id="11162" class="Symbol">:</a> <a id="11164" class="Symbol">∀</a> <a id="11166" class="Symbol">{</a><a id="11167" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11167" class="Bound">m</a> <a id="11169" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11169" class="Bound">n</a> <a id="11171" class="Symbol">:</a> <a id="11173" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="11174" class="Symbol">}</a>
    <a id="11180" class="Symbol">→</a> <a id="11182" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11169" class="Bound">n</a> <a id="11184" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="11186" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11167" class="Bound">m</a>
      <a id="11194" class="Comment">----------</a>
    <a id="11209" class="Symbol">→</a> <a id="11211" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11051" class="Datatype">Total′</a> <a id="11218" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11167" class="Bound">m</a> <a id="11220" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11169" class="Bound">n</a>{% endraw %}</pre>
Each parameter of the type translates as an implicit parameter of each
constructor.  Unlike an indexed datatype, where the indexes can vary
(as in `zero ≤ n` and `suc m ≤ suc n`), in a parameterised datatype
the parameters must always be the same (as in `Total m n`).
Parameterised declarations are shorter, easier to read, and
occcasionally aid Agda's termination checker, so we will use them in
preference to indexed types when possible.

With that preliminary out of the way, we specify and prove totality.
<pre class="Agda">{% raw %}<a id="≤-total"></a><a id="11756" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11756" class="Function">≤-total</a> <a id="11764" class="Symbol">:</a> <a id="11766" class="Symbol">∀</a> <a id="11768" class="Symbol">(</a><a id="11769" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11769" class="Bound">m</a> <a id="11771" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11771" class="Bound">n</a> <a id="11773" class="Symbol">:</a> <a id="11775" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="11776" class="Symbol">)</a> <a id="11778" class="Symbol">→</a> <a id="11780" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10587" class="Datatype">Total</a> <a id="11786" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11769" class="Bound">m</a> <a id="11788" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11771" class="Bound">n</a>
<a id="11790" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11756" class="Function">≤-total</a> <a id="11798" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="11806" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11806" class="Bound">n</a>                         <a id="11832" class="Symbol">=</a>  <a id="11835" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10618" class="InductiveConstructor">forward</a> <a id="11843" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a>
<a id="11847" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11756" class="Function">≤-total</a> <a id="11855" class="Symbol">(</a><a id="11856" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="11860" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11860" class="Bound">m</a><a id="11861" class="Symbol">)</a> <a id="11863" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>                      <a id="11889" class="Symbol">=</a>  <a id="11892" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10675" class="InductiveConstructor">flipped</a> <a id="11900" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a>
<a id="11904" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11756" class="Function">≤-total</a> <a id="11912" class="Symbol">(</a><a id="11913" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="11917" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11917" class="Bound">m</a><a id="11918" class="Symbol">)</a> <a id="11920" class="Symbol">(</a><a id="11921" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="11925" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11925" class="Bound">n</a><a id="11926" class="Symbol">)</a> <a id="11928" class="Keyword">with</a> <a id="11933" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11756" class="Function">≤-total</a> <a id="11941" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11917" class="Bound">m</a> <a id="11943" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11925" class="Bound">n</a>
<a id="11945" class="Symbol">...</a>                        <a id="11972" class="Symbol">|</a> <a id="11974" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10618" class="InductiveConstructor">forward</a> <a id="11982" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11982" class="Bound">m≤n</a>  <a id="11987" class="Symbol">=</a>  <a id="11990" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10618" class="InductiveConstructor">forward</a> <a id="11998" class="Symbol">(</a><a id="11999" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="12003" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11982" class="Bound">m≤n</a><a id="12006" class="Symbol">)</a>
<a id="12008" class="Symbol">...</a>                        <a id="12035" class="Symbol">|</a> <a id="12037" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10675" class="InductiveConstructor">flipped</a> <a id="12045" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12045" class="Bound">n≤m</a>  <a id="12050" class="Symbol">=</a>  <a id="12053" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10675" class="InductiveConstructor">flipped</a> <a id="12061" class="Symbol">(</a><a id="12062" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="12066" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12045" class="Bound">n≤m</a><a id="12069" class="Symbol">)</a>{% endraw %}</pre>
In this case the proof is by induction over both the first
and second arguments.  We perform a case analysis:

* _First base case_: If the first argument is `zero` and the
  second argument is `n` then the forward case holds,
  with `z≤n` as evidence that `zero ≤ n`.

* _Second base case_: If the first argument is `suc m` and the
  second argument is `zero` then the flipped case holds, with
  `z≤n` as evidence that `zero ≤ suc m`.

* _Inductive case_: If the first argument is `suc m` and the
  second argument is `suc n`, then the inductive hypothesis
  `≤-total m n` establishes one of the following:

  + The forward case of the inductive hypothesis holds with `m≤n` as
    evidence that `m ≤ n`, from which it follows that the forward case of the
    proposition holds with `s≤s m≤n` as evidence that `suc m ≤ suc n`.

  + The flipped case of the inductive hypothesis holds with `n≤m` as
    evidence that `n ≤ m`, from which it follows that the flipped case of the
    proposition holds with `s≤s n≤m` as evidence that `suc n ≤ suc m`.

This is our first use of the `with` clause in Agda.  The keyword
`with` is followed by an expression and one or more subsequent lines.
Each line begins with an ellipsis (`...`) and a vertical bar (`|`),
followed by a pattern to be matched against the expression
and the right-hand side of the equation.

Every use of `with` is equivalent to defining a helper function.  For
example, the definition above is equivalent to the following.
<pre class="Agda">{% raw %}<a id="≤-total′"></a><a id="13577" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13577" class="Function">≤-total′</a> <a id="13586" class="Symbol">:</a> <a id="13588" class="Symbol">∀</a> <a id="13590" class="Symbol">(</a><a id="13591" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13591" class="Bound">m</a> <a id="13593" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13593" class="Bound">n</a> <a id="13595" class="Symbol">:</a> <a id="13597" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="13598" class="Symbol">)</a> <a id="13600" class="Symbol">→</a> <a id="13602" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10587" class="Datatype">Total</a> <a id="13608" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13591" class="Bound">m</a> <a id="13610" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13593" class="Bound">n</a>
<a id="13612" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13577" class="Function">≤-total′</a> <a id="13621" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="13629" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13629" class="Bound">n</a>        <a id="13638" class="Symbol">=</a>  <a id="13641" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10618" class="InductiveConstructor">forward</a> <a id="13649" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a>
<a id="13653" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13577" class="Function">≤-total′</a> <a id="13662" class="Symbol">(</a><a id="13663" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13667" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13667" class="Bound">m</a><a id="13668" class="Symbol">)</a> <a id="13670" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>     <a id="13679" class="Symbol">=</a>  <a id="13682" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10675" class="InductiveConstructor">flipped</a> <a id="13690" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a>
<a id="13694" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13577" class="Function">≤-total′</a> <a id="13703" class="Symbol">(</a><a id="13704" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13708" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13708" class="Bound">m</a><a id="13709" class="Symbol">)</a> <a id="13711" class="Symbol">(</a><a id="13712" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13716" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13716" class="Bound">n</a><a id="13717" class="Symbol">)</a>  <a id="13720" class="Symbol">=</a>  <a id="13723" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13755" class="Function">helper</a> <a id="13730" class="Symbol">(</a><a id="13731" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13577" class="Function">≤-total′</a> <a id="13740" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13708" class="Bound">m</a> <a id="13742" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13716" class="Bound">n</a><a id="13743" class="Symbol">)</a>
  <a id="13747" class="Keyword">where</a>
  <a id="13755" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13755" class="Function">helper</a> <a id="13762" class="Symbol">:</a> <a id="13764" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10587" class="Datatype">Total</a> <a id="13770" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13708" class="Bound">m</a> <a id="13772" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13716" class="Bound">n</a> <a id="13774" class="Symbol">→</a> <a id="13776" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10587" class="Datatype">Total</a> <a id="13782" class="Symbol">(</a><a id="13783" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13787" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13708" class="Bound">m</a><a id="13788" class="Symbol">)</a> <a id="13790" class="Symbol">(</a><a id="13791" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="13795" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13716" class="Bound">n</a><a id="13796" class="Symbol">)</a>
  <a id="13800" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13755" class="Function">helper</a> <a id="13807" class="Symbol">(</a><a id="13808" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10618" class="InductiveConstructor">forward</a> <a id="13816" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13816" class="Bound">m≤n</a><a id="13819" class="Symbol">)</a>  <a id="13822" class="Symbol">=</a>  <a id="13825" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10618" class="InductiveConstructor">forward</a> <a id="13833" class="Symbol">(</a><a id="13834" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="13838" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13816" class="Bound">m≤n</a><a id="13841" class="Symbol">)</a>
  <a id="13845" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13755" class="Function">helper</a> <a id="13852" class="Symbol">(</a><a id="13853" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10675" class="InductiveConstructor">flipped</a> <a id="13861" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13861" class="Bound">n≤m</a><a id="13864" class="Symbol">)</a>  <a id="13867" class="Symbol">=</a>  <a id="13870" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10675" class="InductiveConstructor">flipped</a> <a id="13878" class="Symbol">(</a><a id="13879" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="13883" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13861" class="Bound">n≤m</a><a id="13886" class="Symbol">)</a>{% endraw %}</pre>
This is also our first use of a `where` clause in Agda.  The keyword `where` is
followed by one or more definitions, which must be indented.  Any variables
bound of the left-hand side of the preceding equation (in this case, `m` and
`n`) are in scope within the nested definition, and any identifiers bound in the
nested definition (in this case, `helper`) are in scope in the right-hand side
of the preceding equation.

If both arguments are equal, then both cases hold and we could return evidence
of either.  In the code above we return the forward case, but there is a
variant that returns the flipped case.
<pre class="Agda">{% raw %}<a id="≤-total″"></a><a id="14524" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14524" class="Function">≤-total″</a> <a id="14533" class="Symbol">:</a> <a id="14535" class="Symbol">∀</a> <a id="14537" class="Symbol">(</a><a id="14538" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14538" class="Bound">m</a> <a id="14540" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14540" class="Bound">n</a> <a id="14542" class="Symbol">:</a> <a id="14544" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="14545" class="Symbol">)</a> <a id="14547" class="Symbol">→</a> <a id="14549" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10587" class="Datatype">Total</a> <a id="14555" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14538" class="Bound">m</a> <a id="14557" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14540" class="Bound">n</a>
<a id="14559" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14524" class="Function">≤-total″</a> <a id="14568" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14568" class="Bound">m</a>       <a id="14576" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>                      <a id="14602" class="Symbol">=</a>  <a id="14605" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10675" class="InductiveConstructor">flipped</a> <a id="14613" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a>
<a id="14617" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14524" class="Function">≤-total″</a> <a id="14626" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="14634" class="Symbol">(</a><a id="14635" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="14639" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14639" class="Bound">n</a><a id="14640" class="Symbol">)</a>                   <a id="14660" class="Symbol">=</a>  <a id="14663" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10618" class="InductiveConstructor">forward</a> <a id="14671" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1232" class="InductiveConstructor">z≤n</a>
<a id="14675" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14524" class="Function">≤-total″</a> <a id="14684" class="Symbol">(</a><a id="14685" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="14689" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14689" class="Bound">m</a><a id="14690" class="Symbol">)</a> <a id="14692" class="Symbol">(</a><a id="14693" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="14697" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14697" class="Bound">n</a><a id="14698" class="Symbol">)</a> <a id="14700" class="Keyword">with</a> <a id="14705" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14524" class="Function">≤-total″</a> <a id="14714" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14689" class="Bound">m</a> <a id="14716" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14697" class="Bound">n</a>
<a id="14718" class="Symbol">...</a>                        <a id="14745" class="Symbol">|</a> <a id="14747" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10618" class="InductiveConstructor">forward</a> <a id="14755" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14755" class="Bound">m≤n</a>   <a id="14761" class="Symbol">=</a>  <a id="14764" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10618" class="InductiveConstructor">forward</a> <a id="14772" class="Symbol">(</a><a id="14773" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="14777" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14755" class="Bound">m≤n</a><a id="14780" class="Symbol">)</a>
<a id="14782" class="Symbol">...</a>                        <a id="14809" class="Symbol">|</a> <a id="14811" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10675" class="InductiveConstructor">flipped</a> <a id="14819" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14819" class="Bound">n≤m</a>   <a id="14825" class="Symbol">=</a>  <a id="14828" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10675" class="InductiveConstructor">flipped</a> <a id="14836" class="Symbol">(</a><a id="14837" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="14841" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14819" class="Bound">n≤m</a><a id="14844" class="Symbol">)</a>{% endraw %}</pre>
It differs from the original code in that it pattern
matches on the second argument before the first argument.


## Monotonicity

If one bumps into both an operator and an ordering at a party, one may ask if
the operator is _monotonic_ with regard to the ordering.  For example, addition
is monotonic with regard to inequality, meaning

    ∀ {m n p q : ℕ} → m ≤ n → p ≤ q → m + p ≤ n + q

The proof is straightforward using the techniques we have learned, and is best
broken into three parts. First, we deal with the special case of showing
addition is monotonic on the right.
<pre class="Agda">{% raw %}<a id="+-monoʳ-≤"></a><a id="15448" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15448" class="Function">+-monoʳ-≤</a> <a id="15458" class="Symbol">:</a> <a id="15460" class="Symbol">∀</a> <a id="15462" class="Symbol">(</a><a id="15463" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15463" class="Bound">m</a> <a id="15465" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15465" class="Bound">p</a> <a id="15467" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15467" class="Bound">q</a> <a id="15469" class="Symbol">:</a> <a id="15471" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="15472" class="Symbol">)</a>
  <a id="15476" class="Symbol">→</a> <a id="15478" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15465" class="Bound">p</a> <a id="15480" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="15482" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15467" class="Bound">q</a>
    <a id="15488" class="Comment">-------------</a>
  <a id="15504" class="Symbol">→</a> <a id="15506" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15463" class="Bound">m</a> <a id="15508" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="15510" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15465" class="Bound">p</a> <a id="15512" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="15514" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15463" class="Bound">m</a> <a id="15516" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="15518" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15467" class="Bound">q</a>
<a id="15520" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15448" class="Function">+-monoʳ-≤</a> <a id="15530" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="15538" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15538" class="Bound">p</a> <a id="15540" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15540" class="Bound">q</a> <a id="15542" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15542" class="Bound">p≤q</a>  <a id="15547" class="Symbol">=</a>  <a id="15550" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15542" class="Bound">p≤q</a>
<a id="15554" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15448" class="Function">+-monoʳ-≤</a> <a id="15564" class="Symbol">(</a><a id="15565" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="15569" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15569" class="Bound">m</a><a id="15570" class="Symbol">)</a> <a id="15572" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15572" class="Bound">p</a> <a id="15574" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15574" class="Bound">q</a> <a id="15576" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15576" class="Bound">p≤q</a>  <a id="15581" class="Symbol">=</a>  <a id="15584" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1281" class="InductiveConstructor">s≤s</a> <a id="15588" class="Symbol">(</a><a id="15589" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15448" class="Function">+-monoʳ-≤</a> <a id="15599" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15569" class="Bound">m</a> <a id="15601" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15572" class="Bound">p</a> <a id="15603" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15574" class="Bound">q</a> <a id="15605" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15576" class="Bound">p≤q</a><a id="15608" class="Symbol">)</a>{% endraw %}</pre>
The proof is by induction on the first argument.

* _Base case_: The first argument is `zero` in which case
  `zero + p ≤ zero + q` simplifies to `p ≤ q`, the evidence
  for which is given by the argument `p≤q`.

* _Inductive case_: The first argument is `suc m`, in which case
  `suc m + p ≤ suc m + q` simplifies to `suc (m + p) ≤ suc (m + q)`.
  The inductive hypothesis `+-monoʳ-≤ m p q p≤q` establishes that
  `m + p ≤ m + q`, and our goal follows by applying `s≤s`.

Second, we deal with the special case of showing addition is
monotonic on the left. This follows from the previous
result and the commutativity of addition.
<pre class="Agda">{% raw %}<a id="+-monoˡ-≤"></a><a id="16264" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16264" class="Function">+-monoˡ-≤</a> <a id="16274" class="Symbol">:</a> <a id="16276" class="Symbol">∀</a> <a id="16278" class="Symbol">(</a><a id="16279" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16279" class="Bound">m</a> <a id="16281" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16281" class="Bound">n</a> <a id="16283" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16283" class="Bound">p</a> <a id="16285" class="Symbol">:</a> <a id="16287" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="16288" class="Symbol">)</a>
  <a id="16292" class="Symbol">→</a> <a id="16294" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16279" class="Bound">m</a> <a id="16296" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="16298" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16281" class="Bound">n</a>
    <a id="16304" class="Comment">-------------</a>
  <a id="16320" class="Symbol">→</a> <a id="16322" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16279" class="Bound">m</a> <a id="16324" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="16326" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16283" class="Bound">p</a> <a id="16328" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="16330" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16281" class="Bound">n</a> <a id="16332" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="16334" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16283" class="Bound">p</a>
<a id="16336" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16264" class="Function">+-monoˡ-≤</a> <a id="16346" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16346" class="Bound">m</a> <a id="16348" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16348" class="Bound">n</a> <a id="16350" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16350" class="Bound">p</a> <a id="16352" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16352" class="Bound">m≤n</a>  <a id="16357" class="Keyword">rewrite</a> <a id="16365" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#8115" class="Function">+-comm</a> <a id="16372" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16346" class="Bound">m</a> <a id="16374" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16350" class="Bound">p</a> <a id="16376" class="Symbol">|</a> <a id="16378" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#8115" class="Function">+-comm</a> <a id="16385" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16348" class="Bound">n</a> <a id="16387" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16350" class="Bound">p</a>  <a id="16390" class="Symbol">=</a> <a id="16392" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15448" class="Function">+-monoʳ-≤</a> <a id="16402" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16350" class="Bound">p</a> <a id="16404" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16346" class="Bound">m</a> <a id="16406" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16348" class="Bound">n</a> <a id="16408" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16352" class="Bound">m≤n</a>{% endraw %}</pre>
Rewriting by `+-comm m p` and `+-comm n p` converts `m + p ≤ n + p` into
`p + m ≤ p + n`, which is proved by invoking `+-monoʳ-≤ p m n m≤n`.

Third, we combine the two previous results.
<pre class="Agda">{% raw %}<a id="+-mono-≤"></a><a id="16622" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16622" class="Function">+-mono-≤</a> <a id="16631" class="Symbol">:</a> <a id="16633" class="Symbol">∀</a> <a id="16635" class="Symbol">(</a><a id="16636" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16636" class="Bound">m</a> <a id="16638" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16638" class="Bound">n</a> <a id="16640" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16640" class="Bound">p</a> <a id="16642" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16642" class="Bound">q</a> <a id="16644" class="Symbol">:</a> <a id="16646" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="16647" class="Symbol">)</a>
  <a id="16651" class="Symbol">→</a> <a id="16653" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16636" class="Bound">m</a> <a id="16655" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="16657" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16638" class="Bound">n</a>
  <a id="16661" class="Symbol">→</a> <a id="16663" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16640" class="Bound">p</a> <a id="16665" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="16667" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16642" class="Bound">q</a>
    <a id="16673" class="Comment">-------------</a>
  <a id="16689" class="Symbol">→</a> <a id="16691" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16636" class="Bound">m</a> <a id="16693" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="16695" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16640" class="Bound">p</a> <a id="16697" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">≤</a> <a id="16699" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16638" class="Bound">n</a> <a id="16701" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="16703" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16642" class="Bound">q</a>
<a id="16705" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16622" class="Function">+-mono-≤</a> <a id="16714" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16714" class="Bound">m</a> <a id="16716" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16716" class="Bound">n</a> <a id="16718" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16718" class="Bound">p</a> <a id="16720" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16720" class="Bound">q</a> <a id="16722" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16722" class="Bound">m≤n</a> <a id="16726" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16726" class="Bound">p≤q</a>  <a id="16731" class="Symbol">=</a>  <a id="16734" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7303" class="Function">≤-trans</a> <a id="16742" class="Symbol">(</a><a id="16743" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16264" class="Function">+-monoˡ-≤</a> <a id="16753" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16714" class="Bound">m</a> <a id="16755" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16716" class="Bound">n</a> <a id="16757" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16718" class="Bound">p</a> <a id="16759" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16722" class="Bound">m≤n</a><a id="16762" class="Symbol">)</a> <a id="16764" class="Symbol">(</a><a id="16765" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15448" class="Function">+-monoʳ-≤</a> <a id="16775" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16716" class="Bound">n</a> <a id="16777" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16718" class="Bound">p</a> <a id="16779" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16720" class="Bound">q</a> <a id="16781" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16726" class="Bound">p≤q</a><a id="16784" class="Symbol">)</a>{% endraw %}</pre>
Invoking `+-monoˡ-≤ m n p m≤n` proves `m + p ≤ n + p` and invoking
`+-monoʳ-≤ n p q p≤q` proves `n + p ≤ n + q`, and combining these with
transitivity proves `m + p ≤ n + q`, as was to be shown.

#### Exercise (stretch, `≤-reasoning`)

The proof of monotonicity (and the associated lemmas) can be written
in a more readable form by using an anologue of our notation for
`≡-reasoning`.  Read ahead to
Chapter [Equality]({{ site.baseurl }}{% link out/plfa/Equality.md %})
to see how `≡-reasoning` is defined, define `≤-reasoning` analogously,
and use it to write out an alternative proof that addition is
monotonic with regard to inequality.

#### Exercise (stretch, `*-mono-≤`)

Show that multiplication is monotonic with regard to inequality.


## Strict inequality {#strict-inequality}

We can define strict inequality similarly to inequality.
<pre class="Agda">{% raw %}<a id="17655" class="Keyword">infix</a> <a id="17661" class="Number">4</a> <a id="17663" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17673" class="Datatype Operator">_&lt;_</a>

<a id="17668" class="Keyword">data</a> <a id="_&lt;_"></a><a id="17673" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17673" class="Datatype Operator">_&lt;_</a> <a id="17677" class="Symbol">:</a> <a id="17679" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="17681" class="Symbol">→</a> <a id="17683" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="17685" class="Symbol">→</a> <a id="17687" class="PrimitiveType">Set</a> <a id="17691" class="Keyword">where</a>

  <a id="_&lt;_.z&lt;s"></a><a id="17700" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17700" class="InductiveConstructor">z&lt;s</a> <a id="17704" class="Symbol">:</a> <a id="17706" class="Symbol">∀</a> <a id="17708" class="Symbol">{</a><a id="17709" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17709" class="Bound">n</a> <a id="17711" class="Symbol">:</a> <a id="17713" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="17714" class="Symbol">}</a>
      <a id="17722" class="Comment">------------</a>
    <a id="17739" class="Symbol">→</a> <a id="17741" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="17746" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17673" class="Datatype Operator">&lt;</a> <a id="17748" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="17752" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17709" class="Bound">n</a>

  <a id="_&lt;_.s&lt;s"></a><a id="17757" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17757" class="InductiveConstructor">s&lt;s</a> <a id="17761" class="Symbol">:</a> <a id="17763" class="Symbol">∀</a> <a id="17765" class="Symbol">{</a><a id="17766" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17766" class="Bound">m</a> <a id="17768" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17768" class="Bound">n</a> <a id="17770" class="Symbol">:</a> <a id="17772" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="17773" class="Symbol">}</a>
    <a id="17779" class="Symbol">→</a> <a id="17781" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17766" class="Bound">m</a> <a id="17783" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17673" class="Datatype Operator">&lt;</a> <a id="17785" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17768" class="Bound">n</a>
      <a id="17793" class="Comment">-------------</a>
    <a id="17811" class="Symbol">→</a> <a id="17813" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="17817" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17766" class="Bound">m</a> <a id="17819" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17673" class="Datatype Operator">&lt;</a> <a id="17821" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="17825" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17768" class="Bound">n</a>{% endraw %}</pre>
The key difference is that zero is less than the successor of an
arbitrary number, but is not less than zero.

Clearly, strict inequality is not reflexive. However it is
_irreflexive_ in that `n < n` never holds for any value of `n`.
Like inequality, strict inequality is transitive.
Strict inequality is not total, but satisfies the closely related property of
_trichotomy_: for any `m` and `n`, exactly one of `m < n`, `m ≡ n`, or `m > n`
holds (where we define `m > n` to hold exactly when `n < m`).
It is also monotonic with regards to addition and multiplication.

Most of the above are considered in exercises below.  Irreflexivity
requires negation, as does the fact that the three cases in
trichotomy are mutually exclusive, so those points are deferred to
Chapter [Negation]({{ site.baseurl }}{% link out/plfa/Negation.md %}).

It is straightforward to show that `suc m ≤ n` implies `m < n`,
and conversely.  One can then give an alternative derivation of the
properties of strict inequality, such as transitivity, by directly
exploiting the corresponding properties of inequality.

#### Exercise (`<-trans`)

Show that strict inequality is transitive.

#### Exercise (`trichotomy`) {#trichotomy}

Show that strict inequality satisfies a weak version of trichotomy, in
the sense that for any `m` and `n` that one of the following holds:
* `m < n`,
* `m ≡ n`, or
* `m > n`
This only involves two relations, as we define `m > n` to
be the same as `n < m`. You will need a suitable data declaration,
similar to that used for totality.  (We will show that the three cases
are exclusive after [negation]({{ site.baseurl }}{% link out/plfa/Negation.md %}) is introduced.)

#### Exercise (`+-mono-<`)

Show that addition is monotonic with respect to strict inequality.
As with inequality, some additional definitions may be required.

#### Exercise (`≤-implies-<`, `<-implies-≤`)

Show that `suc m ≤ n` implies `m < n`, and conversely.

#### Exercise (`<-trans′`)

Give an alternative proof that strict inequality is transitive,
using the relating between strict inequality and inequality and
the fact that inequality is transitive.


## Even and odd

As a further example, let's specify even and odd numbers.  Inequality
and strict inequality are _binary relations_, while even and odd are
_unary relations_, sometimes called _predicates_.
<pre class="Agda">{% raw %}<a id="20194" class="Keyword">data</a> <a id="even"></a><a id="20199" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20199" class="Datatype">even</a> <a id="20204" class="Symbol">:</a> <a id="20206" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="20208" class="Symbol">→</a> <a id="20210" class="PrimitiveType">Set</a>
<a id="20214" class="Keyword">data</a> <a id="odd"></a><a id="20219" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20219" class="Datatype">odd</a>  <a id="20224" class="Symbol">:</a> <a id="20226" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="20228" class="Symbol">→</a> <a id="20230" class="PrimitiveType">Set</a>

<a id="20235" class="Keyword">data</a> <a id="20240" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20199" class="Datatype">even</a> <a id="20245" class="Keyword">where</a>

  <a id="even.zero"></a><a id="20254" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20254" class="InductiveConstructor">zero</a> <a id="20259" class="Symbol">:</a>
      <a id="20267" class="Comment">---------</a>
      <a id="20283" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20199" class="Datatype">even</a> <a id="20288" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>

  <a id="even.suc"></a><a id="20296" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20296" class="InductiveConstructor">suc</a>  <a id="20301" class="Symbol">:</a> <a id="20303" class="Symbol">∀</a> <a id="20305" class="Symbol">{</a><a id="20306" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20306" class="Bound">n</a> <a id="20308" class="Symbol">:</a> <a id="20310" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="20311" class="Symbol">}</a>
    <a id="20317" class="Symbol">→</a> <a id="20319" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20219" class="Datatype">odd</a> <a id="20323" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20306" class="Bound">n</a>
      <a id="20331" class="Comment">------------</a>
    <a id="20348" class="Symbol">→</a> <a id="20350" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20199" class="Datatype">even</a> <a id="20355" class="Symbol">(</a><a id="20356" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="20360" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20306" class="Bound">n</a><a id="20361" class="Symbol">)</a>

<a id="20364" class="Keyword">data</a> <a id="20369" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20219" class="Datatype">odd</a> <a id="20373" class="Keyword">where</a>

  <a id="odd.suc"></a><a id="20382" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20382" class="InductiveConstructor">suc</a>   <a id="20388" class="Symbol">:</a> <a id="20390" class="Symbol">∀</a> <a id="20392" class="Symbol">{</a><a id="20393" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20393" class="Bound">n</a> <a id="20395" class="Symbol">:</a> <a id="20397" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="20398" class="Symbol">}</a>
    <a id="20404" class="Symbol">→</a> <a id="20406" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20199" class="Datatype">even</a> <a id="20411" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20393" class="Bound">n</a>
      <a id="20419" class="Comment">-----------</a>
    <a id="20435" class="Symbol">→</a> <a id="20437" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20219" class="Datatype">odd</a> <a id="20441" class="Symbol">(</a><a id="20442" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="20446" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20393" class="Bound">n</a><a id="20447" class="Symbol">)</a>{% endraw %}</pre>
A number is even if it is zero or the successor of an odd number,
and odd if it is the successor of an even number.

This is our first use of a mutually recursive datatype declaration.
Since each identifier must be defined before it is used, we first
declare the indexed types `even` and `odd` (omitting the `where`
keyword and the declarations of the constructors) and then
declare the constructors (omitting the signatures `ℕ → Set`
which were given earlier).

This is also our first use of _overloaded_ constructors,
that is, using the same name for different constructors depending on
the context.  Here `suc` means one of three constructors:

    suc : `ℕ → `ℕ

    suc : ∀ {n : ℕ}
      → odd n
        ------------
      → even (suc n)

    suc : ∀ {n : ℕ}
      → even n
        -----------
      → odd (suc n)

Similarly, `zero` refers to one of two constructors. Due to how it
does type inference, Agda does not allow overloading of defined names,
but does allow overloading of constructors.  It is recommended that
one restrict overloading to related meanings, as we have done here,
but it is not required.

We show that the sum of two even numbers is even.
<pre class="Agda">{% raw %}<a id="e+e≡e"></a><a id="21642" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21642" class="Function">e+e≡e</a> <a id="21648" class="Symbol">:</a> <a id="21650" class="Symbol">∀</a> <a id="21652" class="Symbol">{</a><a id="21653" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21653" class="Bound">m</a> <a id="21655" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21655" class="Bound">n</a> <a id="21657" class="Symbol">:</a> <a id="21659" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="21660" class="Symbol">}</a>
  <a id="21664" class="Symbol">→</a> <a id="21666" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20199" class="Datatype">even</a> <a id="21671" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21653" class="Bound">m</a>
  <a id="21675" class="Symbol">→</a> <a id="21677" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20199" class="Datatype">even</a> <a id="21682" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21655" class="Bound">n</a>
    <a id="21688" class="Comment">------------</a>
  <a id="21703" class="Symbol">→</a> <a id="21705" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20199" class="Datatype">even</a> <a id="21710" class="Symbol">(</a><a id="21711" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21653" class="Bound">m</a> <a id="21713" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="21715" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21655" class="Bound">n</a><a id="21716" class="Symbol">)</a>

<a id="o+e≡o"></a><a id="21719" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21719" class="Function">o+e≡o</a> <a id="21725" class="Symbol">:</a> <a id="21727" class="Symbol">∀</a> <a id="21729" class="Symbol">{</a><a id="21730" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21730" class="Bound">m</a> <a id="21732" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21732" class="Bound">n</a> <a id="21734" class="Symbol">:</a> <a id="21736" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="21737" class="Symbol">}</a>
  <a id="21741" class="Symbol">→</a> <a id="21743" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20219" class="Datatype">odd</a> <a id="21747" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21730" class="Bound">m</a>
  <a id="21751" class="Symbol">→</a> <a id="21753" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20199" class="Datatype">even</a> <a id="21758" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21732" class="Bound">n</a>
    <a id="21764" class="Comment">-----------</a>
  <a id="21778" class="Symbol">→</a> <a id="21780" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20219" class="Datatype">odd</a> <a id="21784" class="Symbol">(</a><a id="21785" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21730" class="Bound">m</a> <a id="21787" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="21789" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21732" class="Bound">n</a><a id="21790" class="Symbol">)</a>

<a id="21793" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21642" class="Function">e+e≡e</a> <a id="21799" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20254" class="InductiveConstructor">zero</a>     <a id="21808" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21808" class="Bound">en</a>  <a id="21812" class="Symbol">=</a>  <a id="21815" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21808" class="Bound">en</a>
<a id="21818" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21642" class="Function">e+e≡e</a> <a id="21824" class="Symbol">(</a><a id="21825" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20296" class="InductiveConstructor">suc</a> <a id="21829" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21829" class="Bound">om</a><a id="21831" class="Symbol">)</a> <a id="21833" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21833" class="Bound">en</a>  <a id="21837" class="Symbol">=</a>  <a id="21840" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20296" class="InductiveConstructor">suc</a> <a id="21844" class="Symbol">(</a><a id="21845" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21719" class="Function">o+e≡o</a> <a id="21851" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21829" class="Bound">om</a> <a id="21854" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21833" class="Bound">en</a><a id="21856" class="Symbol">)</a>

<a id="21859" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21719" class="Function">o+e≡o</a> <a id="21865" class="Symbol">(</a><a id="21866" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20382" class="InductiveConstructor">suc</a> <a id="21870" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21870" class="Bound">em</a><a id="21872" class="Symbol">)</a> <a id="21874" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21874" class="Bound">en</a>  <a id="21878" class="Symbol">=</a>  <a id="21881" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20382" class="InductiveConstructor">suc</a> <a id="21885" class="Symbol">(</a><a id="21886" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21642" class="Function">e+e≡e</a> <a id="21892" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21870" class="Bound">em</a> <a id="21895" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21874" class="Bound">en</a><a id="21897" class="Symbol">)</a>{% endraw %}</pre>
Corresponding to the mutually recursive types, we use two mutually recursive
functions, one to show that the sum of two even numbers is even, and the other
to show that the sum of an odd and an even number is odd.

This is our first use of mutually recursive functions.  Since each identifier
must be defined before it is used, we first give the signatures for both
functions and then the equations that define them.

To show that the sum of two even numbers is even, consider the
evidence that the first number is even. If it is because it is zero,
then the sum is even because the second number is even.  If it is
because it is the successor of an odd number, then the result is even
because it is the successor of the sum of an odd and an even number,
which is odd.

To show that the sum of an odd and even number is odd, consider the
evidence that the first number is odd. If it is because it is the
successor of an even number, then the result is odd because it is the
successor of the sum of two even numbers, which is even.

#### Exercise (`o+o≡e`)

Show that the sum of two odd numbers is even.


<!--

## Formalising preorder

<pre class="Agda">{% raw %}<a id="23059" class="Keyword">record</a> <a id="IsPreorder"></a><a id="23066" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23066" class="Record">IsPreorder</a> <a id="23077" class="Symbol">{</a><a id="23078" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23078" class="Bound">A</a> <a id="23080" class="Symbol">:</a> <a id="23082" class="PrimitiveType">Set</a><a id="23085" class="Symbol">}</a> <a id="23087" class="Symbol">(</a><a id="23088" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23088" class="Bound Operator">_≤_</a> <a id="23092" class="Symbol">:</a> <a id="23094" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23078" class="Bound">A</a> <a id="23096" class="Symbol">→</a> <a id="23098" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23078" class="Bound">A</a> <a id="23100" class="Symbol">→</a> <a id="23102" class="PrimitiveType">Set</a><a id="23105" class="Symbol">)</a> <a id="23107" class="Symbol">:</a> <a id="23109" class="PrimitiveType">Set</a> <a id="23113" class="Keyword">where</a>
  <a id="23121" class="Keyword">field</a>
    <a id="IsPreorder.reflexive"></a><a id="23131" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23131" class="Field">reflexive</a> <a id="23141" class="Symbol">:</a> <a id="23143" class="Symbol">∀</a> <a id="23145" class="Symbol">{</a><a id="23146" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23146" class="Bound">x</a> <a id="23148" class="Symbol">:</a> <a id="23150" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23078" class="Bound">A</a><a id="23151" class="Symbol">}</a> <a id="23153" class="Symbol">→</a> <a id="23155" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23146" class="Bound">x</a> <a id="23157" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23088" class="Bound Operator">≤</a> <a id="23159" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23146" class="Bound">x</a>
    <a id="IsPreorder.trans"></a><a id="23165" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23165" class="Field">trans</a> <a id="23171" class="Symbol">:</a> <a id="23173" class="Symbol">∀</a> <a id="23175" class="Symbol">{</a><a id="23176" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23176" class="Bound">x</a> <a id="23178" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23178" class="Bound">y</a> <a id="23180" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23180" class="Bound">z</a> <a id="23182" class="Symbol">:</a> <a id="23184" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23078" class="Bound">A</a><a id="23185" class="Symbol">}</a> <a id="23187" class="Symbol">→</a> <a id="23189" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23176" class="Bound">x</a> <a id="23191" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23088" class="Bound Operator">≤</a> <a id="23193" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23178" class="Bound">y</a> <a id="23195" class="Symbol">→</a> <a id="23197" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23178" class="Bound">y</a> <a id="23199" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23088" class="Bound Operator">≤</a> <a id="23201" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23180" class="Bound">z</a> <a id="23203" class="Symbol">→</a> <a id="23205" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23176" class="Bound">x</a> <a id="23207" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23088" class="Bound Operator">≤</a> <a id="23209" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23180" class="Bound">z</a>

<a id="IsPreorder-≤"></a><a id="23212" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23212" class="Function">IsPreorder-≤</a> <a id="23225" class="Symbol">:</a> <a id="23227" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23066" class="Record">IsPreorder</a> <a id="23238" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1205" class="Datatype Operator">_≤_</a>
<a id="23242" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#23212" class="Function">IsPreorder-≤</a> <a id="23255" class="Symbol">=</a>
  <a id="23259" class="Keyword">record</a>
    <a id="23270" class="Symbol">{</a> <a id="23272" class="Field">reflexive</a> <a id="23282" class="Symbol">=</a> <a id="23284" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6559" class="Function">≤-refl</a>
    <a id="23295" class="Symbol">;</a> <a id="23297" class="Field">trans</a> <a id="23303" class="Symbol">=</a> <a id="23305" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7303" class="Function">≤-trans</a>
    <a id="23317" class="Symbol">}</a>{% endraw %}</pre>

-->


## Standard prelude

Definitions similar to those in this chapter can be found in the standard library.
<pre class="Agda">{% raw %}<a id="23454" class="Keyword">import</a> <a id="23461" href="https://agda.github.io/agda-stdlib/Data.Nat.html" class="Module">Data.Nat</a> <a id="23470" class="Keyword">using</a> <a id="23476" class="Symbol">(</a><a id="23477" href="https://agda.github.io/agda-stdlib/Data.Nat.Base.html#802" class="Datatype Operator">_≤_</a><a id="23480" class="Symbol">;</a> <a id="23482" href="https://agda.github.io/agda-stdlib/Data.Nat.Base.html#833" class="InductiveConstructor">z≤n</a><a id="23485" class="Symbol">;</a> <a id="23487" href="https://agda.github.io/agda-stdlib/Data.Nat.Base.html#875" class="InductiveConstructor">s≤s</a><a id="23490" class="Symbol">)</a>
<a id="23492" class="Keyword">import</a> <a id="23499" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html" class="Module">Data.Nat.Properties</a> <a id="23519" class="Keyword">using</a> <a id="23525" class="Symbol">(</a><a id="23526" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#1902" class="Function">≤-refl</a><a id="23532" class="Symbol">;</a> <a id="23534" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#2095" class="Function">≤-trans</a><a id="23541" class="Symbol">;</a> <a id="23543" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#1952" class="Function">≤-antisym</a><a id="23552" class="Symbol">;</a> <a id="23554" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#2207" class="Function">≤-total</a><a id="23561" class="Symbol">;</a>
                                  <a id="23597" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#11056" class="Function">+-monoʳ-≤</a><a id="23606" class="Symbol">;</a> <a id="23608" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#10966" class="Function">+-monoˡ-≤</a><a id="23617" class="Symbol">;</a> <a id="23619" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#10810" class="Function">+-mono-≤</a><a id="23627" class="Symbol">)</a>
<a id="23629" class="Keyword">import</a> <a id="23636" href="https://agda.github.io/agda-stdlib/Relation.Binary.html" class="Module">Relation.Binary</a> <a id="23652" class="Keyword">using</a> <a id="23658" class="Symbol">(</a><a id="23659" href="https://agda.github.io/agda-stdlib/Relation.Binary.html#794" class="Record">IsPreorder</a><a id="23669" class="Symbol">)</a>{% endraw %}</pre>
In the standard library, `≤-total` is formalised in terms of
disjunction (which we define in
Chapter [Connectives]({{ site.baseurl }}{% link out/plfa/Connectives.md %})),
and `+-monoʳ-≤`, `+-monoˡ-≤`, `+-mono-≤` are proved differently than here,
and more arguments are implicit.


## Unicode

This chapter uses the following unicode.

    ≤  U+2264  LESS-THAN OR EQUAL TO (\<=, \le)
    ≥  U+2265  GREATER-THAN OR EQUAL TO (\>=, \ge)
    ˡ  U+02E1  MODIFIER LETTER SMALL L (\^l)
    ʳ  U+02B3  MODIFIER LETTER SMALL R (\^r)

The commands `\^l` and `\^r` give access to a variety of superscript
leftward and rightward arrows in addition to superscript letters `l` and `r`.
