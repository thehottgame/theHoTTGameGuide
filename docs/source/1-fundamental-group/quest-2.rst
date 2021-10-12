.. _quest2ZIsASet:

****************************
Quest 2 - ``ℤ`` is a Set
****************************

..
  - show if ``loopSpace X`` is a set,
    then ``loopSpace loopSpace X`` is trivial i.e. ``⊤``.
  - motivates if ``ℤ`` is a set then ``loopSpace loopSpace S¹`` is ``⊤``
    hence higher loop spaces are trivial.
    (follows from exercise : ``⊤`` is a set)
  - show ``ℤ ≡ ℕ ⊔ ℕ`` and then reduce problem to path space of ``⊔``.
  - end up with showing isomorphism which needs ``J``
  - For the iso -- do ``inv : ⊔NoConfusion → path`` first
  - To define ``fun : path → ⊔NoConfusion``
  - get stuck on defining ``fun`` and big admonition saying go back to
    trinitarianism
    - list instances of trinitarianism here
  - finish proof


Part 0 - ``loopSpace loopSpace``
================================

We are interested in knowing what
the higher homotopy groups of ``S¹`` might be.
Whilst the data of the fundamental group ``π₁ S¹`` is captured
in ``loopSpace S¹ base``,
the data of ``π₂ S¹`` would be captured in
``loopSpace (loopSpace S¹ base) refl``;
loops in ``loopSpace S¹ base`` based at ``refl``.
Points in the second loop space are paths ``h : refl ≡ refl``,
i.e. ``h`` would be a homotopy from the constant path to itself.

.. insert picture

The second loop space contains an obvious point ``refl : refl ≡ refl``
(this is of course not the same "refl" as the one before),
and we could define the next loop space to be loops in
``loopSpace (loopSpace S¹ base) refl`` based at (the new) ``refl``.

Using what we will eventually show - ``loopSpace S¹ base ≡ ℤ`` -
we can show that the second loop space of ``S¹`` is trivial,
in the sense that it just consists of a point :

.. code::

   loopSpace (loopSpace S¹ base) refl ≡ loopSpace ℤ 0 ≡ ⊤

This is because *any two paths in* ``ℤ`` *are homotopic*,
hence ``refl : 0 ≡ 0`` is homotopic to any other loop ``0 ≡ 0``,
so ``loopSpace ℤ 0`` is contractible -
it looks just like the point ``⊤``.

``isSet``
---------

We can justify what we have just said by showing that
"if any two paths in a space ``A`` are homotopic then
the loop space of ``A`` at any point in ``A``
looks like ``⊤``".

.. admonition:: ``isSet``

   The statement "any two paths in the space ``A`` are homotopic"
   is captured in the definition of ``isSet`` :

   .. code:: agda

      isSet : Type → Type
      isSet A = (x y : A) → isProp (x ≡ y)

   In the above ``isProp`` captures the statement
   "any two points are (continuously) connected by a path" :

   .. code:: agda

      isProp : Type → Type
      isProp A = (x y : A) → x ≡ y

   If a type satisfies ``isSet`` we say it *is a set*,
   and if it satisfies ``isProp`` we say it *is a proposition*.

   .. raw:: html

      <p>
      <details>
      <summary>Why the words "set" and "proposition"?</summary>

   Intuitively a "set" is meant to be a bunch of disjoint points.
   However in homotopy type theory we consider points up to paths,
   and paths up to homotopy,
   hence a "set" is a bunch of disjoint blobs,
   where each blob is contractible to a point.
   In other words a "set" is a type where any circle
   (that lands in a blob)
   can be filled (hence the blob is contractible).

   We will justify the use of the word "proposition" once
   we have introduced the propositional perspective on types,
   see :ref:`trinitarianism<0-trinitarianism>` and
   :ref:`part5UsingThePropositionalPerspective`.

   .. raw:: html

      </details>
      </p>

   .. raw:: html

      <p>
      <details>
      <summary>All maps are continuous in HoTT</summary>

   There is a subtlety in the definition ``isProp``.
   Having ``isProp A`` is *stronger* than saying that the space ``A`` is path connected.
   Since ``A`` is equipped with a *continuous* map taking pairs ``x y : A``
   to a path between them.

   We will show in a later quest
   that ``isProp S¹`` is *empty* despite ``S¹`` being path connected.

   .. missing link

   .. raw:: html

      </details>
      </p>

