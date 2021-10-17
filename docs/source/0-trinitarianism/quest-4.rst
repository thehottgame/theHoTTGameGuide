.. _pathsAndEquality:

****************************
Quest 4 - Paths and Equality
****************************

If you have come here from :ref:`1-fundamental-group`
then have a look at the :ref:`overview <trinitarianismOverview>`
to understand the philosophy of trinitarianism.

So far in :ref:`trinitarianism <0-trinitarianism>`
there has been no mention of "equality";
we have never said what it meant for two
types or two terms to be "the same".
However, in :ref:`Fundamental Group of the Circle <1-fundamentalGroupOfTheCircle>`
we *have* expressed what it means for two *spaces* to look the same,
by creating a path from one space to the next.
Indeed we will take this to be our *definition* of (internal) equality.

Part 0 - The Identity Type
==========================

Given ``A : Type``  and  ``x y : A`` we have a type
``Id x y : Type``, called the *identity type* of ``x`` to ``y``.

.. code:: agda

   data Id {A : Type} : (x y : A) → Type where

     rfl : (x : A) → Id x x

The construction takes in (implicit) argument ``A : Type``,
then for each pair ``x y : A`` it returns a type ``Id x y``,
with four interpretations :

- ``Id x y`` is the proposition "``x`` equals ``y``"
  and for every ``x``, we have a proof ``rfl x`` that
  "``x`` is equal to itself".
  (Hence the name ``rfl``, which is short for *reflexivity*.)
- The only recipe for the construction ``Id x y`` is given when
  ``x`` is the same recipe as ``y``.
- ``Id`` is a bundle over ``A × A`` and the diagonal map ``A -> A × A``
  factors through ``Id -> A × A``.
- ``Id x y`` is the space of paths from ``x`` to ``y``, i.e. points
  in the space are paths from ``x`` to ``y`` in ``A``.
  For every point ``x`` in ``A``,
  there is the constant path ``rfl x`` at ``x``.







.. admonition:: The path type is external

   We have not written out a construction of the path type
   like we have with ``⊤``, ``⊥`` or ``ℕ``.
   This is because the path type is "external" to the type theory,
   and is a ``cubical agda`` specific construction.

Instead , we claim that it looks exactly like the following :

.. code:: agda

   data _≣_ {A : Type} : (x y : A) → Type where

     rfl : (x : A) → x ≣ x



Part 0 - Refl
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
