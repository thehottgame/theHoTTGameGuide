.. _quest-1:

*************************
Quest 1 - Higher Homotopy
*************************

.. _part-0:

Part 0 - Loop Space
===================

In this quest,
we continue to formalise the problem statement.

.. admonition:: The problem statement

   The fundamental group of ``S¹`` is ``ℤ``.

Intuitively,
the fundamental group of ``S¹`` at ``base`` is
consists of loops based as ``base`` up to homotopy of paths.
In homotopy type theory,
we have a native description of loops based at ``base`` :
it is the space ``base ≡ base``.

In general the *loop space* of a space ``A`` at a point ``a`` is defined as follows :

.. code:: agda

   Ω : (A : Type) (a : A) → Type
   Ω A a = a ≡ a

Clearly for each integer ``n : ℤ`` we have a path
that is '``loop`` around ``n`` times'.
Locate ``loop_times`` in ``1FundamentalGroup/Quest1.agda``
(note how ``agda`` treats underscores)

.. code:: agda

   loop_times : ℤ → Ω S¹ base
   loop n times = {!!}

Try casing on ``n``, you should see

.. code:: agda

   loop_times : ℤ → Ω S¹ base
   loop pos n times = {!!}
   loop negsuc n times = {!!}

It says to map out of ``ℤ`` it suffices to
map the non-negative integers (``pos``)
and the negative integers (``negsuc``).

.. code:: agda

   data ℤ : Type where
     pos    : (n : ℕ) → ℤ
     negsuc : (n : ℕ) → ℤ

This definition of ``ℤ`` uses the naturals, so try
casing on ``n`` again, you should see

.. code:: agda

   loop_times : ℤ → Ω S¹ base
   loop pos zero times = {!!}
   loop pos (suc n) times = {!!}
   loop negsuc n times = {!!}

It says to map out of ``ℕ`` it suffices to map ``zero`` and
map each succesive integer ``suc n`` inductively.
Try filling the first hole with
what we get when we loop ``0`` (``pos zero``) times.
For looping ``pos (suc n)`` times we loop ``n`` times and
loop once more.
For this we need composition of paths.

.. code:: agda

   _∙_ : x ≡ y → y ≡ z → x ≡ z

Try typing ``_∙_`` or ``? ∙ ?`` in the hole (input ``/.``)
and refining.
Checking the new holes you should see that now you need
to give two loops.
Try giving it '``loop n times``' composed with ``loop``.
Then try to also define the map on the negative integers.
You will need to invert paths using ``sym``.

.. code:: agda

   sym : x ≡ y → y ≡ x

.. raw:: html

   <p>
   <details>
   <summary></summary>

If you don't know the definition of something
you can look up the definition by sticking your cursor
on it and pressing ``M-SPC c d`` in *insert mode*
or ``SPC c d`` in *evil mode*.

You can use it to find out the definition of ``ℤ`` and ``ℕ``.

.. raw:: html

   </details>
   </p>

.. _part-1:

Part 1 - Homotopy Levels
========================

The loop space can contain higher homotopical information that
the fundamental group does not capture.
For example, consider ``S²``.

.. code:: agda

   data S² : Type where

     base : S²
     loop : base ≡ base
     northHemisphere : loop ≡ refl
     southHemisphere : refl ≡ loop

.. raw:: html

   <p>
   <details>
   <summary>``refl``</summary>


For any space ``A`` and point ``a : A``,
``refl`` is the constant path at ``a``.
Technically speaking, we should write ``refl a`` to indicate the point we are at,
however ``agda`` is often smart enough to figure that out.

.. raw:: html

   </details>
   </p>

Intuitively,
any loop in the sphere ``S²`` based at ``base`` is homotopic to
the constant path ``refl``.
In other words, the fundamental group at ``base`` of ``S²`` is trivial.
However, the "composition" of the path ``southHemisphere`` with ``northHemisphere``
in ``base ≡ base`` gives the surface of ``S²``,
which intuitively is not homotopic to the constant point ``base``.
So ``base ≡ base`` has non-trivial path structure.

.. image:: image/S2.png
   :width: 1000
   :alt: description

