.. _quest0TermsAndTypes:

*************************
Quest 0 - Terms and Types
*************************

There are three ways of looking at ``A : Type``.

  - proof theoretically, "``A`` is a proposition"
  - type theoretically, "``A`` is a construction"
  - geometrically / categorically,
    "``A`` is a space and ``Type`` is the category of spaces".

A first example of a type construction is the function type.
Given types ``A : Type`` and ``B : Type``,
we have another type ``A → B : Type`` which can be seen as

  - the proposition "``A`` implies ``B``"
  - the construction ways to convert ``A`` recipes to ``B`` recipes"
  - the space of maps from ``A`` to ``B``,
    i.e. maps from ``A`` to ``B`` correspond to points of ``A → B``.
  - internal hom of the category ``Type``

To give examples of this, lets make some types first.

Part 0 - True / Unit / Terminal object
======================================

.. code:: agda

   data ⊤ : Type where
     tt : ⊤

It reads "``⊤`` is an inductive type with a constructor ``tt``",
with interpretations

- ``⊤`` is a proposition "true" and there is a proof of it, called ``tt``.
- ``⊤`` is a construction "top" with a recipe called ``tt``
- ``⊤`` is the singleton space
- ``⊤`` is a terminal object: every object has a morphism into ``⊤`` given by ``· ↦ tt``

In general, the expression ``a : A`` is read "``a`` is a term of type ``A``",
and has interpretations interpretations,

- ``a`` is a proof of the proposition ``A``
- ``a`` is a recipe for the construction ``A``
- ``a`` is a point in the space ``A``
- ``a`` is a generalised element of the object ``A`` in the category ``Type``.

The above tells you how we *make* a term of type ``⊤``.
Lets see an example of *using* a term of type ``⊤``:

.. code:: agda

   TrueToTrue : ⊤ → ⊤
   TrueToTrue = {!!}

.. admonition:: Unicode

   One can find how to type ⊤ and other unicode symbols using
   our :ref:`guide to emacs and list of emacs commands <emacsCommands>`.

- enter ``C-c C-l`` (this means ``Ctrl-c Ctrl-l``).
  Whenever you do this, ``agda`` will check the document is written correctly.
  This will open the ``*Agda Information*`` window looking like

  .. code::

    ?0 : ⊤ → ⊤
    ?1 : ⊤
    ?2 : ⊤


  This says you have three unfilled holes.

- Now you can fill the first hole.
- Navigate to the hole ``{ }`` using ``C-c C-f`` (forward) or ``C-c C-b`` (backward)
- Enter ``C-c C-r``. The ``r`` stands for *refine*.
  Whenever you do this whilst having your cursor in a hole,
  ``agda`` will try to help you.
- You should see ``λ x → { }``. This is ``agda`` notation for ``x ↦ { }``
  and is called ``λ`` abstraction, think "``λ`` for let".
- Navigate to the new hole
- Enter ``C-c C-,`` (this means ``Ctrl-c Ctrl-comma``).
  Whenever you make this command whilst having your cursor in a hole,
  ``agda`` will check the *goal*.
- The goal (``*Agda information*`` window) should look like

  .. code::

    Goal: ⊤
    —————————————————————————
    x : ⊤

  you have a proof/recipe/generalized element ``x : ⊤``
  and you need to give a proof/recipe/generalized element of ``⊤``
- Write the proof/recipe/generalized element ``x`` of ``⊤`` in the hole
- Press ``C-c C-SPC`` to fill the hole with ``x``.
  In general when you have some term (and your cursor) in a hole,
  doing ``C-c C-SPC`` will tell ``agda`` to replace the hole with that term.
  ``agda`` will give you an error if it cant make sense of your term.
- The ``*Agda Information*`` window should now only have two unfilled holes left,
  this means ``agda`` has accepted your proof.

  .. code::

    ?1 : ⊤
    ?2 : ⊤

There is more than one proof (see ``Quest0Solutions.agda``).
Here is an important one:

.. code:: agda

   TrueToTrue' : ⊤ → ⊤
   TrueToTrue' x = { }

- Navigate to the hole and check the goal.
- Note ``x`` is already taken out for you.
- You can try type ``x`` in the hole and ``C-c C-c``
- ``c`` stands for cases".
  Doing ``C-c C-c`` with ``x`` in the hole
  tells ``agda`` to do cases on ``x``".
  The only case is ``tt``.

One proof says for any term ``x : ⊤`` give ``x`` again.
The other says it suffices to do the case of ``tt``,
for which we just give ``tt``.

.. admonition:: "The same"

   Are these proofs "the same"? What is "the same"?

  (This question is deep and should be unsettling.
  The short answer is that they are *internally* but
  not *externally* the same.)

