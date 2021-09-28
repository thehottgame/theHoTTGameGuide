.. _pathsAndEquality:

****************************
Quest 3 - Paths and Equality
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
