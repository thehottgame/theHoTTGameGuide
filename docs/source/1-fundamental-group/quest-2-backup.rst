.. _quest2ZIsASet:

****************************
Quest 2 - ``ℤ`` is a Set
****************************

.. _part0APropositionalView:

Part 0 - A propositional view
=============================

Paths As Equality
-----------------

We first take a philosophical detour, which will soon bring rewards.
Let us try naively interpreting some statements in two ways.

- The first is as usual, reading ``x ≡ y`` as the space of paths and
  ``p : x ≡ y`` as a path ``p`` from ``x`` to ``y``.
- The second is reading ``x ≡ y`` as the proposition ``x`` *equals* ``y`` and
  ``p : x ≡ y`` as a proof ``p`` that ``x`` *equals* ``y``.

Recall that in the ``agda`` library we have

.. code:: agda

   refl : {A : Type} {x : A} → x ≡ x

.. raw:: html

   <p>
   <details>
   <summary>Implicit arguments</summary>

.. tip::

   In ``agda`` we can have a way of introducing
   *implicit* arguments of a function.
   We do that by just using curley braces ``{ }`` instead
   of round braces.
   This is why when we use ``refl`` we don't need to mention
   the inputs ``A`` and ``x``,
   ``agda`` is smart enough to figure them out itself.

.. raw:: html

   </details>
   </p>

We can read this as

- For any space ``A`` and point ``x`` in ``A`` we have a (constant) path
  from ``x`` to itself.
- Reflexivity; for any space ``A`` and point ``x`` in ``A`` we have a proof
  that ``x`` is equal to itself.

We also have the statement

.. code:: agda

   sym : {A : Type} {x y : A} → x ≡ y → y ≡ x

We can read this as

- Paths can be reversed.
- Symmetry; for any space ``A`` point ``x`` and ``y`` in ``A``
  if we have a proof that ``x`` equals ``y`` then
  we can turn that into a proof that ``y`` equals ``x``.

Furthermore we have

.. code:: agda

   _∙_ : {A : Type} {x y z : A} (p : x ≡ y) (q : y ≡ z) → x ≡ z

- We can concatenate paths.
- Transitivity; if we have proofs that
  ``x`` equals ``y`` and ``y`` equals ``z`` then
  we get a proof that ``x`` equals ``z``

We can review what we have shown before in this perspective

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


.. important::

   In HoTT the fact that two things are equal
   may not have a unique proof.
   We have seen that ``refl`` and ``loop``
   are both proofs that ``base`` is equal to itself,
   but we showed that these proofs are not equal.
   This is an example of proof relevance;
   that we care about which proof of equality we give.

.. admonition:: Trinitarianism

   It is *not just the path space* that can be
   interpreted as a proposition.
   To have a proper introduction to this perspective
   see our arc on :ref:`trinitarianism <0-trinitarianism>`.

From now on we will switch between these perspectives
depending on which is more appropriate.
The "equality" point of view will help us to motivate important proofs.

Geometric and propositional views of ``isSet``
----------------------------------------------

..
   **** teaching we just teach them how to compute
   using J with some motivation
   (how to think about it but not the motivation)
   We will keep elaborations of this in 0-trinitarianism

In the :ref:`previous quest<quest3LoopSpaceOfTheCircle>`
we treated the loop space as the fundamental group of the circle.
The reason for doing this is:

