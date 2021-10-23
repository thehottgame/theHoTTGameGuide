.. _pathsAndEquality:

****************************
Quest 4 - Paths and Equality
****************************

If you have come here from :ref:`1-fundamental-group`
then have a look at the :ref:`overview <trinitarianismOverview>`
to understand the philosophy of trinitarianism.

So far in :ref:`trinitarianism <0-trinitarianism>`
there has been no mention of "equality";
we have never said what it meant for two
types or two terms to be "the same".
However, in :ref:`Fundamental Group of the Circle <1-fundamentalGroupOfTheCircle>`
we *have* expressed what it means for two *spaces* to look the same,
by creating a path from one space to the next.
Indeed we will take this to be our *definition* of (internal) equality.

We will often adopt the geometric perspective,
but change perspectives when appropriate.

Part 0 - The Identity Type
==========================

The construction
----------------

Given ``A : Type``  and  ``x y : A`` we have a type
``Id x y : Type``, called the *identity type* of ``x`` to ``y``.

.. code:: agda

   data Id {A : Type} : (x y : A) → Type where

     rfl : (x : A) → Id x x

The construction takes in (implicit) argument ``A : Type``,
then for each pair of points ``x y : A`` it returns a space ``Id x y``,
with interpretations :

- ``Id x y`` is the proposition "``x`` equals ``y`` (internally)"
  and for every ``x``, we have a proof ``rfl x`` that
  "``x`` is equal to itself (internally)".
  (Hence the name ``rfl``, which is short for *reflexivity*.)
- The only recipe for the construction ``Id x y`` is given when
  ``x`` is the same recipe as ``y``.
- ``Id x y`` is the space of paths from ``x`` to ``y``, i.e. points
  in the space are paths from ``x`` to ``y`` in ``A``.
  For every point ``x`` in ``A``,
  there is the constant path ``rfl x`` at ``x``.
- ``Id`` is a bundle over ``A × A`` and the diagonal map ``A → A × A``,
  taking ``x ↦ (x , x)``,
  factors through ``Id → A × A``
  (viewing ``Id`` as the total space ``Σ (A × A) Id``).

  .. image:: images/idType.png
     :width: 500
     :alt: idType

.. picture latex https://q.uiver.app/?q=WzAsNCxbMiwwLCJcXHN1bV97KHgseSk6IEEgXFx0aW1lcyBBfSBcXG1hdGhybXtJZH0gKHggLCB5KSJdLFswLDAsIkEiXSxbMiwyLCJBIFxcdGltZXMgQSJdLFs0LDBdLFsxLDAsInggXFxtYXBzdG8gKHgseCxcXG1hdGhybXtyZWZsfSkiXSxbMSwyLCJcXG1hdGhybXtkaWFnb25hbH0iLDJdLFswLDIsIih4LHkscCkgXFxtYXBzdG8gKHgseSkiXV0=

.. admonition:: Internal versus external equality

  In the first perspective use the word "internal"
  since there is also the notion of "external equality"
  that we want to distinguish.
  In short ``x`` and ``y`` are externally equal if
  the computer believes they are the same term,
  i.e. the string of symbols they simplify (normalise) to are
  exactly the same.

  If two terms are externally equal then they are internally equal,
  and the proof that they are internall equal is ``rfl``.
  However, having a proof ``p : Id x y`` is not enough for
  the computer to recognise ``x`` as the same term as ``y``.


Symmetry
--------

For ``Id`` to be a good notion of equality it should at least be
an equivalence relation.
It is reflexive by having ``rfl`` in the definition.
We show that it is symmetric:

.. code:: agda

   idSym : (A : Type) (x y : A) → Id x y → Id y x
   idSym = {!!}

This has interpretations:

- Equality is symmetric
- We can turn recipes for the construction ``Id x y``
  into recipes for the construction ``Id y x``
- Paths can be reversed

