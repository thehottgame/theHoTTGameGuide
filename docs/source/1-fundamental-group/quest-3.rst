.. _ZIsASet:

****************************
Quest 3 - ``ℤ`` is a Set
****************************

.. admonition:: Goal

   .. code:: agda

      isSetℤ : isSet ℤ
      isSetℤ = ?

See :ref:`Quest 1 Part 1 <part1HomotopyLevels>` for the definition of ``isSet``.

As a first step, we note that ``ℤ`` actually looks like
two disjoint copies of ``ℕ``, i.e. we have

.. code:: agda

   ℤ≡ℕ⊔ℕ : ℤ ≡ ℕ ⊔ ℕ

where we have the definition of the *disjoint sum of two spaces* as follows

.. code:: agda

   data _⊔_ (A B : Type) : Type where

     inl : A → A ⊔ B
     inr : B → A ⊔ B

Proving ``ℤ ≡ ℕ ⊔ ℕ`` will be done in a :ref:`side quest <intEqNatSumNat>`.

Thus we can break down our goal into two :

.. admonition:: Goal 1 : ``ℕ`` is a set

   .. code:: agda

      isSetℕ : isSet ℕ
      isSetℕ = ?

.. admonition:: Goal 2

   Determine the path space of ``A ⊔ B`` in terms of
   the path space of ``A`` and ``B``

Goal 1 will be handled in a :ref:`side quest <isSetNat>`.
We focus on Goal 2 in this section.

Part 0 - Equality as Paths
==========================

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
   *implicit* variables of a function.
   We do that by just using curley braces ``{ }`` instead
   of round braces.
   This is why when we use ``refl`` we don't need to mention
   the inputs ``A`` and ``x``.

.. raw:: html

   </details>
   </p>

We can read this as

- For any space ``A`` and point ``x`` in ``A`` we have a path
  from ``x`` to itself.
- For any space ``A`` and point ``x`` in ``A`` we have a proof
  that ``x`` is equal to itself; "reflexivity".

We also have the statement

.. code:: agda

   sym : {A : Type} {x : A}


.. refl, symm, trans and groupoid laws


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
