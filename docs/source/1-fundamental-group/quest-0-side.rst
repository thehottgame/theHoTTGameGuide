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
In ``1FundamentalGroup/Quest0SideQuests/Empty.agda`` locate the three definitions
and try to fill them in according to the above.

.. code:: agda

   toEmpty : (A : Type) → Type
   toEmpty A = {!!}

   pathEmpty : (A : Type) → Type₁
   pathEmpty A = {!!}

   isoEmpty : (A : Type) → Type
   isoEmpty A = {!!}

You may have noticed we used ``Type₁`` in the second definition.
To find out what ``Type₁`` is, see the quests on trinitarianism.

..
   link to Trinitarianism

Check that your definitions are the same as ours by comparing with
the solutions in
``1FundamentalGroup/Quest0SideQuests/EmptySolutions.agda``.
We will make maps from ``toEmpty A`` to ``isoEmpty A`` to ``pathEmpty A``
and back to ``toEmpty A``.
First we fill in ``toEmpty→isoEmpty``

.. code:: agda

   toEmpty→isoEmpty : (A : Type) → toEmpty A → isoEmpty A
   toEmpty→isoEmpty A = {!!}

.. admonition:: Tip

   The *recursor* of ``⊥`` is quite useful here.
   The recursor of ``⊥`` says that the empty space has an obvious map
   into any other space;
   given a point in ``⊥`` we can get a point in any space.
   Define the recursor youself or use ``⊥Rec``, which is already there.

.. raw:: html

   <p>
   <details>
   <summary>Hint 0</summary>

- Check the goal to see what we have
  and what we need to give.
- Assume ``f : toEmpty A``
- Refine the goal to see what ``agda`` suggests.

.. raw:: html

   </details>
   </p>

   <p>
   <details>
   <summary>Hint 1</summary>

- We need to give an isomorphism
- To map forwards we need a map from ``A`` to ``⊥``,
  to map backwards we need to map from ``⊥`` to ``A``.
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

In general we have ``isoToPath`` which takes in an isomorphism
and gives a path.

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
- We can follow the point ``x`` along the path ``p`` using ``transport``,
  as we did for ``flipPath`` in :ref:`quest0WorkingWithTheCircle`.

.. _trueNequivFalse:

Proving ``true≢false``
----------------------

Locate ``1FundamentalGroup/Quest0SideQuests/TrueNotFalse.agda``
we will show

.. code:: agda

   true≢false : true ≡ false → ⊥
   true≢false = {!!}

- Assume a path ``h : true ≡ false``
- Define a map from ``Bool`` to ``Type``
  (as a lemma or using
  `where <https://agda.readthedocs.io/en/v2.5.2/language/let-and-where.html#where-blocks>`_),
  that takes ``true`` to ``⊤`` and ``false`` to ``⊥``.
  This is a *subsingleton bundle* over ``Bool``,
  since each *fiber* is ``⊤`` and ``⊥``,
  having only a single or no points.
- We can follow how the point ``tt : ⊤``
  changes along the path ``h`` using ``transport``,
  as we did for ``flipPath`` in :ref:`quest0WorkingWithTheCircle`.
  This should give you a point in the empty space ``⊥``.

Due to the previous side quest :ref:`differentNotionsOfEmpty` this tells us
that the space ``true ≡ false`` is empty.
