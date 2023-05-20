*********************
Quest 4 - Side Quests
*********************

.. _functionalExtensionality:

Functional Extensionality
=========================

We show that two dependent functions
``f g`` being equal is the same as
them being equal when applied to each
value of the domain.
We call one of these directions
*functional extensionality* :

.. code:: agda

   funExt : {B : A → Type} {f g : (a : A) → B a} →
      ((a : A) → f a ≡ g a) → f ≡ g
   funExt = {!!}

Write this up in ``agda`` and have a go at it.

We will cheat and use the native cubical definition of paths
(rather than using our axiomatic approach with ``J`` and ``JRefl`` etc),
since the HoTT proof of this is much more work.
A path in ``cubical agda`` between two points ``x`` and ``y``
in a space ``A`` can be defined by just taking an arbitrary
point ``i`` on the "interval" ``I``, to a point in the space ``A``,
such that the end points agree.
Assuming we have the bundle ``B``, functions ``f g``,
a proof ``h`` of ``(a : A) → f a ≡ ga``,
we can refine, and ``agda`` will assume such an ``i`` for us.

.. code::

   funExt : {B : A → Type} {f g : (a : A) → B a} →
     ((a : A) → f a ≡ g a) → f ≡ g
   funExt {B = B} {f = f} {g = g} h = λ i a → {!!}

Checking the goal you should see something like the following
(we have extracted the important parts):

.. code::

    Goal: B a
  ——————————————————————————————————
  a : A
  i : I
  h : (a₁ : A) → f a₁ ≡ g a₁
  g : (a₁ : A) → B a₁
  f : (a₁ : A) → B a₁
  B : A → Type
  A : Type   (not in scope)
  ———— Constraints ————————————————————————
  ?0 (i = i1) = g a : B a
  ?0 (i = i0) = f a : B a

We break this down :

- ``agda`` has assumed an arbitrary ``i : I`` and ``a : A``,
  and is now asking for a point in ``B a``.
- Let's call whatever we put in the goal ``?0``; it has type ``B a``.
  The constraints say that at the start and end points of ``I``
  (called ``i0`` and ``i1`` respectively) ``0? i`` should be
  ``f a`` and ``g a`` respectively.
- To understand why ``agda`` also gave us an ``a : A``
  we can go back a step, removing ``a``.
  You should see that the goal at that point was a dependent function
  that at the start and end points are ``f`` and ``g`` respectively.
- Try to complete the quest.
  You will need that given a path ``p`` and ``i : I`` along the interval,
  writing ``p i`` gives the corresponding point along the path ``p``.

.. raw:: html

  <p>
  <details>
  <summary>Hint</summary>

The hypothesis ``h`` applied to the point ``a``
gives us a path from ``f a`` to ``g a`` in ``B a``.

.. raw:: html

  </details>
  </p>

.. raw:: html

  <p>
  <details>
  <summary>Solution</summary>

.. code:: agda

  funExt : {B : A → Type} {f g : (a : A) → B a} →
    ((a : A) → f a ≡ g a) → f ≡ g
  funExt B f g h = λ i a → h a i

.. raw:: html

  </details>
  </p>

Now we can promote this to an isomorphism,
hence an equality between ``f ≡ g`` and
``(a : A) → f a ≡ g a``.
Try to formalize and prove this.

.. raw:: html

  <p>
  <details>
  <summary>Solution</summary>

funExtPath : (B : A → Type) (f g : (a : A) → B a) → (f ≡ g) ≡ ((a : A) → f a ≡ g a)
funExtPath {A} B f g = isoToPath (iso fun (funExt B f g) rightInv leftInv) where

  fun : f ≡ g → (a : A) → f a ≡ g a
  fun h = λ a i → h i a

  rightInv : section fun (funExt B f g)
  rightInv h = refl

  leftInv : retract fun (funExt B f g)
  leftInv h = refl

.. raw:: html

  </details>
  </p>

.. _justifyingJ:

Justifying ``J``
================

Work in progress.
