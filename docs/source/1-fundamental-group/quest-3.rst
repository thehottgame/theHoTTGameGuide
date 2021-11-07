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
However, we discovered we had to define the backwards map
*over all of* ``S¹``.
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
many times the path loops around, in general "an integer twisted around the helix a bit",
or "an integer plus a bit".
We want to make ``rewind`` do the reverse.
So ``rewind`` should take "an integer ``n`` plus a bit",
loop around ``n`` times, then add that extra corresponding bit,
the path from ``base`` to ``x`` to the end.

Part 0 - Dependent Paths
========================

We first try making ``rewind`` directly.
If we assume a point ``x : S¹``,
then we can case on what it is.

.. code:: agda

  rewind : (x : S¹) → helix x → base ≡ x
  rewind base = {!!}
  rewind (loop i) = {!!}

If you follow along in ``1FundamentalGroup/Quest3.agda``
you will need to do some imports :

.. admonition:: Importing files

   Unlike in the previous quests, we have *not* imported anything for you.
   If you write the above definition and try to
   load the file ``agda`` should be complaining that it doesn't know what
   ``S¹`` is.

   .. code::

      Not in scope:
        S¹
        at ...
      when scope checking S¹

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

   The file containing the definition of the path space is ``Cubical.Foundations.Prelude``.

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
Checking the goal we see that at each point ``loop i``
on the loop, it wants a point in the space
``helix (loop i) → base ≡ (loop i)``,
which it might reduce to ``sucℤPath i → base ≡ (loop i)``
according to the definition of ``helix``.

Collecting these spaces together along this ``i``,
we obtain a loop in the space of spaces based at the space ``ℤ → base ≡ base``
given by

.. code::

  λ i → helix (loop i) → base ≡ (loop i) : (ℤ → base ≡ base) ≡ (ℤ → base ≡ base).

Now collecting the points we need to give into a "path" as well,
we obtain the notion of a *dependent path* :
each point of this "path" belongs to a space along the path of spaces.
We define dependent paths and in general design a way of mapping out of
``S¹`` in :ref:`quest5DependentPaths` from :ref:`0-trinitarianism`.
We assume from now on knowledge of dependent paths.

Using ``outOfS¹``
-----------------

Now that we have a way of mapping out of ``S¹`` (using ``PathD``),
called ``outOfS¹D``,
try to use it to rearrange the work we have to far.

.. raw:: html

  <p>
  <details>
  <summary>Solution</summary>

Originally we have

.. code:: agda

  rewind : (x : S¹) → helix x → base ≡ x
  rewind base = loop_times
  rewind (loop i) = {!!}

Now we rearrange this to

.. code:: agda

  rewind : (x : S¹) → helix x → base ≡ x
  rewind = outOfS¹D (λ x → helix x → base ≡ x) loop_times {!!}

since our bundle over ``S¹`` is ``(λ x → helix x → base ≡ x)``
and our image for ``base`` is ``loop_times``.

.. raw:: html

  </details>
  </p>

Checking the last goal, it remains to give a dependent path of type
``PathD (λ i → sucℤPath i → base ≡ loop i) loop_times loop_times``.
Remembering the definition of ``PathD``,
this should be exactly giving a path
``pathToFun (λ i → sucℤPath i → base ≡ loop i) loop_times ≡ loop_times``,
since ``PathD`` reduces the issue of dependent paths to just paths in
the end space, which is ``ℤ → base ≡ base`` in this case.
Let's make this a chain of equalities :

.. raw:: html

  <p>
  <details>
  <summary>Solution</summary>

.. code:: agda

  rewind : (x : S¹) → helix x → base ≡ x
  rewind = outOfS¹D (λ x → helix x → base ≡ x) loop_times
    (
      pathToFun (λ i → sucℤPath i → base ≡ loop i) loop_times
    ≡⟨ {!!} ⟩
      loop_times ∎
    )

.. raw:: html

  </details>
  </p>

Let us remind ourselves that this means,
taking advantage of the propositional perspective.
The map ``loop_times`` takes an integer and
does ``loop`` that many times.
On the other hand ``pathToFun`` follows how ``loop_times``
changed along the path of spaces ``λ i → sucℤPath i → base ≡ loop i``,
and spits out the corresponding point at the end.
This path of spaces is specifically a path of *function spaces*,
so we need to find a more explicit way of describing what ``pathToFun``
does to spaces of functions.