Add this to the file ``0Trinitarianism/Quest4.agda``
and try showing it.
We give a detailed explanation in the hints and solution.

.. raw:: html

   <p>
   <details>
   <summary>Hint 0</summary>

Assume having a space ``A``, points ``x y : A`` and
a proof of equality / recipe / path ``p : Id x y``.
It may help to view ``Id x y`` as a construction
to think about how to proceed.

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Hint 1</summary>

If you case on ``p : Id x y``
then you should see the following

.. code:: agda

   idSym : (A : Type) (x y : A) → Id x y → Id y x
   idSym A x .x rfl = {!!}

We interpret this as

- If ``x`` and ``y`` are equal by proof ``p``
  and we want to show something about ``x``
  ``y`` and ``p``, then it suffices to consider
  the case when they are externally equal;
  that ``y`` is literally the term ``x`` and ``p`` is ``rfl``.
- The only recipe we had for the construction ``Id x y``
  is ``rfl``, so we should be able to reduce to this case.
- To map out of ``Id``, viewed as a total space,
  it suffices to map out of the diagonal.

.. image:: images/idRec
   :width: 500
   :alt: idRec

.. raw:: html

   </details>
   </p>

.. raw:: html

  <p>
  <details>
  <summary>Solution</summary>

Since we have reduced to the case for when both points are ``x``,
we can simply supply a point in ``Id x x``.
There is an obvious one.

.. code:: agda

   idSym : (A : Type) (x y : A) → Id x y → Id y x
   idSym A x .x rfl = rfl

.. raw:: html

  </details>
  </p>

.. admonition:: The Geometric Perspective

   We have *not* included a justification via the geometric perspective.
   This is because intuitively it's not quite obvious that to map out
   of the space of paths it suffices to map the constant path.
   We will return to this issue at a later point.

   .. link to justifying J

Transitivity
------------

In ``0Trinitarianism/Quest4.agda``, try to formalize (and then prove)
the following interpretations of the same statement :

- ``Id`` is transitive, which says if ``Id x y`` and ``Id y z`` both hold, then
  so does ``Id x z``.
- recipes for ``Id x y`` and ``Id y z`` can be made into recipes for ``Id x z``.
- paths can be concatenated

.. raw:: html

  <p>
  <details>
  <summary>The statement</summary>

.. code:: agda

   idTrans : (A : Type) (x y z : A) → Id x y → Id y z → Id x z
   idTrans = {!!}

   You may wish to make some of the arguments implicit.
   We could also introduce notation that suggests concatenation:

.. code:: agda

   _*_ : {A : Type} {x y z : A} → Id x y → Id y z → Id x z
   _*_ = {!!}

   We will use the second.

.. raw:: html

  </details>
  </p>

.. raw:: html

  <p>
  <details>
  <summary>Hints</summary>

There are multiple ways of defining this.
Assuming ``p : Id x y`` and ``q : Id y z``
we could

- case on ``p`` and identify ``x`` and ``y``
- case on ``q`` and identify ``y`` and ``z``
- case on both ``p`` and ``q``, identifying all three

.. raw:: html

  </details>
  </p>

.. raw:: html

  <p>
  <details>
  <summary>Solutions</summary>

.. code:: agda

   _*_ : {A : Type} {x y z : A} → Id x y → Id y z → Id x z
   rfl * q = q

   _*0_ : {A : Type} {x y z : A} → Id x y → Id y z → Id x z
   p *0 rfl = p

   _*1_ : {A : Type} {x y z : A} → Id x y → Id y z → Id x z
   rfl *1 rfl = rfl

These three definitions will work slightly differently in practice.
We will use the first of the three,
but you can use whichever you prefer.

.. raw:: html

  </details>
  </p>

Equality is Respected by everything
-----------------------------------

For ``Id`` be really be a good notion of equality we should show that
in some sense all other constructions respect it.
For example we might want to show that for the product of two spaces
we have ``Id A × Id B`` is isomorphic to ``Id (A × B)``.
More formally

