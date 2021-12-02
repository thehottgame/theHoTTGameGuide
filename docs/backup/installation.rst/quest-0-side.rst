.. _quest0SideQuests:

Quest 0 - Side Quests
=====================

.. _differentNotionsOfEmpty:

Different notions of "empty"
----------------------------

The following are "the same",

- there is a point ``f : A → ⊥`` in the space of functions from ``A`` to the empty space
- there is a path ``p : A ≡ ⊥`` in the space of spaces ``Type`` from ``A`` to the empty space
- there is an isomorphism ``i : A ≅ ⊥`` (input ``\cong`` for ``≅``) of spaces

Here we will take "the same" to mean there are maps from any one to another
(they are "propositionally the same").
We will first define the three.

In ``1FundamentalGroup/Quest0.agda``, uncomment this side quest
and locate these three definitions :

.. code:: agda

   _toEmpty : (A : Type) → Type
   A toEmpty = {!!}

   pathEmpty : (A : Type) → Type₁
   pathEmpty A = {!!}

   isoEmpty : (A : Type) → Type
   isoEmpty A = {!!}

.. NOTE::

   You can use underscores when you name a function.
   ``agda`` will put the inputs in the underscores in order.

.. tip::

   ``agda`` supports unicode symbols such as ``⊥``.
   See :ref:`here <emacsCommands>` for how to insert ``⊥`` and other symbols.

Try to fill them in according to the above.
You may have noticed we used ``Type₁`` in the second definition.
To find out what ``Type₁`` is,
see :ref:`part3Universes` in :ref:`0-trinitarianism`.

Check that your definitions are the same as ours by comparing with
the solutions in
``1FundamentalGroup/Quest0Solutions.agda``.
We will make maps from ``toEmpty A`` to ``isoEmpty A`` to ``pathEmpty A``
and back to ``toEmpty A``.

First we show that the empty space maps into any other space.
This is *very useful* when working with the empty space.

.. code:: agda

   outOf⊥ : (A : Type) → ⊥ → A
   outOf⊥ = {!!}

Try to fill in the definition without looking at the hint.

.. raw:: html

   <p>
   <details>
   <summary>Hint</summary>

Recall the definition of the empty space
being a CW-complex with nothing in it.
We can always :ref:`case <emacsCommands>`
on variable points from CW-complexes.
What cases are there?

.. raw:: html

   </details>
   </p>

We fill in ``toEmpty→isoEmpty``

.. code:: agda

   toEmpty→isoEmpty : (A : Type) → toEmpty A → isoEmpty A
   toEmpty→isoEmpty A = {!!}

.. tip::

   You can use ``where`` to extract lemmas / make local definitions
   like we did in defining ``flipIso``;
   see :ref:`here <part2DefiningFlipPathViaUnivalenceTheIsomorphism>`.

.. raw:: html

   <p>
   <details>
   <summary>Hint 0</summary>

- Check the goal to see what we have
  and what we need to give.
- Assume ``f : toEmpty A`` by putting an ``f``
  before the ``=``.
- Refine the goal to see what ``agda`` suggests.

.. raw:: html

   </details>
   </p>

   <p>
   <details>
   <summary>Hint 1</summary>

- We need to give an isomorphism,
  i.e. a map from ``A`` to ``⊥``,
  and a map from ``⊥`` to ``A``,
  and proofs that these satisfy ``section`` and ``retract`` respectively.
- If we have a point in ``⊥`` then we can get a point in any space.

.. raw:: html

   </details>
   </p>

Try filling in

.. code:: agda

   isoEmpty→pathEmpty : (A : Type) → isoEmpty A → pathEmpty A
   isoEmpty→pathEmpty A = {!!}

.. raw:: html

   <p>
   <details>
   <summary>Hint</summary>

We converted an isomorphism to a path in
:ref:`quest 0 <quest0WorkingWithTheCircle>`.

.. raw:: html

   </details>
   </p>

Lastly try filling in

.. code:: agda

   pathEmpty→toEmpty : (A : Type) → pathEmpty A → toEmpty A
   pathEmpty→toEmpty A = {!!}

- Check the goal
- We can assume a path ``p : pathEmpty A``
- Check the goal again
- Since ``toEmpty A`` as defined as ``A → ⊥`` we can assume a point ``x : A``
- We can follow the point ``x`` along the path ``p`` using ``pathToFun``,
  as we did for ``flipPath`` in :ref:`quest0WorkingWithTheCircle`.

.. _trueNequivFalse:

Proving ``true≢false``
----------------------

Locate ``1FundamentalGroup/Quest0SideQuests/TrueNotFalse.agda``
we will show

.. code:: agda

   true≢false : true ≡ false → ⊥
   true≢false = {!!}

We do this by making a *subsingleton bundle* over ``Bool``
whose fiber over ``true`` is the singleton space ``⊤``
and fiber over ``false`` is the empty space ``⊥``.
The definition of ``⊤`` is

.. code:: agda

   data ⊤ : Type where
     tt : ⊤

- Assume a path ``h : true ≡ false``
- Define a map from ``Bool`` to ``Type``
  (as a lemma or using
  `where <https://agda.readthedocs.io/en/v2.5.2/language/let-and-where.html#where-blocks>`_),
  that takes ``true`` to ``⊤`` and ``false`` to ``⊥``.
  This is a *subsingleton bundle* over ``Bool``,
  since each *fiber* is ``⊤`` and ``⊥``,
  having only a single or no points.
- We can follow how the point ``tt : ⊤``
  changes along the path ``h`` using ``pathToFun``,
  as we did for ``flipPath`` in :ref:`quest0WorkingWithTheCircle`.
  This should give you a point in the empty space ``⊥``.

Due to the previous side quest :ref:`differentNotionsOfEmpty` this tells us
that the space ``true ≡ false`` is empty.

.. _definingCong:

Defining ``cong``
-----------------

Under construction
