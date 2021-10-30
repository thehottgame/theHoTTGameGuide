.. _ZIsASet:

************************************
Quest 3 - The Missing Comparison Map
************************************

Recap
=====

In :ref:`quest1LoopSpaceOfTheCircle` we introduced our main
method of proving that the fundamental group
(which we take to be ``loopSpace S¹ base`` for now)
is ``ℤ``,
and in :ref:`quest2ZIsASet` we decided that this
means to show that they are equal spaces.

.. admonition:: The Goal

   .. code:: agda

      loopSpace≡ℤ : loopSpace S¹ base ≡ ℤ
      loopSpace≡Z = {!!}

As usual we will show this via giving an isomorphism,
so we must make comparison maps forward and back.
However, we discovered we could only define the backwards map
*over all of* ``S¹``.
A similar difficulty will arise when we try to show that
the composition of the comparison maps are identities.
Thus from now on we only work over ``S¹``.

We already have ``windingNumber``, the forwards comparison map,
which gives us a map ``loopSpace S¹ base → ℤ`` when applied to ``base``.

.. code:: agda

   windingNumber : (x : S¹) → base ≡ x → helix x

   windingNumber base : loopSpace S¹ base → ℤ

In this quest our goal is to make a map backwards

.. admonition:: Current Goal

   .. code:: agda

      rewind : (x : S¹) → helix x → base ≡ x

Since ``windingNumber`` took a path and found how
many times the path loops around, in general "an integer plus a bit".
We want to make ``rewind`` do the reverse.
So ``rewind`` should take "an integer ``n`` plus a bit",
loop around ``n`` times, then add that corresponding bit,
the path from ``base`` to ``x`` to the end.

Part 0 - Mapping Out of ``S¹``
==============================

We first try making ``rewind`` directly.
If we assume a point ``x : S¹``,
then we can case on what it is.

.. code:: agda

  rewind : (x : S¹) → helix x → base ≡ x
  rewind base = {!!}
  rewind (loop i) = {!!}

If you follow along in ``1FundamentalGroup/Quest3.agda``
you will need to import the things we have defined.

.. admonition:: Importing files

   We have *not* imported ``1FundamentalGroup/Quest0Solutions.agda`` for you,
   which is where ``S¹`` was defined.
   To import either this or your own set of solutions,
   after the ``where`` at the top of the file write

   .. code:: agda

      open import 1FundamentalGroup.Quest0Solutions

   If you load the document, ``agda`` will try to import any file
   with this directory relative to the location of a ``.agda-lib`` file
   specified in ``.agda/libraries``.
   In this case it is relative to the location of ``TheHoTTGame.agda-lib``.

In the case of ``base`` we want a map
from ``helix base`` i.e. ``ℤ``, to ``base ≡ base``.
Try filling this in.

.. raw:: html

   <p>
   <details>
   <summary>Solution</summary>

We want this to be the correct inverse,
described as looping around ``n`` times and adding that extra bit on the end.
However there is nothing to add at the end in this case,
so it should just be ``loop_times``,
which we already defined in :ref:`quest1LoopSpaceOfTheCircle`.

.. raw:: html

  </details>
  </p>

The case of ``loop i`` will be a lot more work.
Recall that for mapping out of the circle,
for the case of having an "arbitrary point" ``loop i`` on ``loop``,
we need to give a point in the codomain, such that it is
(externally equal to) ``loop_times`` on the boundary.
Indeed, checking the goal for the second case we see that we need to give a
map ``succPath i → base ≡ loop i`` (it has reduces ``bundle (loop i)``),
and the constraints below tell us the boundary conditions.

What we really want to do is make a path from ``loop_times`` to itself,
then apply that to ``i``.
We could do this by extracting the path as a lemma and so on,
but what the above really suggests is making a general way of mapping out of ``S¹``.

``outOfS¹``
-----------

We suggest that making a map out of ``S¹`` should just be giving a point
in the codomain and a path from that point to itself.
Try formalizing this in ``1FundamentalGroup/Quest3``,
calling this ``outOfS¹``.

.. this requires knowing what path over is