.. code:: agda

   id× : {A B : Type} (a0 a1 : A) (b0 b1 : B) → (Id a0 a1 × Id b0 b1) ≅ Id {A × B} ( a0 , b0 ) ( a1 , b1 )

where we have some kind of product of spaces (however you wish to define it)
and some kind of notion of isomorphism.
We will revisit these ideas again in a later quest.

.. missing link to later quest

Part 1 - Mapping out of ``Id``
==============================

Groupoid Laws
-------------

The identity type satisfies (infinity) groupoid laws,
which we have as exercises below.
This aligns well with the geometric perspective of types :
in classical homotopy theory any space has a groupoid structure
and any groupoid can be made into a space.

We describe the groupoid laws and leave their formalisation
and proofs as exercises.
Note that our solutions may differ to yours depending on
your choice of how to define transitivity / concatenation.

We take the geometric perspective :

- ``rfl`` is the left and right identity under concatenation,
  (we can also take ``Id`` as the notion of *equality of paths*)


  .. raw:: html

     <p>
     <details>
     <summary>The statements</summary>

  .. code:: agda

     rfl* : {x y : A} (p : Id x y) → Id (rfl * p) p
     rfl* = {!!}

     *rfl : {x y : A} (p : Id x y) → Id (p * rfl) p
     *rfl = {!!}


  .. raw:: html

     </details>
     </p>

  .. raw:: html

     <p>
     <details>
     <summary>Solutions</summary>

  .. code:: agda

     rfl* : {x y : A} (p : Id x y) → Id (rfl * p) p
     rfl* p = rfl

     *rfl : {x y : A} (p : Id x y) → Id (p * rfl) p
     *rfl rfl = rfl

  Note that we needed to case on the path in the second proof
  due to our definition of concatenation.

  .. raw:: html

     </details>
     </p>

  .. tip::

     If you are tired of writing ``{A : Type} {x y : A}`` each time
     you can stick

     .. code::

        private
          variable
            A : Type
            x y : A

     at the beginning of your ``agda`` file,
     and it will assume ``A``, ``x`` and ``y`` implicitely
     whenever they are mentioned.
     Make sure it is indented correctly.

- ``Sym`` is a left and right inverse.

  .. raw:: html

     <p>
     <details>
     <summary>The statements</summary>

  .. code:: agda

     *Sym : {A : Type} {x y : A} (p : Id x y) → Id (p * Sym p) rfl
     *Sym = {!!}

     Sym* : {A : Type} {x y : A} (p : Id x y) → Id rfl (p * Sym p)
     Sym* = {!!}

  .. raw:: html

     </details>
     </p>

  .. raw:: html

     <p>
     <details>
     <summary>Solutions</summary>

  .. code:: agda

     *Sym : {A : Type} {x y : A} (p : Id x y) → Id (p * Sym p) rfl
     *Sym rfl = rfl

     Sym* : {A : Type} {x y : A} (p : Id x y) → Id rfl (p * Sym p)
     Sym* rfl = rfl

  .. raw:: html

     </details>
     </p>

- Concatenation is associative

  .. raw:: html

     <p>
     <details>
     <summary>The statement</summary>

  .. code:: agda

     Assoc : {A : Type} {w x y z : A} (p : Id w x) (q : Id x y) (r : Id y z)
             → Id ((p * q) * r) (p * (q * r))
     Assoc = {!!}
   
  .. raw:: html

     </details>
     </p>

  .. raw:: html

     <p>
     <details>
     <summary>Solution</summary>

  .. code:: agda

     Assoc : {A : Type} {w x y z : A} (p : Id w x) (q : Id x y) (r : Id y z)
             → Id ((p * q) * r) (p * (q * r))
     Assoc rfl q r = rfl

  .. raw:: html

     </details>
     </p>