- The fundamental group is the *set truncation* of the loop space.
  This says to make the fundamental group we take the space
  built by

  1. Including all the points from the loop space.
     For us ``refl`` and ``loop`` were in ``loopSpace S¹ base``
     so there will be copies of them in the fundamental group.
  2. For any two points ``h0 h1`` in the loop space
     (for us loops in ``S¹``)
     and two paths ``h0 h1 : p0 ≡ p1``
     (i.e. homotopies of loops in ``S¹``)
     we add a path ``q : h0 ≡ h1``
     (a homotopy of homotopies of loops in ``S¹``).

  This says that we take the loop space and remove all the higher homotopical data,
  so that paths ``h0 h1`` between points ``p0 p1`` are unique up to homotopy.
  We could also view this as any circle in ``loopSpace S¹ base``
  (represented by going along ``h0`` and going back along ``h1``)
  can be filled to make a disk in ``loopSpace S¹ base``
  (represented by the path ``q``).

  Intuitively a "set" is meant to be a bunch of disjoint points.
  However in homotopy type theory anything connected by a path
  to a point is considered to be equal to it,
  hence a "set" is a bunch of disjoint blobs,
  where each blob is contractible to a point,
  i.e. a "set" is a type where any circle (the circle must land in a blob)
  can be filled (the blob is contractible).

  .. insert picture

- It turns out that ``loopSpace S¹ base`` satisfies 2
  already - we say it *is a set* -
  so *set truncation* does nothing.
- By the end of this entire arc we will have made
  a path in the space ``loopSpace S¹ base ≡ ℤ``.
  In this quest we will show that ``ℤ`` is a set,
  hence using ``pathToFun`` or ``endPt`` we will evenutally be able to show
  that ``loopSpace S¹ base`` is a set.

Hence for this quest our goal is

.. admonition:: Goal

   .. code:: agda

      isSetℤ : isSet ℤ
      isSetℤ = {!!}

Where

.. code:: agda

   isSet : Type → Type
   isSet A = (x y : A) (p q : x ≡ y) → p ≡ q


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

Thus we can break down our goal into two :

.. admonition:: Goal 1 : ``ℕ`` is a set

   .. code:: agda

      isSetℕ : isSet ℕ
      isSetℕ = {!!}

.. admonition:: Goal 2

   Determine the path space of ``A ⊔ B`` in terms of
   the path space of ``A`` and ``B``

Goal 1 will be handled in a :ref:`side quest <isSetNat>`.
We focus on Goal 2 in this section.

Part 2 - Groupoid laws
======================

Using this propositional perspective we can prove that any space
looks like a groupoid with
composition as ``_∙_``, the identity at each point as ``refl``,
and inverting arrows as ``sym``.
The key to each of these proofs will be to use ``J``,
which says

.. admonition:: ``J``

   To make a map out of the path type it suffices to
   consider the case when the path is ``refl``.

   Specifically, given ``x : A``,
   and a bundle called the *motive* ``M : (y : A) (p : x ≡ y) → Type`` over
   ``(y : A) × (p : x ≡ y)`` - the space of paths in ``A``
   that start at ``x`` -
   to make a map ``(y : A) (p : x ≡ y) → M y p``
   it suffices to just give a point in ``M x refl`` :

   .. code:: agda

      J : {x : A}
          (M : (y : A) (p : x ≡ y) → Type)
          → M x refl
          → {y : A} (p : x ≡ y) → M y p

   In the perspective that paths are a notion of equality
   this is quite obvious:
   to show ``M`` about things equal to ``x``
   it suffices to just show it for ``x``.
   We assume ``J`` for now and justify it geometrically later on.

   Importantly if you feed ``J`` a motive ``M``,
   a proof ``prefl : M x refl`` and the path ``refl``,
   ``J`` will give you something that is equal to ``prefl`` :

   .. _JRefl:

   .. code:: agda

      JRefl : {x : A}
          (M : (y : A) (p : x ≡ y) → Type)
          → (hrefl : M x refl)
          → J M hrefl ≡ refl

``refl`` is the identity
------------------------

In ``1FundamentalGroup/Quest2.agda`` locate

.. code:: agda

   ∙refl : {A : Type} {x y : A} (p : x ≡ y) → p ∙ refl ≡ p
   ∙refl = {!!}