Here is one way of capturing homotopical data :
We can check that a space is 'homotopically trivial' (h-trivial)
from dimension ``n``
by checking if spheres of dimension ``n`` can be filled.
To be h-trivial from ``0`` is for any two points
to have a line in between; to fill ``S⁰``.
This data is captured in

.. code:: agda

   isProp : Type → Type
   isProp A = (x y : A) → x ≡ y

.. raw:: html

   <p>
   <details>
   <summary>All maps are continuous in HoTT</summary>

There is a subtlety in the definition ``isProp``.
This is *stronger* than saying that the space ``A`` is path connected.
Since ``A`` is equipped with a continuous map taking pairs ``x y : A``
to a path between them.

We will show that ``isProp S¹`` is *empty* despite ``S¹`` being path connected.

.. raw:: html

   </details>
   </p>

Similarly, to be h-trivial from dimension ``1`` is for any two points ``x y : A``
and any two paths ``p q : x ≡ y`` to have a homotopy from ``p`` to ``q``;
to fill ``S¹`` (using a slightly different definition of ``S¹``,
see `higher spheres <side-higher-spheres>`_).
This is captured in

.. code:: agda

   isSet : Type → Type
   isSet A = (x y : A) → isProp (x ≡ y)

To define the fundamental group we will make the loop space satisfy
``isSet`` by *trucating* the loop space',
i.e. by forcefully adding homotopies between any two paths
with the same start and end point.
However, our work will show directly that the loop space is ``ℤ``
(connected by some path to ``ℤ``), and later in this quest
we see that ``ℤ`` satisfies ``isSet``.
This implies the loop space satisfies ``isSet``, and truncating does nothing.

From now on we set our goal as showing that the loop space is ``ℤ``.
Apart from some exercises here and in side quests, we will not revisit
the ideas of h-triviality or truncation.

.. _part-2:

Part 2 - ``isProp S¹`` is empty
===============================

First we show that ``isSet S¹`` is empty.
The library contains the result

.. code:: agda

   isProp→isSet : (A : Type) → isProp A → isSet A

which we can then use to show ``isProp S¹`` is also empty
(we will show this in a side quest).
.. link to side quest
Locate ``¬isSetS¹`` in ``1FundamentalGroup/Quest1.agda``.

We assume ``h : isSet S¹``, which
continuously maps each pair ``x y : A``
to a point in ``isProp (x ≡ y)``.
We can apply ``h`` twice to the only point ``base`` available to us,
obtaining a point of ``isProp (base ≡ base)``.
Try mapping from this into the empty space.

.. raw:: html

   <p>
   <details>
   <summary>Hint 0</summary>

We have already shown that ``Refl ≡ loop`` is the empty space.
We have imported ``Quest0Solutions.agda`` for you,
so you can just quote the result from there.

.. raw:: html

   </details>
   </p>

.. raw:: html

   <p>
   <details>
   <summary>Hint 1</summary>

- assume ``h``
- type ``Refl≢loop`` it in the hole and refine
- it should now be asking for a proof that ``Refl ≡ loop``
- try to use ``h``

.. raw:: html

   </details>
   </p>

Now locate ``¬isProp S¹``.
Try proving this using ``isProp→isSet``.


.. _part-3:

Part 3 - ``ℤ`` satisfies ``isSet``
==================================








..
   .. raw:: html

      <p>
      <details>
      <summary>Higher spheres, generalising h-triviality and equivalent definitions</summary>

   In general, for a space ``X : Type`` to be h-trivial from dimension ``n``
   is intuitively to be able to fill the image of any ``Sⁿ``.
   We make a definition for ``Sⁿ`` in general using suspension,
   and formalize this definition of h-triviality in a side quest.
   .. enter side quest link

   There are several ways of expressing h-triviality.
   The one we have suggested is intuitive but not as clean
   nor easy to work with as the following definition

   .. code:: agda

      isHLevel : (n : ℕ) (X : Type) → Type
      isHLevel zero = isProp
      isHLevel (suc n) = λ X → (x y : X) → isHLevel n (x ≡ y)

   .. raw:: html

      </details>
      </p>
