.. _quest2ComparisonMaps:

******************************************
Quest 2 - counting loops across the circle
******************************************

In ``Quest1`` we have defined the map ``loop_times : ℤ → Ω S¹ base``.
Creating the inverse map is difficult without access to the entire circle.
Similarly to how we used ``doubleCover`` to distinguish ``refl`` and ``base``,
the idea is to replace ``Bool`` with ``ℤ``,
allowing us to distinguish between all loops on ``S¹``.
In this quest we will construct one of the two comparison maps
across the whole circle, called ``windingNumber``.

The plan is :

1. Define a function ``sucℤ : ℤ → ℤ`` that increases every integer by one
2. Prove that ``sucℤ`` is an isomorphism by constructing
   an inverse map ``predℤ : ℤ → ℤ``.
3. Turn the isomorphism ``sucℤ`` into a path
   ``sucPath : ℤ ≡ ℤ`` using ``isoToPath``
4. Define ``helix : S¹ → Type`` by mapping ``base`` to ``ℤ`` and
   a generic point ``loop i`` to ``sucPath i``.
5. Use ``helix`` and ``endPt`` to define the map
   ``windingNumberBase : base ≡ base → ℤ``.
   Intuitively it counts how many times a path loops around ``S¹``.
   a generic point ``loop i`` to ``sucPath i``.
6. Generalize this across the circle.

In this part, we focus on ``1``, ``2`` and ``3``.

Part 0 - Comparison maps between ``Ω S¹ base`` and ``ℤ``
========================================================

Defining ``sucℤ``
-----------------

- Set up the definition of ``sucℤ`` so that it is of the form :

  .. code:: agda

     Name : TypeOfSpace
     Name inputs = ?

  Just writing in the name and the type of the space is enough for now.
  Load the file and check that it is looks like:

  .. raw:: html

     <p>
     <details>
     <summary>Solution:</summary>

  .. code:: agda

     sucℤ : ℤ → ℤ
     sucℤ = ?

  .. raw:: html

     </details>
     </p>

- We will define ``sucℤ`` the same way we defined ``loop_times`` :
  by induction.
  Do cases on the input of ``sucℤ``.
  You should have something like :

  .. raw:: html

     <p>
     <details>
     <summary>Solution:</summary>

  .. code:: agda

     sucℤ : ℤ → ℤ
     sucℤ pos n = ?
     sucℤ negsuc n = ?

  .. raw:: html

     </details>
     </p>

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

  .. raw:: html

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

Part 1 - Winding Number
=======================

The ``ℤ``-bundle ``helix``
--------------------------

We want to make a ``ℤ``-bundle over ``S¹`` by
'copying ℤ across the loop via ``sucℤPath``'.
In ``Quest2.agda`` locate

.. code:: agda

   helix : S¹ → Type
   helix = {!!}

Try to imitate the definition of ``doubleCover`` to define the bundle ``helix``.
You should compare your definition to ours in ``Quest2Solutions.agda``.
Note that we have called this ``helix``, since the picture of this ``ℤ``-bundle
looks like

..
   .. image:: images/helix.png
      :width: 1000
      :alt: helix

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

   windingNumberBase : base ≡ base → helix base
   windingNumberBase = {!!}

.. raw:: html

   <p>
   <details>
   <summary>Hint</summary>

- ``endPt`` evaluates the end point of 'lifted paths'.

.. raw:: html

   </details>
   </p>

Try computing a few values using ``C-c C-n``,
you can try it on ``refl``, ``loop``, ``loop 3 times``, ``loop (- 1) times`` and so on.

Generalising
------------

The function ``windingNumberBase``
can actually be improved without any extra work to a function on all of ``S¹``.

.. code:: agda

   windingNumber : (x : S¹) → base ≡ x → helix x
   windingNumber = {!!}

Try filling this in.
We will show that this and a general version of ``loop_times`` are
inverses of each other over ``S¹``, in particular obtaining an isomorphism
between ``base ≡ base`` and ``ℤ``.