- :ref:`Check the goal <emacsCommands>`.
- Type ``J`` in the hole and :ref:`refine <emacsCommands>`.
  You should see

  .. code:: agda

     ∙refl : {A : Type} {x y : A} (p : x ≡ y) → p ∙ refl ≡ p
     ∙refl = J {!!} {!!}

  ``agda`` figured out that ``J`` maps into the space
  ``{y : A} (p : x ≡ y) → M y p``,
  so it is now asking for the motive ``M`` and a point in ``M x refl``.
- Check the new holes.
- We want ``M y p`` to be the same as ``p ∙ refl ≡ refl``
  so ``M`` should take any ``y : A`` and ``p : x ≡ y``
  to the space ``p ∙ refl ≡ refl``; this fills the first hole.
- To show the case when ``p`` is ``refl`` we use the
  result from the library

  .. code:: agda

        refl∙refl : refl ∙ refl ≡ refl

  This fills the second hole.

If all was correct, we have just produced a proof that
``refl`` is a right identity.
Similarly,
formulate and prove the statement that ``refl`` is a left identity.

``sym`` inverts arrows
----------------------

In ``1FundamentalGroup/Quest2.agda`` locate

.. code:: agda

   ∙sym : {A : Type} {x y : A} (p : x ≡ y) → p ∙ sym p ≡ refl
   ∙sym = {!!}

- :ref:`Check the goal <emacsCommands>`.
- As before try to :ref:`refine <emacsCommands>` using ``J``.
- Check the new holes and fill in what the motive ``M`` should be,
  as we did for ``∙refl``.
- It remains to fill the last hole, which is to show ``M x refl``.
  You may need to use the result ``symRefl : sym refl ≡ refl``
  from the library.

  .. raw:: html

     <p>
     <details>
     <summary>Spoiler</summary>

  The last hole should be asking you for a proof that ``refl ∙ sym refl ≡ refl``.
  For this we will use a chain of equalities starting at ``refl ∙ sym refl``,
  going to ``refl ∙ refl`` and ending at ``refl``.
  To do so make your code look like

  .. code:: agda

     ∙sym : {A : Type} {x y : A} (p : x ≡ y) → p ∙ sym p ≡ refl
     ∙sym = J (λ y p → p ∙ sym p ≡ refl)
            (
              refl ∙ sym refl
            ≡⟨ ? ⟩
              refl ∙ refl
            ≡⟨ ? ⟩
              refl
            ∎)

  Check the new holes,
  they should be asking for proofs of
  ``refl ∙ sym refl ≡ refl ∙ refl``
  and ``refl ∙ refl`` respectively.

  To prove the first equality you can
  use ``cong`` on the function ``λ p → refl ∙ p``,
  and a proof that the paths on the right are equal ``sym refl ≡ refl``.

  .. raw:: html

     </details>
     </p>

If all was correct, we have just produced a proof that
``sym`` gives right inverses.
Similarly you can formulate and prove that it gives left inverses.

Associativity
-------------

Lastly locate

.. code:: agda

   assoc : {A : Type} {w x : A} (p : w ≡ x) {y : A} (q : x ≡ y) {z : A} (r : y ≡ z)
           → (p ∙ q) ∙ r ≡ p ∙ (q ∙ r)
   assoc {A} = {!!}

We have assumed for you the implicit argument ``{A}`` as you will be needing it.

- Try using ``J`` to reduce to the case when ``p`` is ``refl``.
  Once you have included the "motive" your code should look like

  .. raw:: html

     <p>
     <details>
     <summary>Spoiler</summary>

  .. code::

     assoc : {A : Type} {w x : A} (p : w ≡ x) {y z : A} (q : x ≡ y) (r : y ≡ z)
        → (p ∙ q) ∙ r ≡ p ∙ (q ∙ r)
     assoc {A} = J
        (λ x p → {y z : A} (q : x ≡ y) (r : y ≡ z) → (p ∙ q) ∙ r ≡ p ∙ (q ∙ r))
        {!!}

  .. raw:: html

     </details>
     </p>

