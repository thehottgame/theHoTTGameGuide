.. _quest5DependentPaths:

*************************
Quest 5 - Dependent Paths
*************************

Part 0 - A motivating example
=============================

In :ref:`quest0WorkingWithTheCircle` we define the circle,
which we work with here.
We recommend also going through the definitions
for ``doubleCover``, ``flipPath`` and ``Flip`` in the same quest.
They will be referred to here.

.. code:: agda

   data S¹ : Type where
     base : S¹
     loop : base ≡ base

In said quest we experience mapping out of ``S¹`` by casing on
a point ``x : S¹``; this was ``doubleCover``,
it was *not a dependent function*,
in the sense that ``doubleCover : (x : S¹) → Type`` where ``Type``
does *not* depend on ``x``.
We give an example of having to construct a map out of ``S¹``
that *is* dependent on ``x``:

.. code:: agda

   example : (x : S¹) → doubleCover x → doubleCover x
   example = {!!}

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
  Case on things in ``flipPath i`` is not an option
  for instance.

We should take a step back and notice what we have.
Firstly ``λ i → doubleCover (loop i) → doubleCover (loop i)``
defines a path in the space of spaces
(it is a function space at each ``i``).
It starts and ends at ``Bool → Bool``.

On the other hand, the goal requires is a "path" starting
and ending at ``Flip : Bool → Bool``,
and being a point in ``p i : doubleCover (loop i) → doubleCover (loop i)``
at each point.
It moves along *inside* the path of spaces
``λ i → doubleCover (loop i) → doubleCover (loop i)``.
However, this "path" ``p`` moving along inside the path of spaces
is *not* a path in a single space,
so we need to formalise this new notion.

.. missing picture

.. admonition:: Idea

   What we need is a generalisation of paths :
   we need paths that can move between spaces,
   which we call "dependent paths".

Part 1 - Dependent Paths
========================

In general
----------

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

In HoTT, a workaround is "say ``x`` and ``y`` are equal along ``A``
when we have a path from ``pathToFun A x`` to ``y`` in the space ``A1``"
(note that we made a choice of a path in ``A1`` here;
we could have done the same with ``A0``, with an inverted ``pathToFun``).
This is sensible, since ``pathToFun A x`` is meant to be the point in ``A1``
corresponding to ``x``
under the identification of the spaces ``A0`` and ``A1`` given by ``A``.
Try to define this in ``1FundamentalGroup/Quest5.agda``.

.. raw:: html

  <p>
  <details>
  <summary>Solution</summary>

.. code:: agda

   PathD : {A0 A1 : Type} (A : A0 ≡ A1) (x : A0) (y : A1) → Type
   PathD A x y = pathToFun A x ≡ y

.. raw:: html

  </details>
  </p>

If you like, we can introduce suggestive notation for dependent paths,
but may be harder to read than ``PathD`` :

.. code:: agda

   syntax PathD A x y = x ≡ y along A

So now we can write ``x ≡ y along A`` to mean paths from ``x`` to ``y``
dependent on the path ``A``.

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

We will try to mostly use the HoTT version of paths,
since HoTT is the main discussion here.
So we will assume that the two notions are the same
using an isomorphism ``PathPIsoPathD`` from the library.

.. code:: agda

   PathPIsoPath : (A : I → Type) (x : A i0) (y : A i1) →
      (PathP A x y) ≅ (transport (λ i → A i) x ≡ y)

Let us continue with the example to understand how this works.

Using Dependent Paths
---------------------

Going back to our example,
we need to give a dependent path from ``Flip``
to ``Flip`` - dependent on the path ``λ i → flipPath i → flipPath i``
in the space of spaces.
Let us extract this as a lemma :

.. code::

   example : (x : S¹) → doubleCover x → doubleCover x
   example base = Flip
   example (loop i) = p i where

     p : PathP (λ i → flipPath i → flipPath i) Flip Flip
     p = {!!}

At point ``loop i`` on the loop, we give
the point ``p i`` in ``flipPath i → flipPath i``.
Note that ``PathP`` needs to know which path we are depending on,
and that is the first piece of data it takes in.

Now, instead of giving a ``PathP``, as ``agda`` natively prefers,
we will give a ``PathD``, using ``PathPIsoPathD``.
``PathPIsoPathD`` will give us an isomorphism,
but we only want the map backwards -
taking a ``PathD`` and giving us a ``PathP``.
To do so we write ``_≅_.inv`` in the hole and refine.
It knows that the goal is a ``PathP``,
so it should reduce to

.. code:: agda

  p : PathP (λ i → flipPath i → flipPath i) Flip Flip
  p = _≅_.inv {!!} {!!}

Check the goals,
in the first it should now be asking for an isomorphism,
which we give by refining with ``PathPIsoPathD``,
the second hole depends on the first,
so it will make more sense when we can come back to it later.

.. code:: agda

  p : PathP (λ i → flipPath i → flipPath i) Flip Flip
  p = _≅_.inv (PathPIsoPathD {!!} {!!} {!!}) {!!}

Now try to give ``PathPIsoPathD``
the necessary inputs.