So we show that

.. code:: agda

   isSet→LoopSpace≡⊤ : {A : Type} (x : A) → isSet A → (x ≡ x) ≡ ⊤
   isSet→LoopSpace≡⊤ = {!!}

Locate this in ``1FundamentalGroup/Quest2.agda``
and try filling it in.

.. raw:: html

   <p>
   <details>
   <summary>Hint 0</summary>

Imitating what we did with ``flipPath`` and ``flipIso``
reduce this to showing that for each ``x : A`` and ``h : isSet A``
we have

- ``fun : x ≡ x → ⊤``
- ``inv : ⊤ → x ≡ x``
- ``rightInv : section fun inv``
- ``leftInv : retract inv fun``

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Hint 1</summary>

There is only one possible map from ``x ≡ x`` to ``⊤``
since ``⊤`` is terminal (see :ref:`trinitarianism <0-trinitarianism>`).

To map out of ``⊤`` one can do cases and see that
you only need to map ``tt``.

.. raw:: html

   <p>
   <details>
   <summary>Solution</summary>

- ``fun`` can just be ``(λ p → tt)``
- ``inv`` can be

  .. code:: agda

     inv : ⊤ → x ≡ x
     inv tt = refl

.. raw:: html

   </details>
   </p>

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Hint 2</summary>

For ``rightInv`` by casing on the point in ``⊤``
there should be nothing much to show.

For ``leftInv`` we need to use our assumption
that "any two paths are homotopic".

.. raw:: html

   <p>
   <details>
   <summary>Solution</summary>

.. code:: agda

   rightInv : section (λ p → tt) inv
   rightInv tt = refl

   leftInv : retract (λ p → tt) inv
   leftInv p = h x x refl p

.. raw:: html

   </details>
   </p>

.. raw:: html

   </details>
   </p>

.. admonition:: The goal

   We have therefore reduced our goal to
   showing that ``ℤ`` is a set.

Part 1 - ``ℤ`` as a disjoint sum ``ℕ ⊔ ℕ``
==========================================

As a first step, we note that ``ℤ`` actually looks like
two disjoint copies of ``ℕ``, i.e. we have

.. code:: agda

   ℤ≡ℕ⊔ℕ : ℤ ≡ ℕ ⊔ ℕ

where we have the definition of the *disjoint sum of two spaces* as follows

.. code:: agda

   data _⊔_ (A B : Type) : Type where

     inl : A → A ⊔ B
     inr : B → A ⊔ B

It says there are two ways of making points in the space,
taking them from ``A`` and taking them from ``B``.
Try proving ``ℤ ≡ ℕ ⊔ ℕ`` in ``1FundamentalGroup/Quest2.agda``.

.. raw:: html

   <p>
   <details>
   <summary>Hint</summary>

As in defining ``flipPath`` in :ref:`quest 0 <quest0WorkingWithTheCircle>`
we first make an isomorphism and then convert it to a path/proof of equality.
To make the isomorphism note that
the definition of ``ℤ`` is already as "two copies of ``ℕ``",
as described in :ref:`quest 1 <definitionOfZ>`.

If you have made the function and inverse appropriately,
you should only need constant paths in the
proofs that they satisfy ``section`` and ``retract``
respectively.

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Solution</summary>

