.. _quest1SideQuests:

Quest 1 - Side Quests
=====================

Under construction

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

.. _truncation:

Truncation
----------
