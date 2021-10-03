.. _quest2ZIsASet:

****************************
Quest 2 - ``ℤ`` is a Set
****************************

In the :ref:`previous quest<quest3LoopSpaceOfTheCircle>`
we treated the loop space as the fundamental group of the circle.
The reason for doing this is:

- The fundamental group is the *set truncation* of the loop space.
  This says to make the fundamental group we take the space
  built by

  1. Including all the points in ``loopSpace S¹ base``
  2. For any two point ``p0 p1 : loopSpace S¹ base``
     (i.e. loops in ``S¹``) and two paths ``h0 h1 : p0 ≡ p1``
     (i.e. homotopies in ``S¹``)
     we add a path ``q : h0 ≡ h1``.

  This says that we take the loop space and remove all the higher homotopical data,
  so that paths ``h0 h1`` between points ``p0 p1`` are unique up to homotopy.
  We could also view this as any circle in ``loopSpace S¹ base``
  (represented by going along ``h0`` and going back along ``h1``)
  can be filled to make a disk in ``loopSpace S¹ base``
  (represented by the path ``q``).

  .. insert picture

- However, it turns out that ``loopSpace S¹ base`` satisfies 2.
  already - we say it *is a set* -
  so *truncation* does nothing.
- By the end of this entire arc we will have made
  a path in the space ``loopSpace S¹ base ≡ ℤ``.
  In this quest we will show that ``ℤ`` is a set,
  hence using ``pathToFun`` or ``endPt`` we can
  also show that ``loopSpace S¹ base`` is a set.

Hence for this quest we aim to show

.. admonition:: Goal

   .. code:: agda

      isSetℤ : isSet ℤ
      isSetℤ = {!!}

Where

.. code:: agda

   isSet : Type → Type
   isSet A = (x y : A) (p q : x ≡ y) → p ≡ q


Showing ``ℤ`` is a set
----------------------

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
as described in :ref:`quest 1 <definitionOfZ>`

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

Part 0 - Paths as Equality
==========================

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
   the inputs ``A`` and ``x``.

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
  if ``x`` equals ``y`` then ``y`` equals ``x``.

Furthermore we have

.. code:: agda

   _∙_ : {A : Type} {x y z : A} (p : x ≡ y) (q : y ≡ z) → x ≡ z

- We can concatenate paths.
- Transitivity; if ``x`` equals ``y`` and ``y`` equals ``z`` then
  ``x`` equals ``z``

In this perspective we can review what we have shown before

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
  says that the image of a point under application of a function is unique.
- ``true`` is not equal to ``false``
- ``refl`` is not equal to ``loop``
- ``flipPath : Bool ≡ Bool`` is a non-trivial equality
  between ``Bool`` and itself.


.. important::

   In HoTT the fact that two things are equal
   may not have a unique proof.
   As we saw already that ``flipPath`` and ``refl``
   are both proofs that ``Bool`` is equal to itself,
   but computing the image of ``true`` under ``pathToFun``
   using ``flipPath`` and ``refl`` will give us ``false``
   and ``true`` respectively.
   This is an example of proof relevance;
   that we care about which proof of equality we give.

From now on we will switch between these perspectives
depending on which is more appropriate.
The "equality" point of view will help us to motivate important proofs.

Part 1 - Groupoid laws
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
   it suffices to just give a point in ``M x refl``

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

``sym`` is inverts arrows
-------------------------

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

Part 1 - First Attempt at Path Space of Sums / Coproducts
=========================================================

..
   attempt path space of coproduct
   idea for ``J`` : think about recursor of equality

Part 2 - Justifying ``J`` Geometrically
=======================================

.. geometrically realise ``J`` as transport + "refl in centre"

Part 3 - Finishing Path Space of Sums
=====================================
