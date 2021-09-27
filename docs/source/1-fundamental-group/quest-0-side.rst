.. _quest0SideQuests:

Quest 0 - Side Quests
=====================

.. _differentNotionsOfEmpty:

Different notions of "empty"
----------------------------

Under construction

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
- You can follow how the point ``tt : ⊤``
  changes along the path ``h`` using ``transport``,
  as we did for ``flipPath`` in :ref:`quest0WorkingWithTheCircle`.
  This should give you a point in the empty space ``⊥``.

Due to the previous side quest :ref:`differentNotionsOfEmpty` this tells us
that the space ``true ≡ false`` is empty.