.. code:: agda

  ℤ≡ℕ⊔ℕ : ℤ ≡ ℕ ⊔ ℕ
  ℤ≡ℕ⊔ℕ = isoToPath (iso fun inv rightInv leftInv) where

  fun : ℤ → ℕ ⊔ ℕ
  fun (pos n) = inl n
  fun (negsuc n) = inr n

  inv : ℕ ⊔ ℕ → ℤ
  inv (inl n) = pos n
  inv (inr n) = negsuc n

  rightInv : section fun inv
  rightInv (inl n) = refl
  rightInv (inr n) = refl

  leftInv : retract fun inv
  leftInv (pos n) = refl
  leftInv (negsuc n) = refl

.. raw:: html

   </details>
   </p>

We want to show that ``ℤ`` is a set,
by using the path ``ℤ≡ℕ⊔ℕ``.
Intuitively if ``ℕ`` is a set then two disjoint
copies of it should also be a set,
(think about filling circles on the disjoint sum).
Thus we can break down our goal into two :

.. admonition:: Goal 1 : ``ℕ`` is a set

   .. code:: agda

      isSetℕ : isSet ℕ
      isSetℕ = {!!}

.. admonition:: Goal 2

   If ``A`` and ``B`` are both sets then ``A ⊔ B``
   is also a set.

Goal 1 will be handled in a :ref:`side quest <isSetNat>`.
We focus on Goal 2 from now on.

Part 2 - Disjoint Sum of Sets is a Set
======================================

Try formulating (but not proving) the result ``isSet⊔``,
which should say "if spaces ``A`` and ``B`` are both sets
then so is their disjoint sum ``A ⊔ B``".
This should be done in ``1FundamentalGroup/Quest2.agda``
where indicated.

.. raw:: html

   <p>
   <details>
   <summary>Solution</summary>

.. code::

   isSet⊔ : {A B : Type} → isSet A → isSet B → isSet (A ⊔ B)
   isSet⊔ = {!!}

.. raw:: html

   </details>
   </p>

Without proving this, we can use this to show ``isSet (ℕ ⊔ ℕ)``
using ``isSetℕ : isSet ℕ``,
which will be shown in a :ref:`side quest <isSetNat>`.
Then using either ``pathToFun`` or ``endPt`` you can show
``isSet ℤ`` from ``isSet (ℕ ⊔ ℕ)``,
using the path from ``ℤ`` to ``ℕ ⊔ ℕ`` we made earlier.
Try to set up everything described in this paragraph where indicated
in ``1FundamentalGroup/Quest2.agda``.

.. raw:: html

   <p>
   <details>
   <summary>Hint : The statement</summary>

.. code:: agda

   isSetℤ : isSet ℤ

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Hint : following along paths </summary>

To use ``pathToFun`` you must figure out what path you are following
and what point you are following the path along.

To use ``endPt`` you must figure out what bundle you are making,
what the path in the base space is,
and what point you are starting at in the first fiber.

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Partial solutions</summary>

The point you need to follow in either case
is the point in the space ``isSet (ℕ ⊔ ℕ)`` :

.. code:: agda

   isSetℤ : isSet ℤ
   isSetℤ = pathToFun {!!} (isSet⊔ isSetℕ isSetℕ)

   isSetℤ' : isSet ℤ
   isSetℤ' = endPt {!!} {!!} (isSet⊔ isSetℕ isSetℕ)

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Solutions</summary>

.. code:: agda

   isSetℤ : isSet ℤ
   isSetℤ = pathToFun (cong isSet (sym ℤ≡ℕ⊔ℕ)) (isSet⊔ isSetℕ isSetℕ)

   isSetℤ' : isSet ℤ
   isSetℤ' = endPt (λ A → isSet A) (sym ℤ≡ℕ⊔ℕ) (isSet⊔ isSetℕ isSetℕ)

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Refining issues</summary>

If you tried refining using ``endPt``
you may have been given 2 holes instead of 3.
This is because ``agda`` had too many
possible options when trying to match up
the output of ``endPt`` and the goal.
To add an extra hole simply add a ``?``
afterwards and reload.