- Try to prove the case when ``p`` is ``refl``.

  .. raw:: html

     <p>
     <details>
     <summary>Hint</summary>

  * You may need a chain of equalities.
  * You may need that ``refl`` is a left identity.
  * You may need to use ``cong``.

  .. raw:: html

     </details>
     </p>

We have just shown that composition is associative.
This completes our goal of showing that each space
looks like a groupoid.

Part 3 - ``isSet ℤ``
====================

We want to show that ``ℤ`` is a set,
which we reduce to showing that ``ℕ ⊔ ℕ`` is a set
by the path ``ℤ≡ℕ⊔ℕ`` we made at the beginning.
Intuitively if ``ℕ`` is a set then two disjoint
copies of it should also be a set,
(think about filling spheres on the disjoint sum).

So we first formulate a generalization of this result ``isSet⊔``,
which says if spaces ``A`` and ``B`` are both sets
then so is their disjoint sum.
Please do this in ``1FundamentalGroup/Quest2.agda`` where indicated.
It should look like

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

We can use this to show ``isSet (ℕ ⊔ ℕ)``, using ``isSetℕ : isSet ℕ``,
which will be shown in a :ref:`side quest <isSetNat>`.
Then using either ``pathToFun`` or ``endPt`` you can show
``isSet ℤ`` from ``isSet (ℕ ⊔ ℕ)``,
using the path from ``ℤ`` to ``ℕ ⊔ ℕ`` we made earlier.
Try to set this up after your definition of ``isSet⊔``.

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
   <summary>Hint : using ``pathToFun`` and ``endPt``</summary>

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
is the point in the space ``isSet (ℕ ⊔ ℕ)``.
Which we have :

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

Once this is complete we can go back and work on ``isSet⊔``.

``isProp`` and ``isSet``
------------------------

- We assume ``hA : isSet A``,
  ``hB : isSet B``, and points ``x y : A ⊔ B``.
  Following along in ``agda`` your code should look like

  .. code:: agda

     isSet⊔ : {A B : Type} → isSet A → isSet B → isSet (A ⊔ B)
     isSet⊔ hA hB x y = {!!}

- Check the goal.
  It should be asking for a point in the space ``isProp (x ≡ y)``.
- This is because the slicker definition of ``isSet`` used ``isProp``.

  .. code:: agda

     isProp : Type → Type
     isProp A = (x y : A) → x ≡ y

     isSet : Type → Type
     isSet A = (x y : A) → isProp (x ≡ y)

  Whilst ``isSet A`` says that any circle ``S¹`` can be filled,
  ``isProp A`` - "``A`` is a proposition" -
  says any two points has a path in between; ``S⁰`` can be filled.

We must stop here and consider how to get information on
the path space of ``A ⊔ B`` when our hypotheses are
about the path spaces of ``A`` and ``B`` respectively.
We could try to case on ``x`` and ``y``.

- If ``x`` and ``y`` are both of the form ``inl ax`` and
  ``inl ay`` for ``ax ay : A``,
  then we are reduced to proving ``isProp (inl ax ≡ inl ay)``.
  This *should* be due to ``hA``, which gives us
  ``hA ax ay : isProp (ax ≡ ay)``.
  However somehow we would have to identify the spaces
  ``inl ax ≡ inl ay`` and ``ax ≡ ay``.
- If ``x`` and ``y`` are of the forms ``inl ax`` and ``inr by``
  respectively for ``ax : A`` and ``by : B`` then
  intuitively the space ``inl ax ≡ inr bx`` *should* be empty.
- The other two cases are similar.

The conclusion is that we need some kind of
classification of the path space of disjoint sums.

Path space of disjoint sums
---------------------------