Recursor - The Mapping Out Property of ``Id``
---------------------------------------------

We may wish to extract the way we have made maps out of the identity type :

.. admonition:: Mapping out property of ``Id``

   Assuming a space ``A`` and a point ``x : A``.
   Given a bundle ``M : (y : A) (p : Id x y) → Type`` over the "space of paths out of ``x``",
   in order to make a map ``{y : A} (p : Id x y) → M y p``,
   it suffices to give a point in ``M x refl``.
   This is traditionally called the "recursor" of ``Id``.
   (We have still not justified this geometrically.)

For example, in order to prove ``*Sym : {A : Type} {x y : A} (p : Id x y) → Id (p * Sym p) rfl``,
we would choose our bundle ``M`` to be ``λ y p → Id (p * Sym p) rfl``,
taking each ``y : A`` and ``p : Id x y`` to the space of paths from ``(p * Sym p)`` to ``rfl``
in ``Id x x``.
When we proved this in the previous section,
``agda`` figured out what ``M`` needed to be and just asked for a proof of the case
``M x rfl``.

In ``0Trinitarianism/Quest4.agda``, try formalising the mapping out property,
and call it ``outOfId``.

.. raw:: html

   <p>
   <details>
   <summary>The statement</summary>

.. code:: agda

   outOfId : (M : (y : A) → Id x y → Type) → M x rfl
     → {y : A} (p : Id x y) → M y p
   outOfId = {!!}

Note that we have used the symbol ``y`` in the type of ``M``,
but it really is just a local variable and will not appear outside that bracket.

.. raw:: html

  </details>
  </p>

.. raw:: html

   <p>
   <details>
   <summary>Solution</summary>

.. code:: agda

   outOfId : (M : (y : A) → Id x y → Type) → M x rfl
     → {y : A} (p : Id x y) → M y p
   outOfId M h rfl = h

The proof is of course just casing on the path ``p``,
as we are trying to extract that idea.

.. raw:: html

  </details>
  </p>

Part 2 - The Path Space
=======================

If you came here from the quest on :ref:`fundamentalGroupOfTheCircle`
then you may be wondering why there has not been any mention of
the *path space* ``x ≡ y``.
The reason is that whilst ``≡`` and ``Id`` are meant to represent the same idea,
the implementation of ``Id`` is simple - we were able to write it down;
whereas the implementation of ``≡`` is "external",
and purely existing in ``cubical agda``.
In this part we will show that the two are the same,
and from this point onwards we will only use ``≡`` for equality and paths
(as is the convention in the `cubical library <https://github.com/agda/cubical>`_).

.. admonition:: The goal

   Given two points ``x y : A``,
   the path type ``x ≡ y`` is equal to ``Id x y``.
   Where we take the notion of "equal to" as
   giving a point in ``(x ≡ y) ≡ (Id x y)``.

We will only use these axioms about ``≡``

- If ``x`` is a point in some space then ``refl`` is a proof of ``x ≡ x``.
- The mapping out property, called ``J`` :

  .. code:: agda

     J : (M : (y : A) → x ≡ y → Type) → M x refl
       → {y : A} (p : x ≡ y) → M y p

  This looks exactly like ``outOfId``.
- The mapping out property applied to ``refl`` :

  .. code:: agda

     JRefl : (M : (y : A) → x ≡ y → Type) (h : M x refl)
       → J M h refl ≡ h

  This says that when we feed ``refl`` to ``J M h`` it indeed gives us
  what we expect - something equal to ``h``.
  Unfortunately this ``J M h refl`` is not *externally equal* to ``h``,
  though that is a ``cubical agda`` issue and not a HoTT issue.
- Univalence : if two spaces are isomorphic then they are equal.
  We explain isomorphism and justify univalence geometrically in
  :ref:`Quest 0 of the Fundamental Group arc<part2DefiningFlipPathViaUnivalence>`.

  .. code:: agda

     isoToPath : A ≅ B → A ≡ B