.. raw:: html

   </details>
   </p>

Once this is complete we can go back and work on ``isSet⊔``.

Part 3 - Path Space of Disjoint Sums
====================================

Motivation
----------

- Locate your formulation of ``isSet⊔``.
- We assume ``hA : isSet A``,
  ``hB : isSet B``, and points ``x y : A ⊔ B``.
  Currently our code looks like

  .. code:: agda

     isSet⊔ : {A B : Type} → isSet A → isSet B → isSet (A ⊔ B)
     isSet⊔ hA hB x y = {!!}

- Check the goal.
  It should be asking for a point in the space ``isProp (x ≡ y)``.

  We need to consider how to get information on
  the path space of ``A ⊔ B`` when our hypotheses are
  about the path spaces of ``A`` and ``B`` respectively.
  We could try to case on ``x`` and ``y``.

- If ``x`` and ``y`` are "both from ``A``",
  i.e. of the form ``inl ax`` and ``inl ay`` for ``ax ay : A``,
  then we need to find a point in ``isProp (inl ax ≡ inl ay)``.
  This *should* be due to ``hA``, which gives us
  ``hA ax ay : isProp (ax ≡ ay)``.
  So somehow we need to identify the path spaces
  ``inl ax ≡ inl ay`` and ``ax ≡ ay``.
- If ``x`` and ``y`` are of the forms ``inl ax`` and ``inr by``
  respectively for ``ax : A`` and ``by : B`` then
  intuitively the space ``inl ax ≡ inr bx`` *should* be empty,
  since the sum is disjoint.
- The other two cases are similar.

The conclusion is that we need some kind of
classification of the path space of disjoint sums.

Classifying the path space
--------------------------

.. admonition:: Path space of disjoint sums

   A path in the the disjoint sum
   should just be a path in one of the two parts.

   This says points from ``A``
   cannot be confused with points from ``B``
   or points in ``A`` that they were not already path connected to.

For now we leave ``isSet⊔`` alone and define a function ``⊔NoConfusion``
that takes two points in ``A ⊔ B`` and returns a space,
which is meant to represent the path space in each case,
as described in our motivation above.
Try to formulate (but not fill in) this where indicated in
``Quest2.agda``.
It should look like:

.. raw:: html

   <p>
   <details>
   <summary>Solution</summary>

.. code:: agda

   ⊔NoConfusion : {A B : Type} → A ⊔ B → A ⊔ B → Type
   ⊔NoConfusion = {!!}

.. raw:: html

   </details>
   </p>

Assume points ``x`` and ``y`` in the disjoint sum
and try to case on them.
There should be four cases.

- When both points are from ``A``,
  i.e. they are ``inl ax`` and ``inl ay``,
  then we should give the space ``ax ≡ ay``,
  which we expect to be isomorphic to ``inl ax ≡ inl ay``.
- (Two cases) When each is from a different space we expect the path
  space between them to be empty, so we should give ``⊥``.
- If both are from ``B`` then we should
  imitate what we did in the first case

Using the Classification
------------------------

Now we have two of goals :

- ``Path≡⊔NoConfusion`` :
  We need to show that for each ``x y : A ⊔ B``
  the path space looks like our classification,
  i.e. that ``(x ≡ y) ≡ (⊔NoConfusion x y)``
- ``isSet⊔NoConfusion`` : For ``isSet⊔``, given
  ``hA : isProp A``, ``hB : isProp B`` and ``x y : A ⊔ B``
  we needed to show ``isProp (x ≡ y)``.
  Hence we want to show that under the same assumptions
  ``isProp (⊔NoConfusion x y)``.

