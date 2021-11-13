
.. content for expositing homotopy levels

Generalising h-triviality
-------------------------
..
   ``isHLevel``

.. _isPropAndIsSetVersesIsHLevel:

``isProp`` and ``isSet`` versus ``isHLevel``
--------------------------------------------

.. _hCumulativity:

h-Cumulativity
--------------

.. _higherSpheres:

Higher spheres
--------------

Here we present a way of defining spheres and disk of any dimension
generality via *suspension*.

The suspension of a space ``X : Type`` is meant to be a space
``susp X : Type`` containing
- points ``north`` and ``south``
- a continuous map a point ``x : X`` to a path ``merid x : north ≡ south``

If we take the space ``X`` to be a circle ``S¹``,
then collecting the paths from ``merid`` together we form a surface which tapers
to points at both ``north`` and ``south``.
Hence ``susp S¹`` should be the sphere ``S²``.
If we take the space ``X`` to only consist of two points
(e.g. taking ``X`` to be ``Bool``)
then there are only two paths due to ``merid`` since
there are only two points of ``X``,
and they meet at ``north`` and ``south``.
Therefore ``susp Bool`` should be the circle ``S¹``.
We can even go further and define ``Bool`` as the suspension
of the empty space.

In ``1FundamentalGroup/Quest1SideQuests/Sn.agda``
locate

.. code:: agda

   data susp (X : Type) : Type where
     north : {!!}
     south : {!!}
     merid : {!!}

Try filling in the definition for ``susp``.
Unfortunately, ``agda`` doesn't treat holes in ``HITs`` very nicely,
so you will have to remove the last hole and
just write what you think goes there.

.. raw:: html
s
   <p>
   <details>
   <summary>Solution</summary>

.. code:: agda

   data susp (X : Type) : Type where
     north : susp X
     south : susp X
     merid : X → north ≡ south

.. raw:: html

   </details>
   </p>

In the same file locate

.. code:: agda

   Sphere : ℕ → Type
   Sphere = {!!}

Try defining ``Sphere`` using ``susp``,
where ``Sphere n`` is meant to be ``Sⁿ``.
Check ``1FundamentalGroup/Quest1SideQuests/SnSolutions.agda``
to compare your solution.

We can similarly define disks
as suspensions.
A disk is just the suspension of the space ``⊤`` with only one point,
and an ``n+1``-disk is just the suspension of an ``n``-disk.
In the same file, locate and try to complete the definition of ``Disk``

.. code:: agda

   Disk : (n : ℕ) → Type
   Disk zero = {!!}
   Disk (suc n) = {!!}

Check ``1FundamentalGroup/Quest1SideQuests/SnSolutions.agda``
to compare your solution.

We can now define a map from ``Sphere n`` to ``Disk suc n``,
with the image as the boundary of ``Disk suc n``.
In the same file, locate

.. code:: agda

   SphereToDisk : {n : ℕ} → Sphere n → Disk (suc n)
   SphereToDisk {n} s = {!!}

Note that we have made the natural ``n`` `implicit <https://agda.readthedocs.io/en/v2.6.1/language/implicit-arguments.html>`_.
Try filling in the definition.

.. raw:: html

   <p>
   <details>
   <summary>Hint</summary>

- Case on ``n : ℕ``.
- When ``n`` is ``zero`` you can also case on ``s : Sphere 0``
  since the ``Sphere 0`` is just ``Bool``.
- In the successor case don't forget to use the induction hypothesis.

.. raw:: html

   </details>
   </p>

Check ``1FundamentalGroup/Quest1SideQuests/SnSolutions.agda``
to compare your solution.

Equivalent notions of h-triviality
----------------------------------


..
  from what used to be quest 1
.. _part1HomotopyLevels:

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
   <summary>refl</summary>


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
However, the "concatenation" of the path ``southHemisphere`` with ``northHemisphere``
in ``base ≡ base`` gives the surface of ``S²``,
which intuitively is not homotopic to the constant point ``base``.
So ``base ≡ base`` has non-trivial path structure.

.. image:: images/S2.png
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
to fill ``S¹``.
This is captured in

.. code:: agda

   isSet : Type → Type
   isSet A = (x y : A) → isProp (x ≡ y)

To define the fundamental group we will make the loop space satisfy
``isSet`` by *truncation* the loop space',
i.e. by forcefully adding homotopies between any two paths
with the same start and end point.
However, our work will show directly that the loop space is ``ℤ``
(connected by some path to ``ℤ``), which satisfies ``isSet``
(see quest 3).
..
  link
This implies the loop space satisfies ``isSet``, and *truncation* does nothing.
We define :ref:`truncation` in a side quest.

.. admonition:: Updated goal

   From now on we set our goal as showing that the loop space is ``ℤ``.

Apart from some exercises here and in :ref:`quest1SideQuests`, we will not revisit
the ideas of h-triviality or truncation.

.. _part2IsPropS1IsEmpty:

Part 2 - ``isProp S¹`` is empty
===============================

First we show that ``isSet S¹`` is empty.
The library contains the result

.. code:: agda

   isProp→isSet : (A : Type) → isProp A → isSet A

(see future content about *homotopy levels* .)

.. TODO insert link to content on homotopy levels

which we can then use to show ``isProp S¹`` is also empty.
Locate ``¬isSetS¹`` in ``1FundamentalGroup/Quest1.agda``.

.. code:: agda

   ¬isSetS¹ : isSet S¹ → ⊥
   ¬isSetS¹ = {!!}

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

.. code::

   ¬isPropS¹ : isProp S¹ → ⊥
   ¬isPropS¹ = {!!}

Try proving this using ``isProp→isSet``.


.. importing things

..
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