Try to formalise the statement that the two are equal.

.. raw:: html

   <p>
   <details>
   <summary>The statement</summary>

.. code:: agda

   Path≡Id : (x ≡ y) ≡ (Id x y)
   Path≡Id = {!!}

.. raw:: html

   </details>
   </p>

We try to reduce this to giving an isomorphism.
This involves a lot of small steps,
which we split up into hints.

.. Hint 0

.. raw:: html

   <p>
   <details>
   <summary>Hint 0</summary>

You can write ``isoToPath`` in the hole and "refine".
Refining again will make it ask for the four components
in the proof of an isomorphism.

.. code:: agda

   Path≡Id : (x ≡ y) ≡ (Id x y)
   Path≡Id = isoToPath (iso {!!} {!!} {!!} {!!})

.. raw:: html

   </details>
   </p>

.. Hint 1

.. raw:: html

   <p>
   <details>
   <summary>Hint 1</summary>

To make an isomorphism we need to make maps forwards and backwards,
these go in the first two holes.

.. code:: agda

   Path→Id : x ≡ y → Id x y
   Path→Id = {!!}

   Id→Path : Id x y → x ≡ y
   Id→Path = {!!}

.. raw:: html

   </details>
   </p>

.. Hint 2

.. raw:: html

   <p>
   <details>
   <summary>Hint 2</summary>

To make the map forwards we will need to use ``J`` - the mapping
out property of the path space.
To map backwards we can use ``outOfId`` or just case on a path.

.. code:: agda

   Path→Id : x ≡ y → Id x y
   Path→Id {A} {x} = J {!!} {!!}

   Id→Path : Id x y → x ≡ y
   Id→Path rfl = {!!}

For the first, in order to state the motive we need the implicit arguments
``A`` and ``x``.

.. raw:: html

  <p>
  <details>
  <summary>Solution</summary>

.. code:: agda

   Path→Id : x ≡ y → Id x y
   Path→Id {A} {x} = J (λ y p → Id x y) rfl

   Id→Path : Id x y → x ≡ y
   Id→Path rfl = refl

.. raw:: html

  </details>
  </p>

.. raw:: html

   </details>
   </p>

.. Hint 3

.. raw:: html

   <p>
   <details>
   <summary>Hint 3</summary>

Filling in what we have so far and extracting the relevant lemmas
we have

.. code:: agda

  Path≡Id : (x ≡ y) ≡ (Id x y)
  Path≡Id {A} {x} {y} = isoToPath (iso Path→Id Id→Path rightInv leftInv) where

     rightInv : section (Path→Id {A} {x} {y}) Id→Path
     rightInv = {!!}

     leftInv : retract (Path→Id {A} {x} {y}) Id→Path
     leftInv = {!!}

We have filled in the necessary implicit arguments for you.

.. raw:: html

   </details>
   </p>

.. Hint 4

.. raw:: html

   <p>
   <details>
   <summary>Hint 4</summary>

Since ``section Path→Id Id→Path`` will first take in ``p : Id x y``
we give such a ``p`` and case on it.
It should of course just turn into ``rfl``.

Since ``retract Path→Id Id→Path`` will first take in ``p : x ≡ y``
we directly use ``J``.

.. code:: agda

  Path≡Id : (x ≡ y) ≡ (Id x y)
  Path≡Id {A} {x} {y} = isoToPath (iso Path→Id Id→Path rightInv leftInv) where

     rightInv : section (Path→Id {A} {x} {y}) Id→Path
     rightInv rfl = {!!}

     leftInv : retract (Path→Id {A} {x} {y}) Id→Path
     leftInv = J {!!} {!!}


.. raw:: html

   </details>
   </p>

.. Hint 5

.. raw:: html

   <p>
   <details>
   <summary>Hint 5</summary>

