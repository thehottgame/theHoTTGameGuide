.. _quest2ComparisonMaps:

*************************
Quest 2 - Comparison maps
*************************

Comparison maps between ``Ω S¹ base`` and ``ℤ``
===========================================

In ``Quest1`` we have defined the map ``loop_times : ℤ → Ω S¹ base``.
Creating the inverse map is difficult without access to the entire circle.
Similarly to how we used ``doubleCover`` to distinguish ``refl`` and ``base``,
the idea is to replace ``Bool`` with ``ℤ``,
allowing us to distinguish between all loops on ``S¹``.
In ``Part0`` and ``Part1`` we will construct one of the two comparison maps
across the whole circle, called ``spinCount``.

The plan is :

1. Define a function ``sucℤ : ℤ → ℤ`` that increases every integer by one
2. Prove that ``sucℤ`` is an isomorphism by constructing
   an inverse map ``predℤ : ℤ → ℤ``.
3. Turn ``sucℤ`` into a path ``sucPath : ℤ ≡ ℤ`` using ``isoToPath``
4. Define ``helix : S¹ → Type`` by mapping ``base`` to ``ℤ`` and
   a generic point ``loop i`` to ``sucPath i``.
5. Use ``helix`` and ``endPt`` to define the map
   ``spinCountBase : base ≡ base → ℤ``
   Intuitively it counts how many times a path loops around ``S¹``.
   a generic point ``loop i`` to ``sucPath i``.
6. Generalize this across the circle.

In this part, we focus on ``1``, ``2`` and ``3``.

``sucℤ``
--------

- Setup the definition of ``sucℤ`` so that it looks of the form :

  .. code:: agda

     Name : TypeOfSpace
     Name inputs = ?

  Compare it with our solutions in ``1FundamentalGroup/Quest1.agda``
- We will define ``sucℤ`` the same way we defined ``loop_times`` :
  by induction.
  Do cases on the input of ``sucℤ``.
  You should have something like :

  .. code:: agda

     sucℤ : ℤ → ℤ
     sucℤ pos n = ?
     sucℤ negsuc n = ?

- For the non-negative integers ``pos n`` we want to map to its successor.
  Recall that the ``n`` here is a point of the naturals ``ℕ`` whose definition is :

  .. code:: agda

     data ℕ : Type where
     zero : ℕ
     suc : ℕ → ℕ

  Use ``suc`` to map ``pos n`` to its successor.
- The negative integers require a bit more care.
  Recall that annoyingly ``negsuc n`` means "``- (n + 1)``".
  We want to map ``- (n + 1)`` to ``- n``.
  Try doing this.
  Then realise "you run out of negative integers at ``-(0 + 1)``"
  so you must do cases on ``n`` and treat the ``-(0 + 1)`` case separately.

  .. raw:: html

     <p>
     <details>
     <summary>Hint</summary>

  Do ``C-c C-c`` on ``n``.
  Then map ``negsuc 0`` to ``pos 0``.
  For ``negsuc (suc n)``, map it to ``negsuc n``.

  </details>
  </p>

- This completes the definition of ``sucℤ``.
  Use ``C-c C-n`` to check it computes correctly.
  E.g. check that ``sucℤ (- 1)`` computes to ``pos 0``
  and ``sucℤ (pos 0)`` computes to ``pos 1``.

``sucℤ`` is an isomorphism
--------------------------

- The goal is to define ``predℤ : ℤ → ℤ`` which
  "takes ``n`` to its predecessor ``n - 1``".
  This will act as the (homotopical) inverse of ``sucℤ``.
  Now that you have experience from defining ``sucℤ``,
  try defining ``predℤ``.
- Imitating what we did with ``flipIso`` and
  give a point ``sucℤIso : ℤ ≅ ℤ``
  by using ``predℤ`` as the inverse and proving
  ``section sucℤ predℤ`` and ``retract sucℤ predℤ``.

``sucℤ`` is a path
------------------

- Imitating what we did with ``flipPath``,
  upgrade ``sucℤIso`` to ``sucℤPath``.

Comparison maps between ``Ω S¹ base`` and ``ℤ`` - ``spinCount``
===============================================================

The ``ℤ``-bundle ``helix``
--------------------------

We want to make a ``ℤ``-bundle over ``S¹`` by
'copying ℤ across the loop via ``sucℤPath``'.
In ``Quest2.agda`` locate

.. code:: agda

   helix : S¹ → Type
   helix = {!!}

Try to imitate the definition of ``doubleCover`` to define the bunlde ``helix``.
You should compare your definition to ours in ``Quest2Solutions.agda``.
Note that we have called this ``helix``, since the picture of this ``ℤ``-bundle
looks like

..
   /.. image:: images/helix.png
   :width: 1000
   :alt: helix
s
Counting loops
--------------

Now we can do what was originally difficult - constructing an inverse map
(over all points).
Now we want to be able to count how many times a path ``base ≡ base`` loops around
``S¹``, which we can do now using ``helix`` and finding end points of 'lifted' paths.
For example the path ``loop`` should loop around once,
counted by looking at the end point of 'lifted' ``loop``, starting at ``0``.
Hence try to define

.. code:: agda

   spinCountBase : base ≡ base → helix base
   spinCountBase = {!!}

Try computing a few values using ``C-c C-n``,
you can try it on ``refl``, ``loop``, ``loop 3 times``, ``loop (- 1) times`` and so on.

Generalising
------------

The function ``spinCountBase``
can actually be improved without any extra work to a function on all of ``S¹``.

.. code:: agda

   spinCount : (x : S¹) → base ≡ x → helix x
   spinCount = {!!}

We will show that this and a general version of ``loop_times`` are
inverses of each other over ``S¹``, in particular obtaining an isomorphism
between ``base ≡ base`` and ``ℤ``.