Formalise (but don't prove) both of these where indicated in
``1FundamentalGroup/Quest2.agda``.
They should look like

.. raw:: html

   <p>
   <details>
   <summary>Solutions</summary>

.. code:: agda

   Path≡⊔NoConfusion : (x y : A ⊔ B) → (x ≡ y) ≡ ⊔NoConfusion x y
   Path≡⊔NoConfusion = {!!}

   isSet⊔NoConfusion : isSet A → isSet B → (x y : A ⊔ B) → isProp (⊔NoConfusion x y)
   isSet⊔NoConfusion = {!!}

.. raw:: html

   </details>
   </p>

.. tip::

   If you are tired of writing ``{A B : Type} →`` each time
   you can stick

   .. code::

      private
        variable
          A B : Type

   at the beginning of your ``agda`` file,
   and it will assume ``A`` and ``B`` implicitely
   whenever they are mentioned.
   Make sure it is indented correctly.

Without showing either of these new definitions,
try using them to complete ``isSet⊔``.

.. raw:: html

   <p>
   <details>
   <summary>Hint</summary>

We can use ``pathToFun`` or ``endPt``
to follow how a point of "``isProp`` applied to ``⊔NoConfusion``"
changes into a point of "``isProp`` on the path space ``x ≡ y``".

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Partial solutions</summary>

.. code:: agda

   isSet⊔ : {A B : Type} → isSet A → isSet B → isSet (A ⊔ B)
   isSet⊔ hA hB x y = pathToFun {!!} (isSet⊔NoConfusion hA hB x y)

   isSet⊔' : {A B : Type} → isSet A → isSet B → isSet (A ⊔ B)
   isSet⊔' hA hB x y = endPt {!!} {!!} (isSet⊔NoConfusion hA hB x y)

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Solutions</summary>

.. code:: agda

   isSet⊔ : {A B : Type} → isSet A → isSet B → isSet (A ⊔ B)
   isSet⊔ hA hB x y = pathToFun (cong isProp (sym (Path≡⊔NoConfusion x y)))
                        (isSet⊔NoConfusion hA hB x y)

   isSet⊔' : {A B : Type} → isSet A → isSet B → isSet (A ⊔ B)
   isSet⊔' hA hB x y = endPt (λ A → isProp A) (sym (Path≡⊔NoConfusion x y))
                        (isSet⊔NoConfusion hA hB x y)

.. raw:: html

   </details>
   </p>

Proving ``isSet⊔NoConfusion``
-----------------------------

We will now show that ``⊔NoConfusion`` "is a set".
Locate your definition of ``isSet⊔NoConfusion``
and try proving it.

.. raw:: html

   <p>
   <details>
   <summary>Hint</summary>

We need to case on the points in ``A ⊔ B``.

- If they are both "from ``A``" then we need to show that
  the path spaces in ``A`` are propositions.
- (2 cases) If they are from different spaces then we must show that
  the path spaces in ``⊥`` are propositions.
- If they are both "from ``B``" then it is similar to the first case.

.. raw:: html

   </details>
   </p>

Part 4 - Proving ``Path≡⊔NoConfusion``
======================================

It suffices to make an isomorphism
----------------------------------

Replicate our proof of ``flipPath`` in :ref:`quest 0 <>`,
it suffices to show an isomorphism instead of an equality.
Make this precise in ``1FundamentalGroup/Quest2``.

.. raw:: html

   <p>
   <details>
   <summary>Spoiler</summary>

So that you can follow, we will make a lemma
(you don't have to) :

.. code:: agda

   Path≅⊔NoConfusion : (x y : A ⊔ B) → (x ≡ y) ≅ ⊔NoConfusion x y
   Path≅⊔NoConfusion = {!!}

.. raw:: html

   </details>
   </p>

To prove the isomorphism (for each arbitrary ``x`` and ``y``) we need
four things, which we can extract as local definitions / lemmas using ``where``.

.. raw:: html

   <p>
   <details>
   <summary>Spoiler</summary>

.. code:: agda

  fun : (x y : A ⊔ B) → (x ≡ y) → ⊔NoConfusion x y
  fun x y = {!!}

  inv : (x y : A ⊔ B) → ⊔NoConfusion x y → x ≡ y
  inv x y = {!!}

  rightInv : (x y : A ⊔ B) → section (fun x y) (inv x y)
  rightInv {A} {B} = {!!}

  leftInv : (x y : A ⊔ B) → retract (fun x y) (inv x y)
  leftInv = {!!}

.. raw:: html

   </details>
   </p>

``inv``
-------

First try defining ``inv : (x y : A ⊔ B) → ⊔NoConfusion x y → x ≡ y``.

.. raw:: html

   <p>
   <details>
   <summary>Hint 0</summary>

Check the goal.
You can assume points ``x y : A ⊔ B``
and a point ``h : ⊔NoConfusion x y``.
If you case on ``x`` and ``y``
you might find there are fewer cases than you need.
This is because ``⊔NoConfusion (inl ax) (inr by)``
was defined to be empty, so ``agda`` automatically removes the case.

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Hint 1</summary>

In the case that both points are from ``x`` we need to show that
given a proof ``p : ax ≡ ay`` we get a proof of ``inl ax ≡ inr ay``.
We already have the result that if two points are equal then
their images under a function are equal.

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Solution</summary>

.. code:: agda

   inv : (x y : A ⊔ B) → ⊔NoConfusion x y → x ≡ y
   inv (inl x) (inl y) p = cong inl p
   inv (inr x) (inr y) p = cong inr p

.. raw:: html

   </details>
   </p>

Attempting ``fun``
------------------

We try to define the map forward, which we called ``fun``.
If we assume and case on ``x`` and ``y`` in the disjoint sum then

- When ``x`` and ``y`` are both from ``A`` then
  they will be ``inl ax`` and ``inl ay``,
  so checking the goal we should be required to give a point in
  ``inl x ≡ inl y → x ≡ y``.
  Reading this carelessly one could call this "``inl`` is injective".
- When ``x`` and ``y`` are from different spaces then
  checking the goal, we should be required to give a point in
  ``inl ax ≡ inr by → ⊥``.
  This says there are no paths between the disjoint parts.
- The last case is similar to the first.

We can extract the second case as a lemma :

.. raw:: html

   <p>
   <details>
   <summary>Spoiler</summary>

.. code:: agda


   disjoint : (a : A) (b : B) → inl a ≡ inr b → ⊥
   disjoint a b p = {!!}

.. raw:: html

   </details>
   </p>

which we can prove by constructing a *subsingleton bundle*
over ``A ⊔ B``,
just like we did to prove that ``true ≡ false`` is empty,
in the :ref:`side quest <trueNequivFalse>`.
In fact this is a generalisation of that result,
and the proof also generalises.

.. raw:: html

   <p>
   <details>
   <summary>Hint</summary>

We make a bundle over the disjoint union
with the starting fiber as ``⊤`` and the
ending fiber as ``⊥``.

.. raw:: html

   </details>
   </p>


.. raw:: html

   <p>
   <details>
   <summary>Solution</summary>

.. code:: agda

   disjoint : (a : A) (b : B) → inl a ≡ inr b → ⊥
   disjoint a b p = endPt bundle p tt where

     bundle : A ⊔ B → Type
     bundle (inl a) = ⊤
     bundle (inr b) = ⊥

.. raw:: html

   </details>
   </p>

The other case is quite problematic.
This is what we want to show

.. code::

  inlInj : (x y : A) → (inl {A} {B} x ≡ inl y) → x ≡ y
  inlInj x y p = {!!}

Here are the problems:

- If we had a map backwards that cancelled ``inl``
  we would be done, but in general this doesn't
  exist. For example, if ``A`` were empty
  and ``B`` had a point then we cannot
  expect to have a map ``A ⊔ B → A``.
- There is nothing to induct on :
  we have no information about ``x y : A``.
  More importantly :

  .. important:: Path induction

    We don't know how to induct on paths.

  Specifically we don't yet know how to map out of a path space
  in general.

To find out how to induct on paths,
complete :ref:`quest 4<linkneeded>` in trinitarianism,
and return to this quest with a completely new perspective.

.. link needed

.. _part5UsingThePropositionalPerspective:

Part 5 - Using the Propositional Perspective
============================================

After learning about the propositional perspective on
equality, we can review some of the things
we showed in a new light :

- ``a ≡ b → ⊥`` can be read as ``a`` is not equal to ``b``
  since assuming a proof that ``a`` is equal to ``b``
  we have a point in the empty space.
- In showing an isomorphism between spaces
  we must show that two functions satisfy ``fun (inv x) ≡ x``
  for each ``x`` in the domain.
  This can now be read as ``fun`` composed with ``inv``
  is equal to the identity on points.
- ``endPt`` (``subst`` for substitute in the library)
  takes a bundle and a proof that ``x ≡ y`` in the base space
  and substitutes ``x`` for ``y``,
  hence replacing a point in the fiber of ``x``
  with a point in the fiber of ``y``.
- ``cong : (f : A → B) → (p : x ≡ y) → f x ≡ f y``
  says that if two points are equal then their images are equal.
- ``true`` is not equal to ``false``
- ``refl`` is not equal to ``loop``
- ``flipPath : Bool ≡ Bool`` is a non-trivial equality
  between ``Bool`` and itself.
- ``inl`` is injective (we still have not shown this yet).

.. side quest?

- ``isProp`` says there is at most one point in the space;
  at most one proof of the proposition.
  Classically propositions are meant to only have a single proof
  ("proof irrelevance"), because for propositions ``A`` and ``B``,
  having implications ``A → B`` and ``B → A`` is enough
  to show ``A ≡ B``.

.. side quest?

- ``isSet`` says between any two points there is at most one path between them,
  i.e. "there is only ``refl``", i.e. the space is disjoint.

We shall apply this perspective to the problem at hand.

``fun``
-------

Now that we know how to induct on paths,
we need to pick a path to induct on.
Continuing with trying to show that ``inl`` is injective
we will notice that path induction does *not* actually work here,
since we have

- a start point ``ax : A``
- a variable end point ``ay : A``
- but the path is in the disjoint union ``inl ax ≡ inl ay``
  not a path in ``A``

We instead take a step back and look at ``fun`` itself.
(You can now abandon ``inlInj`` if you like,
this will become a corollary of the classification.)
We also remove the cases so that we are back to just having

.. side quest?

.. code:: agda

   fun : (x y : A ⊔ B) → (x ≡ y) → ⊔NoConfusion x y
   fun x y = {!!}

You might have noticed by now that we are in the perfect position to
induct on paths in ``x ≡ y``.
Path induction - ``J`` - says that to make
a function ``(x y : A ⊔ B) → (x ≡ y) → ⊔NoConfusion x y``,
it suffices just to give a point in ``⊔NoConfusion x x``.
Formalise the above
(without showing ``⊔NoConfusion x x`` yet) :

.. raw:: html

   <p>
   <details>
   <summary>Spoiler</summary>

.. code:: agda

   fun : (x y : A ⊔ B) → (x ≡ y) → ⊔NoConfusion x y
   fun x y = J (λ y' p → ⊔NoConfusion x y') {!!}

.. raw:: html

   </details>
   </p>

To prove ``⊔NoConfusion x x`` it would be convenient to be able to case on ``x``
so we will extract it as a lemma.
Once you extract and case on ``x`` this it should be quite easy to show.

.. raw:: html

   <p>
   <details>
   <summary>Spoiler</summary>

.. code:: agda

   ⊔NoConfusionSelf : (x : A ⊔ B) → ⊔NoConfusion x x
   ⊔NoConfusionSelf (inl x) = refl
   ⊔NoConfusionSelf (inr x) = refl

.. raw:: html

   </details>
   </p>

``rightInv``
------------

Try to define
``rightInv : (x y : A ⊔ B) → section (fun x y) (inv x y)``.

.. raw:: html

   <p>
   <details>
   <summary>Hint 0</summary>

It is a good idea to case on ``x`` and ``y`` in the space ``A ⊔ B``,
since ``inv`` is the first to take these inputs in here,
and ``inv`` was defined by casing on ``x`` and ``y``.
This should reduce us to just two cases,
like when defining ``inv``.
We will just describe the case when they are both from ``A``.

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Hint 1</summary>

We can use ``J`` to reduce to the case of when the path is ``refl``.
(No proof of the ``refl`` case yet.)

.. raw:: html

   <p>
   <details>
   <summary>Solution</summary>

.. code:: agda

   rightInv : (x y : A ⊔ B) → section (fun x y) (inv x y)
   rightInv {A} {B} (inl x) (inl y) p =
      J (λ y' p → fun {A} {B} (inl x) (inl y') (inv (inl x) (inl y') p) ≡ p) {!!}

We added the implicit arguments ``{A}`` and ``{B}`` so we can actually access them here.
The remaining hole is for showing that

.. code:: agda

   fun (inl x) (inl x) (inv (inl x) (inl x) refl) ≡ refl

.. raw:: html

   </details>
   </p>

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Hint 2</summary>

It would help to make a chain of equalities.
We defined ``inv (inl x) (inl x) refl`` to be ``refl``,
so we only need to show that

.. code:: agda

   fun (inl x) (inl x) refl ≡ refl

Since ``fun`` was defined using ``J`` we need to know how
``J`` computes when it is fed ``refl``.
We :ref:`described this before <JRefl>`, it is called ``JRefl``.

.. missing link

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Solution</summary>

.. code:: agda

   rightInv : (x y : A ⊔ B) → section (fun x y) (inv x y)
   rightInv {A} {B} (inl x) (inl y) p = J (λ y' p → fun {A} {B} (inl x) (inl y') (inv (inl x) (inl y') p) ≡ p)
                        (
                          fun {A} {B} (inl x) (inl x) refl
                        ≡⟨ JRefl {x = inl x} ((λ y' p → ⊔NoConfusion {A} {B} (inl x) y')) _ ⟩
                        -- uses how J computes on refl
                          refl ∎
                        ) p
   rightInv {A} {B} (inr x) (inr y) p = {!!}

.. raw:: html

   </details>
   </p>

``leftInv``
-----------

Try to define ``leftInv``.

.. raw:: html

   <p>
   <details>
   <summary>Hint 0</summary>

We use ``J`` since ``fun`` "happens first".
This should reduce the problem to showing

.. code::

   inv x x (fun x x refl) ≡ refl

.. raw:: html

   <p>
   <details>
   <summary>Solution</summary>

.. code:: agda

   leftInv : (x y : A ⊔ B) → retract (fun x y) (inv x y)
   leftInv x y = J (λ y' p → inv x y' (fun x y' p) ≡ p) {!!}

.. raw:: html

   </details>
   </p>


.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Hint 1</summary>

If you extract what is needed as a lemma
you can case on the variable.
Remember to use ``JRefl`` for the application of ``fun``.

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Solution</summary>

.. code:: agda

   leftInv : (x y : A ⊔ B) → retract (fun x y) (inv x y)
   leftInv x y = J (λ y' p → inv x y' (fun x y' p) ≡ p)
                   (
                     (inv x x (fun x x refl))
                   ≡⟨ cong (inv x x) (JRefl ((λ y' p → ⊔NoConfusion x y')) _) ⟩
                     inv x x (⊔NoConfusionSelf x)
                   ≡⟨ lem x ⟩
                     refl ∎
                   ) where

     lem : (x : A ⊔ B) → inv x x (⊔NoConfusionSelf x) ≡ refl
     lem (inl x) = refl
     lem (inr x) = refl



.. raw:: html

   </details>
   </p>