Checking the goal for ``rightInv`` we should see it requires a point in
``Path→Id (λ _ → x) ≡ rfl``, which is the same as ``Path→Id refl ≡ rfl``.
What's happened is ``agda`` knows that ``Id→Path rfl`` is just ``refl``
(they are externally equal), so instead of asking for a point of
``Path→Id (Id→Path rfl) ≡ rfl`` it just asks for a proof of the reduced version.
(In our heads we reduce ``(λ _ → x)`` to ``refl`` but ``agda`` does the opposite.)

We extract the above result as a lemma :

.. code:: agda

  Path→IdRefl : Path→Id (refl {x = x}) ≡ rfl
  Path→IdRefl = {!!}

.. raw:: html

   <p>
   <details>
   <summary>Solution</summary>

Since ``Path→Id`` uses ``J``,
the only thing we can do here is use ``JRefl`` :

.. code:: agda

  Path→IdRefl : Path→Id (refl {x = x}) ≡ rfl
  Path→IdRefl {x = x} = JRefl (λ y p → Id x y) rfl

.. raw:: html

   </details>
   </p>

.. raw:: html

   </details>
   </p>

.. Hint 6

.. raw:: html

   <p>
   <details>
   <summary>Hint 6</summary>

For ``leftInv``, giving the correct motive requires knowing what ``retract`` says.
It should look like

.. code:: agda

   leftInv : retract (Path→Id {A} {x} {y}) Id→Path
   leftInv = J (λ y p → Id→Path (Path→Id p) ≡ p) {!!}

Checking the goal we should see it requires a point in
``Id→Path (Path→Id refl) ≡ refl``.
It should be that we just can replace ``Path→Id refl`` with ``rfl``
using our lemma ``Path→IdRefl : Path→Id refl ≡ rfl`` -
but we haven't proven anything about paths yet!
Let us do so now : if ``f : A → B`` is a function (in our case ``Id→Path``)
then if two of its inputs are the same ``x ≡ y`` then so are the outputs,
``f x ≡ f y``.

.. code::

   cong : (f : A → B) (p : x ≡ y) → f x ≡ f y
   cong = {!!}

We can prove this directly using ``J`` or via ``Id``.

.. raw:: html

  <p>
  <details>
  <summary>Solutions</summary>

.. code:: agda

   cong : (f : A → B) (p : x ≡ y) → f x ≡ f y
   cong {x = x} f = J (λ y p → f x ≡ f y) refl

   cong' : (f : A → B) (p : x ≡ y) → f x ≡ f y
   cong' f p = Id→Path (Cong f (Path→Id p))

.. raw:: html

  </details>
  </p>

.. HERE

.. code:: agda

  leftInv : retract (Path→Id {A} {x} {y}) Id→Path
  leftInv = J (λ y p → Id→Path (Path→Id p) ≡ p)
    (
      Id→Path (Path→Id refl)
    ≡⟨ cong (λ p → Id→Path p) Path→IdRefl ⟩
      Id→Path rfl
    ≡⟨ refl ⟩
      refl ∎
    )


.. raw:: html

   </details>
   </p>

Part 3 - Justifying the Mapping Out Property Geometrically
==========================================================



..
   - exercise on mapping out of Id
     - note that geometric interpretation fails
       and promise justification later
     - defining sym, trans
     - groupoid laws A.K.A. types are infinity groupoids hence
       geometric interpretation of types
   - justifyig J geometrically
     - transport + paths out of x contractible to refl x
     - J and JRefl as the recursor and "computational rule"
       for path type
     - exercise : Id x y ≡ Path x y
   - paths in various types
     - pi types
       - funExt
     - sigma types
       - heterogenous paths§
     - universe ? ? ? univalence




.. admonition:: The path type is external

   We have not written out a construction of the path type
   like we have with ``⊤``, ``⊥`` or ``ℕ``.
   This is because the path type is "external" to the type theory,
   and is a ``cubical agda`` specific construction.

