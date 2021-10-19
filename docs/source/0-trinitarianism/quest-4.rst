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

Part 1 - Groupoid Laws
======================

In this part we look at the algebraic properties of the identity type.
Specifically, we will see that the identity type satisfies (infinity) groupoid
laws.
This aligns well with the geometric perspective of types :
in classical homotopy theory any space has a groupoid structure
and any groupoid can be made into a space.

We will describe the groupoid laws and leave their formalisation
and proofs as exercises.
Note that our solutions may differ to yours depending on
your choice of how to define transitivity / concatenation.

We take the geometric perspective :

- ``rfl`` is the left and right identity under concatenation,
  (we also take ``Id`` as the notion of *equality of paths*)

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
