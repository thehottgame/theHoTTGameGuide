.. _quest3PiTypes:

******************
Quest 3 - Pi Types
******************

We will try to formulate and prove the statement

.. admonition:: Problem statement

   The sum of two even naturals is even.

Part 0 - Defining Addition 
==========================

To do so we must define ``+`` on the naturals.
Addition takes in two naturals and spits out a natural,
so it should have type ``ℕ → ℕ → ℕ``.

.. code-block:: agda

   _+_ : ℕ → ℕ → ℕ
   n + m = ?

Try coming up with a sensible definition.
It may not look the same as ours.

.. raw:: html

   <p>
   <details>
   <summary>Hint</summary>

``n + 0`` should be ``n`` and ``n + (m + 1)`` should be ``(n + m) + 1``.

.. raw:: html

   </details>
   </p>

Part 1 - The Statement
======================

Now we can make the statement that a sum of even naturals is even in ``agda``.
Make sure it is of the form

.. code:: agda

   Name : Statement
   Name = ?

The statement should be of the form ``(x y : A) → B`` where ``A``
represents the :ref:`subset <totalSpaceAsSubset>` of even naturals
and ``B`` expresses what it means for the "sum of ``x`` and ``y``" to be even.

.. raw:: html

   <p>
   <details>
   <summary>Hint</summary>

Given ``x y : Σ ℕ isEven`` we want to show that their sum
(really the sum of their fist components) is even,
so we should give ``isEven (x .fst + y .fst)``

.. tip::

   ``x .fst`` is another notation for ``fst x``.
   This works for all sigma types.

.. raw:: html

   </details>
   </p>

There are three ways to interpret this:

.. raw:: html

   <p>
   <details>
   <summary>Spoiler</summary>


- For all even naturals ``x`` and ``y``,
  their sum is even.
- ``isEven (x .fst + y .fst)`` is a construction depending on two recipes
  ``x`` and ``y``.
  Given two recipes ``x`` and ``y`` of ``Σ ℕ isEven``,
  we break them down into their first components,
  apply the conversion ``_+_``,
  and form a recipe for ``isEven`` of the result.
- ``isEven (_ .fst + _ .fst)`` is a bundle over the categorical product
  ``Σ ℕ isEven × Σ ℕ isEven`` and ``SumOfEven`` is a *section* of the bundle.
  This means for every point ``(x , y)`` in ``Σ ℕ isEven × Σ ℕ isEven``,
  it gives a point in the fiber ``isEven (x .fst + y .fst)``.

  ..
     (picture)

.. raw:: html

   </details>
   </p>

More generally given ``A : Type`` and ``B : A → Type``
we can form the *pi type* ``(x : A) → B x : Type``
(in other languages ``Π (x : ℕ), B n``),
with interpretations :

- it is the proposition "for all ``x : A``, we have ``B x``",
  and each term of the pi type `is a collection of proofs ``bx : B x``,
  one for each ``x : A``.
- recipes of ``(x : A) → B x`` are made by
  converting each ``x : A`` to some recipe of ``B x``.
  Indeed the function type ``A → B`` is
  the special case where
  the type ``B x`` is not dependent on ``x``.
  Hence pi types are also known as *dependent function types*.
  Note that terms in the sigma type are pairs ``(a , b)``
  whilst terms in the dependent function type are
  a collection of pairs ``(a , b)`` indexed by ``a : A``
- Given the bundle ``B : A → Type``,
  we have the total space ``Σ A B`` which is equipped with a projection
  ``fst : Σ A B → A``.
  A term of ``(x : A) → B x`` is a section of this projection.

We are now in a position to prove the statement. Have fun!

Part 2 - Remarks
================

.. important::

   Once you have proven the statement,
   check out our two ways of defining addition ``_+_`` and ``_+'_``
   (in the solutions).

- Use ``C-c C-n`` to check that they compute the same values
  on different examples.
- Uncomment the code for ``Sum'OfEven`` in the solutions.
  It is just ``SumOfEven`` but with each ``+`` changed to ``+'``.
- Load the file. Does the proof still work?

Our proof ``SumOfEven`` relied on
the explicit definition of ``_+_``,
which means if we wanted to use our proof on
someone else's definition of addition,
it might not work anymore.

.. admonition:: Important Question

   But ``_+_`` and ``_+'_`` compute the same values.
   Are ``_+_`` and ``_+'_`` "the same"? What is "the same"?