.. raw:: html

  <p>
  <details>
  <summary>Solution</summary>

It just needs to know what path we want to be dependent over,
the starting point, and the ending point.

.. code:: agda

  p : PathP (λ i → flipPath i → flipPath i) Flip Flip
  p = _≅_.inv (PathPIsoPathD (λ i → flipPath i → flipPath i) Flip Flip) {!!}

.. raw:: html

  </details>
  </p>

Checking the final hole we see that we need a path from the function
``pathToFun (λ i₁ → flipPath i₁ → flipPath i₁) Flip``
to the function ``Flip``.
This is now just a normal path in ``Bool → Bool``.
We refrain from spoiling the rest of the proof.

.. raw:: html

  <p>
  <details>
  <summary>Hint</summary>

To prove that two functions are the same we can use ``funExt``
to just check they are the same at each point.
Naturally, we extract this as a lemma so that we can case on the point in ``Bool``.

Reminding ourselves of what ``flipPath`` looks like, and what ``pathToFun`` does,
we should be able to guess what the values on each side turn out to be.

.. raw:: html

  </details>
  </p>

.. raw:: html

  <p>
  <details>
  <summary>Solutions</summary>

.. code:: agda

   example : (x : S¹) → doubleCover x → doubleCover x
   example base = Flip
   example (loop i) = p i where

     lem : (x : Bool) → pathToFun (λ i → flipPath i → flipPath i) Flip x ≡ Flip x
     lem false = refl
     lem true = refl

     p : PathP (λ i → flipPath i → flipPath i) Flip Flip
     p = _≅_.inv (PathPIsoPathD (λ i → flipPath i → flipPath i) Flip Flip) (funExt lem)

.. raw:: html

  </details>
  </p>

.. _mappingOutOfTheCircle:

Mapping out of the circle
-------------------------