.. admonition:: Path space of disjoint sums

   A path in the the disjoint sum
   should just be a path in one of the two parts.

   Viewed as equality, this says points from ``A``
   cannot be confused with points from ``B``
   or points in ``A`` they were not already equal to.

For now we leave ``isSet⊔`` alone and define a function ``⊔NoConfusion``
that takes two points in ``A ⊔ B`` and returns a space,
which is meant to represent the path space in each case.
Try to describe this space in ``1FundamentalGroup/Quest2.agda``.
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

Then assume points ``x`` and ``y`` in the disjoint sum
and try to case on them.
There should be four cases.

- When both points are from ``A``,
  i.e. they are ``inl ax`` and ``inl ay``,
  then we should give the space ``ax ≡ ay``,
  which we expect to be isomorphic to ``inl ax ≡ inl ay``.
- (Two cases) When each is from a different space we expect the path
  space between them to be empty, so we should give ``⊥``.
- If both are from ``B`` then we should
  replicate what we did in the first case.

Concluding ``isSet⊔``
---------------------

Now we have two of goals :

- "``Path≡⊔NoConfusion``" :
  We need to show that for each ``x y : A ⊔ B``
  the path space is equal to our classification,
  i.e. that ``(x ≡ y) ≡ (⊔NoConfusion x y)``
- "``isSet⊔NoConfusion``" : For ``isSet⊔``, given
  ``hA : isProp A``, ``hB : isProp B`` and ``x y : A ⊔ B``
  we needed to show ``isProp (x ≡ y)``.
  Hence we want to show that under the same assumptions
  ``isProp (⊔NoConfusion x y)``.

Formalise both of these above appropriate places indicated in
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

.. tip:: Local variables

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
to follow how a proof of ``isProp`` on
``⊔NoConfusion`` changes into a proof of in ``isProp``
on the path space ``x ≡ y``
(where proofs are points in a space).

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

We need to case on which points we took ``A ⊔ B``.

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
(you don't have to do this but each part of this proof will be relevant anyway) :

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

``fun``
-------

We will try to define the map forward, which we called ``fun``.
If we assume and case on ``x`` and ``y`` in the disjoint sum then

- When ``x`` and ``y`` are both from ``A`` then
  they will be ``inl ax`` and ``inl ay``,
  so checking the goal we should be required to give a point in
  ``inl x ≡ inl y → x ≡ y``.
  Read propositionally this says ``inl`` is injective.
  You can think about how to do this, but
  we thought this was *hard*,
  especially considering the tech we already have.
- When ``x`` and ``y`` are from different spaces then
  checking the goal, we should be required to give a point in
  ``inl ax ≡ inr by → ⊥``.
  This says there are no paths between the disjoint parts.
  This also seems hard.

The trick is to *not* case on ``x`` and ``y``,
and instead view things propositionally and use ``J``.
Intuitively ``J`` allows us just do show this
when ``x`` and ``y`` are both ``x``,
i.e. give a point in ``⊔NoConfusion x x``.
This is how the above is formalized
(without showing ``⊔NoConfusion x x`` yet):

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

``inv``
=======

Try defining ``inv``.

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

.. raw:: html

   <p>
   <details>
   <summary>Solution</summary>

.. code:: agda

   rightInv : (x y : A ⊔ B) → section (fun x y) (inv x y)
   rightInv {A} {B} (inl x) (inl y) p = J (λ y' p → fun {A} {B} (inl x) (inl y') (inv (inl x) (inl y') p) ≡ p) {!!}

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
   <summary>Hiint 0</summary>

We should use ``J`` since ``fun`` "happens first".
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




..
   Part 3 - First Attempt at Path Space of Sums / Coproducts
   =========================================================


   ..
   attempt path space of coproduct
      idea for ``J`` : think about recursor of equality

   Part 4 - Justifying ``J`` Geometrically
   =======================================

   .. geometrically realise ``J`` as transport + "refl in centre"

   Part 5 - Finishing Path Space of Sums
   =====================================
