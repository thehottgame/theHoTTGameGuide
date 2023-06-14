.. _quest1LoopSpaceOfTheCircle:

**************************************
Quest 1 - Loop Space of the Circle
**************************************

.. _part0DefinitionOfTheLoopSpace:

Part 0 - Definition of the Loop Space
=====================================

In this quest,
we continue to formalise the problem statement.

.. admonition:: The problem statement

   The fundamental group of ``S¹`` is ``ℤ``.

Intuitively,
the fundamental group of ``S¹`` at ``base``
consists of loops based as ``base`` up to homotopy of paths.
In homotopy type theory,
we have a native description of loops based at ``base`` :
it is the space ``base ≡ base``.

In general the *loop space* of a space ``A`` at a point ``a`` is defined as follows :

.. code:: agda

   loopSpace : (A : Type) (a : A) → Type
   loopSpace A a = a ≡ a

For now, we will treat the loop space of ``S¹`` as the fundamental group.
Later we will understand why this is illegal in general
(the fundamental group is *set truncated*)
but legitimate in this special case
(the loop space of ``S¹`` turns out to be a *set* anyway).

Exercise - ``loop_times``
-------------------------

Clearly for each integer ``n : ℤ`` we have a path
that is "``loop`` around ``n`` times".
Locate ``loop_times`` in ``1FundamentalGroup/Quest1.agda``
(note how ``agda`` treats underscores)

.. code:: agda

   loop_times : ℤ → loopSpace S¹ base
   loop n times = {!!}

.. NOTE::

   You can use underscores when you name a function.
   ``agda`` will put the inputs in the underscores in order.

Try casing on ``n``, you should see

.. code:: agda

   loop_times : ℤ → loopSpace S¹ base
   loop pos n times = {!!}
   loop negsuc n times = {!!}

It says to map out of ``ℤ`` it suffices to
map the non-negative integers (``pos``)
and the negative integers (``negsuc``).
The definition of ``ℤ`` in ``agda`` is

.. _definitionOfZ:

.. code:: agda

   data ℤ : Type where
     pos    : ℕ → ℤ
     negsuc : ℕ → ℤ

It says is ``ℤ`` is two copies of ``ℕ`` where the first
copy represents ``0, 1, 2, ...``,
and the second represents ``-1, -2, ...``
(``negsuc n`` is meant to mean ``- (n + 1)``).
This definition of ``ℤ`` uses the naturals, so try
casing on ``n`` again, you should see

.. code:: agda

   loop_times : ℤ → Ω S¹ base
   loop pos zero times = {!!}
   loop pos (suc n) times = {!!}
   loop negsuc n times = {!!}

It says to map out of ``ℕ`` it suffices to map ``zero`` and
map each successive integer ``suc n`` inductively.
We can do the same with ``negsuc n``,
obtaining four cases.

.. code:: agda

   loop_times : ℤ → Ω S¹ base
   loop pos zero times = {!!}
   loop pos (suc n) times = {!!}
   loop negsuc zero times = {!!}
   loop negsuc (suc n) times = {!!}

These four cases represent :

- If you "loop 0 times" then you stay at ``base``.
- If you "loop n + 1 times", you "loop n times"
  then "loop once more".
- If you "loop -1 times", you "loop once in reverse"
- If you "loop -(n + 2) times", you loop "loop -(n + 1) times"
  then "loop once more in reverse"

Individually

- Try filling the first hole with
  what we get when we loop ``0`` (``pos zero``) times.
- For looping ``pos (suc n)`` times we loop ``n`` times and
  loop once more.
  For this we need composition of paths.

  .. code:: agda

     _∙_ : x ≡ y → y ≡ z → x ≡ z

  Try typing ``_∙_`` or ``? ∙ ?`` in the hole (input ``\.``)
  and refining.
  Checking the new holes you should see that now you need
  to give two loops.

  .. code:: agda

     loop pos (suc n) times = {!!} ∙ {!!}

  Try giving it "``loop n times``" concatenated with ``loop``.
- To "loop in reverse" we use

  .. code:: agda

     sym : x ≡ y → y ≡ x

  Use this to define "loop -1 times".
- For the last case "concatenate loop -(n + 1) times with loop in reverse".

..
   .. raw:: html

      <p>
      <details>
      <summary>Looking up definitions</summary>

   If you don't know the definition of something
   you can look up the definition by sticking your cursor
   on it and pressing ``M-SPC c d`` in *insert mode*
   or ``SPC c d`` in *evil mode*.

   You can use it to find out the definition of ``ℤ`` and ``ℕ``.

   .. raw:: html

      </details>
      </p>

Part 1 - Making a Path From ``ℤ`` to Itself
===========================================

In the previous part we have defined the map ``loop_times : ℤ → Ω S¹ base``.
Creating the inverse map is difficult without access to the entire circle.
Similarly to how we used ``doubleCover`` to distinguish ``refl``
(``Refl`` is now ``refl`` which is more general) from ``loop``,
the idea is to replace ``Bool`` with ``ℤ``,
allowing us to distinguish between all loops on ``S¹``.
In this quest we will construct one of the two comparison maps
across the whole circle, called ``windingNumber``.

The plan is :

1. Define a function ``sucℤ : ℤ → ℤ`` that increases every integer by one
2. Prove that ``sucℤ`` is an isomorphism by constructing
   an inverse map ``predℤ : ℤ → ℤ``.
3. Turn the isomorphism ``sucℤ`` into a path
   ``sucℤPath : ℤ ≡ ℤ`` using ``isoToPath``
4. Define ``helix : S¹ → Type`` by mapping ``base`` to ``ℤ`` and
   a generic point ``loop i`` to ``sucℤPath i``.
5. Use ``helix`` and ``endPt`` to define the map
   ``windingNumberBase : base ≡ base → ℤ``.
   Intuitively it counts how many times a path loops around ``S¹``.
   a generic point ``loop i`` to ``sucℤPath i``.
6. Generalize this across the circle.

In this part, we focus on ``1``, ``2`` and ``3``.

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
  Then map ``negsuc zero`` to ``pos zero``.
  For ``negsuc (suc n)``, map it to ``negsuc n``.

  .. raw:: html

     </details>
     </p>

- This completes the definition of ``sucℤ``.
  Use ``C-c C-n`` to check it computes correctly.
  E.g. check that ``sucℤ (negsuc zero)`` computes to ``pos zero``
  and ``sucℤ (pos zero)`` computes to ``pos (suc zero)``.

``sucℤ`` is an Isomorphism
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

``sucℤ`` is a Path
------------------

- Imitating what we did with ``flipPath``,
  upgrade ``sucℤIso`` to ``sucℤPath``.

Part 2 - Winding Number
=======================

The ``ℤ``-bundle ``helix``
--------------------------

We want to make a ``ℤ``-bundle over ``S¹`` by
'copying ℤ across the loop via ``sucℤPath``'.
In ``Quest1.agda`` locate

.. code:: agda

   helix : S¹ → Type
   helix = {!!}

Try to imitate the definition of ``doubleCover`` to define the bundle ``helix``.
You should compare your definition to ours in ``Quest1Solutions.agda``.
Note that we have called this ``helix``, since the picture of this ``ℤ``-bundle
looks like


.. raw:: html

   <video controls width="600">

      <source src="../_static/helix.webm"
              type="video/webm">

   </video>

Counting Loops
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
you can try it on ``refl``, ``loop``, 'loop three times', 'loop negative one times' and so on.

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