Instead , we claim that it looks exactly like the following :

.. code:: agda

   data _≣_ {A : Type} : (x y : A) → Type where

     rfl : (x : A) → x ≣ x



Part 0 - Refl
=============================

Paths As Equality
-----------------

We first take a philosophical detour, which will soon bring rewards.
Let us try naively interpreting some statements in two ways.

- The first is as usual, reading ``x ≡ y`` as the space of paths and
  ``p : x ≡ y`` as a path ``p`` from ``x`` to ``y``.
- The second is reading ``x ≡ y`` as the proposition ``x`` *equals* ``y`` and
  ``p : x ≡ y`` as a proof ``p`` that ``x`` *equals* ``y``.

Recall that in the ``agda`` library we have

.. code:: agda

   refl : {A : Type} {x : A} → x ≡ x

.. raw:: html

   <p>
   <details>
   <summary>Implicit arguments</summary>

.. tip::

   In ``agda`` we can have a way of introducing
   *implicit* arguments of a function.
   We do that by just using curley braces ``{ }`` instead
   of round braces.
   This is why when we use ``refl`` we don't need to mention
   the inputs ``A`` and ``x``,
   ``agda`` is smart enough to figure them out itself.

.. raw:: html

   </details>
   </p>

We can read this as

- For any space ``A`` and point ``x`` in ``A`` we have a (constant) path
  from ``x`` to itself.
- Reflexivity; for any space ``A`` and point ``x`` in ``A`` we have a proof
  that ``x`` is equal to itself.

We also have the statement

.. code:: agda

   sym : {A : Type} {x y : A} → x ≡ y → y ≡ x

We can read this as

- Paths can be reversed.
- Symmetry; for any space ``A`` point ``x`` and ``y`` in ``A``
  if we have a proof that ``x`` equals ``y`` then
  we can turn that into a proof that ``y`` equals ``x``.

Furthermore we have

.. code:: agda

   _∙_ : {A : Type} {x y z : A} (p : x ≡ y) (q : y ≡ z) → x ≡ z

- We can concatenate paths.
- Transitivity; if we have proofs that
  ``x`` equals ``y`` and ``y`` equals ``z`` then
  we get a proof that ``x`` equals ``z``

We can review what we have shown before in this perspective

- ``a ≡ b → ⊥`` can be read as ``a`` is not equal to ``b``
  since assuming a proof that ``a`` is equal to ``b``
  we have a point in the empty space.
- In showing an isomorphism between spaces
  we must show that two functions satisfy ``fun (inv x) ≡ x``
  for each ``x`` in the domain.
  This can now be read as ``fun`` composed with ``inv``
  is equal to the identity on points.
- ``endPt`` (``subst`` for substitute in the library)
  takes a bundle and a proof that ``x ≡ y`` in the base space
  and substitutes ``x`` for ``y``,
  hence replacing a point in the fiber of ``x``
  with a point in the fiber of ``y``.
- ``cong : (f : A → B) → (p : x ≡ y) → f x ≡ f y``
  says that if two points are equal then their images are equal.
- ``true`` is not equal to ``false``
- ``refl`` is not equal to ``loop``
- ``flipPath : Bool ≡ Bool`` is a non-trivial equality
  between ``Bool`` and itself.


.. important::

   In HoTT the fact that two things are equal
   may not have a unique proof.
   We have seen that ``refl`` and ``loop``
   are both proofs that ``base`` is equal to itself,
   but we showed that these proofs are not equal.
   This is an example of proof relevance;
   that we care about which proof of equality we give.

.. admonition:: Trinitarianism

   It is *not just the path space* that can be
   interpreted as a proposition.
   To have a proper introduction to this perspective
   see our arc on :ref:`trinitarianism <0-trinitarianism>`.

From now on we will switch between these perspectives
depending on which is more appropriate.
The "equality" point of view will help us to motivate important proofs.
