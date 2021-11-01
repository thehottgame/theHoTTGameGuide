.. _quest3TheMissingComparisonMap:

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

   Unlike in the previous quests, we have *not* imported anything for you.
   If you load the file ``agda`` should be complaining that it doesn't know what
   ``S¹`` is.
   You can import ``S¹ ; base ; loop`` from the file ``Cubical.HITs.S1`` in the ``cubical library``,
   by writing

   .. code:: agda

      open import Cubical.HITs.S1 using ( S¹ ; base ; loop )

   at the top of the file (after the ``where``).
   ``Cubical.HITs.S1` is where ``S¹`` was defined in the cubical library
   (this directory is relative to wherever the cubical library ``.agda-lib``
   file is on your computer).
   This will *only* import those three things from that file,
   and is a good idea since we might have overlapping definitions
   (such as ``helix``).

   If you load again it should be complaining about ``helix``,
   which was defined in ``1FundamentalGroup.Quest1``.
   So in a new line add

   .. code:: agda

      open import 1FundamentalGroup.Quest1

   Which should import *everything* from your ``Quest1`` file.
   Load the file to check this works.
   This time it has found the file relative to the HoTT Game library
   ``TheHoTTGame.agda-lib``.

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
In :ref:`quest0WorkingWithTheCircle`
we already saw an example of making a map out of ``S¹``,
by casing on the point in ``S¹``
though it was not a *dependent map*.
To recall, we made :

.. code::

   doubleCover : (x : S¹) → Type -- or simply S¹ → Type
   doubleCover base = Bool
   doubleCover (loop i) = flipPath i

The path ``flipPath i`` was simply a path in (the constant bundle) ``Type``,
which doesn't depend on the value of ``x : S¹``.
However, in our situation we will need a *dependent* path,
since ``helix x → base ≡ x`` is a *dependent* bundle
over ``S¹`` (it depends on which ``x`` we took).
Indeed, checking the goal for the second case we see that we need to give a
map ``succPath i → base ≡ loop i`` (it reduces ``bundle (loop i)`` to ``SuccPath i``),
and the constraints below tell us the boundary condition
that it should be equal to ``loop_times`` at the start and end.

We introduce dependent paths in :ref:`0-trinitarianism`.

..
  missing link

``outOfS¹``
-----------

With the knowledge of dependent paths,
we can come up with a systematic way of mapping out of the circle.
We suggest that making a map out of ``S¹`` should just be giving a point
in the codomain and a dependent path over ``loop`` from that point to itself.
Try formalizing this in ``1FundamentalGroup/Quest3``, calling this ``outOfS¹``.