We might want to generalize the above process once and for all so that
we can map out the circle with greater ease.
We suggest that to map out of the circle into a bundle over the circle
```B : S¹ → Type``,
it suffices to give a point ``b : B base`` to map ``base`` to,
and to give a ``PathD`` dependent on ``B`` and ``loop`` which
starts and ends at ``b``.

Try to formalise and prove this in the quest.

.. raw:: html

  <p>
  <details>
  <summary>Hint</summary>

You need not, but we found it is convenient to define one for each
``PathP`` and ``PathD``.
The first is of course trivial.

.. code:: agda

   outOfS¹P : (B : S¹ → Type) → (b : B base) → PathP (λ i → B (loop i)) b b → (x : S¹) → B x
   outOfS¹P B b p base = b
   outOfS¹P B b p (loop i) = p i

   outOfS¹D : (B : S¹ → Type) → (b : B base) → b ≡ b along (λ i → B (loop i))
      → (x : S¹) → B x
   outOfS¹D B b p x = {!!}

The next we can define using the first, using ``PathPIsoPathD``.

.. raw:: html

  </details>
  </p>

.. raw:: html

  <p>
  <details>
  <summary>Solution</summary>

.. code:: agda

   outOfS¹D : (B : S¹ → Type) → (b : B base) → b ≡ b along (λ i → B (loop i))
      → (x : S¹) → B x
   outOfS¹D B b p x = outOfS¹P B b (_≅_.inv (PathPIsoPathD (λ i → B (loop i)) b b) p) x

.. raw:: html

  </details>
  </p>

.. admonition:: Cases / Induction / Recursors / Universal properties

   In general, given a higher inductive type we will always
   have the above process, which can be interpreted in the following ways :

   - It is casing on where the term came from or where the proof came from.
     For example to map out of the proposition "``A`` or ``B``"
     we can case on if the proof came from ``A`` or from ``B``.
     To map out of "``A`` and ``B``" we can case on the proof
     and it must give us a pair, proving both.
   - It is induction on the inductively defined type.
     This was exemplified in our
     :ref:`discussion on the naturals <part0PredicatesDependentConstructionsBundles>`.
   - It is the mapping out property of the type, commonly called *the recursor*,
     and it just considers what recipes went into making the type.
     For example the only recipes that went into making the circle
     are ``base`` and ``loop``.
   - Often this can be seen as a universal property.
     For example the universal property of disjoint sums (a.k.a coproducts a.k.a "or")
     can be seen as saying
     "to map out of ``A ⊔ B`` it suffices to give a
     map out of ``A`` and a map out of ``B``"

We should verify that the this mapping out property does actually give us
what we expect.
For example, we gave it a point ``b : B base`` to map ``base``.
We therefore should expect that it does map ``base`` to ``b``.
Tracing through the definitions we have made,
we should be able to see this is true *externally*.

More explicitly

.. code:: agda

   outOfS¹DBase : (B : S¹ → Type) (b : B base)
     (p : b ≡ b along (λ i → B (loop i)))→ outOfS¹D B b p base ≡ b
   outOfS¹DBase B b p = refl

.. _part2HowPathToFunInteractsWithOtherTypes:

Part 2 - How ``pathToFun`` Interacts with Other Types
=====================================================

When we are coming up with dependent paths between points in
equal spaces connected by some path ``A``,
we end up with needing some idea of what
``pathToFun`` looks like when it goes along the path.
For example, if ``A`` were ``λ i → B i → C i``,
where ``B`` and ``C`` are respectfully paths between spaces,
then we might guess that we can describe ``pathToFun B f``
more explicitly by checking what it does on points.

In this part we will consider different type constructions
and how paths between them convert to functions between them
via ``pathToFun``.
A detailed motivating example can be found
:ref:`here <quest3TheLoopSpaceIsZ>`.

Function spaces
---------------

Suppose we have spaces ``A0 A1 B0 B1 : Type``
and paths ``A : A0 ≡ A1`` and ``B : B0 ≡ B1``.
Then let ``pAB`` denote the path
``λ i → A i → B i : (A0 → B0) ≡ (A1 → B1)``.
We want to figure out what ``pathToFun``
does when it follows a function ``f : A0 → B0`` along the path ``pAB``.

We know by functional extensionality that the function
``pathToFun pAB f : A1 → B1``
should be determined by what it does to terms in ``A1``,
so we can assume ``a1 : A1``.
The idea is we "apply ``f`` by sending ``a1`` back to ``A0``".
Since ``pathToFun (sym A) a1`` is meant to give the point in ``A0``
that "looks like ``a1``", we try applying ``f`` to this point,
then send it across again via the path ``B`` to the point
``f (pathToFun (sym A) a1)`` looks like in ``B1``.
We expect the outcome to be the same.

.. image:: ../1-fundamental-group/images/pathToFunAndPiTypes.png
     :width: 500
     :alt: pathToFunAndPiTypes

Try to formalize and prove this in ``0Trinitarianism/Quest5.agda``.

.. raw:: html

  <p>
  <details>
  <summary>The Statement</summary>

.. code:: agda

   pathToFun→ : {A0 A1 B0 B1 : Type} {A : A0 ≡ A1} {B : B0 ≡ B1} (f : A0 → B0) →
     pathToFun (λ i → A i → B i) f ≡ λ a1 → pathToFun B (f (pathToFun (sym A) a1))
   pathToFun→ = {!!}

There are several ways to state the same idea. We didn't have to reverse the path ``A``
for example.

.. raw:: html

  </details>
  </p>

.. Hint 0

.. raw:: html

  <p>
  <details>
  <summary>Hint 0</summary>

We can induct on both ``A`` and ``B``.

.. raw:: html

   <p>
   <details>
   <summary>Solution for Hint 0</summary>

.. code:: agda

   J (λ A1 A → pathToFun (λ i → A i → B i) f ≡ λ a1 → pathToFun B (f (pathToFun (sym A) a1)))
  (
    J (λ B1 B → pathToFun (λ i → A0 → B i) f ≡ λ a → pathToFun B (f (pathToFun (sym refl) a)))
    (
        pathToFun refl f
      ≡⟨ {!!} ⟩
        (λ a → pathToFun refl (f (pathToFun (sym refl) a))) ∎
    )
    B
  )
  A

.. raw:: html

  </details>
  </p>

.. raw:: html

  </details>
  </p>

.. raw:: html

  <p>
  <details>
  <summary>Hint 1</summary>

There are many small equalities that are needed,
for example, we need how ``sym`` and ``refl`` interact
and what ``pathToFun`` does to ``refl``.
At some point it would be useful to just check that
the functions are equal on terms.

.. raw:: html

  </details>
  </p>

.. raw:: html

  <p>
  <details>
  <summary>Solutions</summary>

.. code:: agda

   pathToFun→ : {A0 A1 B0 B1 : Type} {A : A0 ≡ A1} {B : B0 ≡ B1} (f : A0 → B0) →
     pathToFun (λ i → A i → B i) f ≡ λ a1 → pathToFun B (f (pathToFun (sym A) a1))
   pathToFun→ {A0} {A1} {B0} {B1} {A} {B} f =
     J (λ A1 A → pathToFun (λ i → A i → B i) f ≡ λ a1 → pathToFun B (f (pathToFun (sym A) a1)))
     (
       J (λ B1 B → pathToFun (λ i → A0 → B i) f ≡ λ a → pathToFun B (f (pathToFun (sym refl) a)))
       (
           pathToFun refl f
         ≡⟨ pathToFunReflx f ⟩
           f
         ≡⟨ funExt (λ a →
              f a
            ≡⟨ cong f (sym (pathToFunReflx a)) ⟩
              f (pathToFun refl a)
            ≡⟨ cong (λ p → f (pathToFun p a)) (sym symRefl) ⟩
              f (pathToFun (sym refl) a)
            ≡⟨ sym (pathToFunReflx (f (pathToFun (sym refl) a))) ⟩
              pathToFun refl (f (pathToFun (sym refl) a)) ∎
         )
         ⟩
           (λ a → pathToFun refl (f (pathToFun (sym refl) a))) ∎
       )
       B
     )
     A

.. raw:: html

  </details>
  </p>

More to come in the future
--------------------------

This quest is a work in progress.
