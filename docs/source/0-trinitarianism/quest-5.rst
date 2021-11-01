
*************************
Quest 5 - Dependent Paths
*************************

Part 0 - Mapping Out of the Circle
==================================

In :ref:`quest0WorkingWithTheCircle` we define the circle,
which we work with here.
We recommend also going through the definitions
for ``doubleCover``, ``flipPath`` and ``Flip`` in the same quest.
They will be referred to here.

.. code:: agda

   data S¹ : Type where
     base : S¹
     loop : base ≡ base

In that quest we experience mapping out of ``S¹`` by casing on
a point ``x : S¹``; this was ``doubleCover``,
it was *not a dependent function*.
We give an example of having to construct a map out of ``S¹``
that is dependent on ``x``:

.. code:: agda

   example : (x : S¹) → doubleCover x → doubleCover x
   example = {!!}

.. coming up with a non-trivial non-extremely hard example here was hard lol

We intend for this map to flip each fiber ``doubleCover x``
just like ``Flip : Bool → Bool`` flips ``Bool``.

- We could case on ``x`` like we did in the definition of ``doubleCover``,
  resulting in

  .. code:: agda

     example : (x : S¹) → doubleCover x → doubleCover x
     example base = {!!}
     example (loop i) = {!!}

- Check and fill the first goal for the ``base`` case.

  .. raw:: html

     <p>
     <details>
     <summary>Solution</summary>

  It asks for a map ``Bool → Bool``
  since the fiber ``doubleCover base``
  is by definition ``Bool``.
  We give ``Flip`` since we want it to flip each fiber.

  .. raw:: html

     </details>
     </p>

- In the second case we need to give a map
  ``doubleCover (loop i) → doubleCover (loop i)``,
  which by definition reduces to
  ``flipPath i → flipPath i``.
  It is not immediately obvious what we can do here.
  We don't know how to case on things in ``flipPath i``,
  so we cannot make a map that "flips" it.

We should take a step back and notice what we have.
Firstly ``λ i → doubleCover (loop i) → doubleCover (loop i)``
defines a path in the space of spaces
(it is a function space at each ``i``).
It starts and ends at ``Bool → Bool``.
What the goal requires is a "path" starting
and ending at ``Flip : Bool → Bool``,
and being a point in ``doubleCover (loop i) → doubleCover (loop i)``
at each point.
This is *not* a path in a single space,
but some kind of path that moves along inside our path of spaces.

.. missing picture

.. admonition:: Idea

   What we need is a generalisation of paths :
   we need paths that can move between spaces.

Part 1 - Dependent Paths
========================

Recall that if we have two spaces ``A0 A1 : Type`` (e.g. both ``Bool → Bool``)
and a path ``A : A0 ≡ A1`` between them
(e.g. ``λ i → doubleCover (loop i) → doubleCover (loop i)``),
then any point in ``A0`` can be transported along the path ``A``
to a point in ``A1`` using ``pathToFun`` a.k.a. ``transport``.
Since ``A0`` and ``A1`` are internally *equal*,
one might wonder if we can even consider what it means for points ``x : A0``
and ``y : A1`` to be *equal*, perhaps keeping the path ``A`` in mind somehow
(e.g. ``x`` and ``y`` are both ``Flip``).
We are asking whether the notion of a *dependent path* -
in this case a path dependent on ``A`` - can be made precise.
*Externally* ``x`` and ``y`` belong to different spaces
so it doesn't make sense to ask for the path type ``x ≡ y``,
but HoTT and ``cubical agda`` offer solutions to this.

In HoTT, a workaround is say ``x`` and ``y`` are equal along ``A``
to mean having a path from ``pathToFun A x`` to ``y`` in the space ``A1``
(note that we made a choice here).
This is sensible, since ``pathToFun A x`` is meant to be the point in ``A1``
corresponding to ``x``
under the identification of the spaces ``A0`` and ``A1`` given by ``A``.
Try to define this in ``1FundamentalGroup/Quest5.agda``.

.. raw:: html

  <p>
  <details>
  <summary>Solution</summary>

.. code:: agda

   depPath : {A0 A1 : Type} (A : A0 ≡ A1) (x : A0) (y : A1) → Type
   depPath A x y = pathToFun A x ≡ y

.. raw:: html

  </details>
  </p>

There is a slightly different ``cubical agda`` way of going about this.
Intuitively a path in ``cubical agda`` is a starting point,
an ending point, and something in between that agrees on the boundary.
Thus a path dependent on ``A : A0 ≡ A1`` from ``x`` to ``y`` can be
introduced by giving at each arbitrary ``i : I`` on the "interval"
a point ``t : A i`` such that ``t`` is *externally equal to* ``x`` at the start
and ``y`` at the end.

.. code:: agda

   PathP : (A : I → Type) → A i0 → A i1 → Type

``A`` takes each ``i : I`` to a space ``A i : Type``,
so we can think of ``A`` as a path.
Then ``PathP`` takes ``A``, a point ``x : A i0``
in the starting space and a point ``y : A i1``
in the ending space, and gives the space of dependent paths
along ``A``.