Built into the definition of ``⊤`` is the way ``agda`` can make a map out of ``⊤``
into another type ``A``, which we have just used.
It says to map out of ``⊤`` it suffices to do the case when ``x`` is ``tt``", or

- the only proof of ``⊤`` is ``tt``
- the only recipe for ``⊤`` is ``tt``
- the only point in ``⊤`` is ``tt``
- the only one generalized element ``tt`` in ``⊤``

Lets define another type.

Part 1 - False / Empty / Initial object
=======================================

.. code::

   data ⊥ : Type where

This reads "``⊥`` is an inductive type with no constructors",
with interpretations

- ``⊥`` is a proposition "false" with no proofs
- ``⊥`` is a construction "bot" with no recipes
- ``⊥`` is the empty space
- There are no generalized elements of ``⊥`` (it is a strict initial object)

We can make a map from ``⊥`` to any other type, in particular into ``⊤``.

.. code:: agda

  explosion : ⊥ → ⊤
  explosion x = {!!}

- Navigate to the hole and do cases on ``x``.

``agda`` knows that there are no cases so there is nothing to do!
(See ``Quest0Solutions.agda``)
Our interpretations:

- "false" implies "true".
  In fact the same proof gives "false" implies anything (principle of explosion)
- One can convert recipes of ``⊥`` to recipes of ``⊤``.
  In fact the same construction gives a recipe of
  any other construction since
  there are no recipes of ``⊥``.
- There is a map from the empty space to the singleton space.
  In fact given any space ``A`` , there is a map
  from the empty space to ``A``.
- ``⊥`` is has a map into ``⊤``.
  This is due to ``⊥`` being initial
  in the category ``Type``.

Part 2 - The natural numbers
============================

We can also encode "natural numbers" as a type.

.. code::

   data ℕ : Type where
     zero : ℕ
     suc : ℕ → ℕ

Our interpretations are:

- ``ℕ`` has no interpretation as a proposition since
  there are "too many proofs" -
  mathematicians classically don't distinguish
  between proofs of a single proposition.
  (ZFC doesn't even mention logic internally,
  but type theory does.)
  In this sense constructions are *proof relevant* types.

- As a construction :

  - ``ℕ`` is a type of construction
  - ``zero`` is a recipe for ``ℕ``
  - ``suc`` takes an existing recipe for ``ℕ`` and gives
    another recipe for ``ℕ``.

- Categorically :
  ``ℕ`` is a natural numbers object in the category ``Type``.
  This means it is equipped with morphisms ``zero : ⊤ → ℕ``
  and ``suc : ℕ → ℕ`` such that
  given any ``⊤ → A → A`` there exist a unique morphism ``ℕ → A``
  such that the diagram commutes:

.. image:: images/nno.png
   :width: 500
   :alt: nno

- Geometrically : ``ℕ`` is a space with a point ``zero``
  and for every point ``n`` in ``ℕ``, there is another point
  ``suc n`` in ``ℕ``.

.. Previous version :
   we will show that ``ℕ`` is a discrete space in
   :ref:`a later arc<isSetNat>`.
   We call discrete spaces *sets*.

To see how to use terms of type ``ℕ``, i.e. to induct on ``ℕ``,
go to :ref:`quest1DependentTypes`.

.. _part3Universes:

Part 3 - Universes
==================

You may have noticed the notational similarities between
``zero : ℕ`` and ``ℕ : Type``.
The type ``Type`` has the following interpretations :

- As a construction :
  any type of construction is a recipe for ``Type``.
- Geometrically :
  ``Type`` is a space of spaces.
  Each individual point in ``Type`` is a space.

This may have lead you to the question, ``Type : ?``.
In type theory, we simply assert ``Type : Type₁``.
But then we are chasing our tail, asking ``Type₁ : Type₂``.
Type theorists make sure that every type
(i.e. anything the right side of ``:``)
itself is a term (i.e. anything on the left of ``:``),
and every term has a type.
So what we really need is

.. code::

   Type : Type₁, Type₁ : Type₂, Type₂ : Type₃, ⋯

These are called *universes*.
The numberings of universes are called *levels*.
It will be crucial that types can be treated as terms.
This will allows us to

- talk about *predicates* i.e. "propositions depending on a variable".
  E.g. the proposition "``n`` is even" depends on a natural number ``n``.
  See the next quest where we elaborate on this example.
- reason about "*structures*" such as "the structure of a group",
  to express "for all groups, ..."
- do category theory without stepping out of the theory.
  (For experts, we have Grothendieck universes.)
- reason about when two types are "the same",
  for example when are two definitions of
  the natural numbers "the same"? What is "the same"?
