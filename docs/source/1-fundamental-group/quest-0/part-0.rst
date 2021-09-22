.. _part-0:

**********
The Definition of the Circle
**********

In this series of quests we will prove that the fundamental group
of ``S¹`` is ``ℤ``.
In fact, our strategy will also show that the higher homotopy groups of
``S¹`` are all trivial.
We begin by formalising the problem statement.

A contruction of 'the circle' is :

- a point called ``base``
- an edge from that point to itself called ``loop``

Here is our definition of the circle in ``agda``.

.. code-block:: agda

   data S¹ : Type where
     base : S¹
     loop : base ≡ base

The ``base ≡ base`` is the * space of paths from ``base`` to ``base`` *.
The definition asserts that there is a point called ``loop``
in ``base ≡ base``, i.e. a path from ``base`` to itself.
Whenever we have a colon like ``S¹ : Type`` or ``base : S¹``
it says the former is a point in the latter,
where the latter is viewed as a space;
in the first case ``Type`` is the space of spaces.

.. NOTE::
   :hidden:

   This is called a _higher inductive type_ (HIT), which generally
   follows the format of

   * ``data``
   * the name of the HIT - in our case ``S¹``
   * the *type* of the HIT, in our case ``Type``
   * ``where`` followed by
   * the *constructors* of the HIT, in our case ``base`` and ``loop``,
     which we will think of as vertices, edges, surfaces, and so on.

An "edge" is the same as a path.
There are other paths in ``S¹``,
for example the *constant path at ``base``*.
In ``1FundamentalGroup/Quest0.agda`` navigate to

.. code-block:: agda

   Refl : base ≡ base
   Refl = {!!}

we will guide you through defining it.
We are about to construct a path ``Refl : base ≡ base``
(read path ``Refl`` from ``base`` to ``base``).
The *hole* ``{!!}`` is where you describe the path.
We will fill the hole ``{!!}``.

* enter ``C-c C-l`` (this means ``Ctrl-c Ctrl-l``).
  Whenever you do this, ``agda`` will check the document is written correctly.
  This will open the ``*Agda Information*`` window looking like

.. code-block::

   ?0 : base ≡ base
   ?1 : (something)
   ?2 : (something)
   ...


  This says you have some unfilled *holes*.
* navigate to between holes using ``C-c C-f`` (forward) or ``C-c C-b`` (backward)
* enter ``C-c C-r``. The ``r`` stands for _refine_.
  Whenever you do this whilst having your cursor in a hole,
  Agda will try to help you.
* You should now see ``λ i → {!!}``.
  Load the file again (using ``C-c C-l``) and
  the ``*Agda Information*`` window should now look like :

  .. code-block::

     ?0 : S¹
     ...
     ?6 : (something)

     ———— Errors ————————————————————————————————————————————————
     Failed to solve the following constraints:
       ?0 (i = i1) = base : S¹ (blocked on _3)
       ?0 (i = i0) = base : S¹ (blocked on _3)

  This is ``agda`` suggesting that for each
  ``i : I`` (if you like you can think of this as a generic point
  on the the unit interval ``I``)
  you give a point in between the start and end of the path,
  such that at "``i = 1``" and "``i = 0``" it is ``base``.
  This is all you need to specify a path in ``agda``.

* navigate to that new hole
* enter ``C-c C-,`` (this means ``Ctrl-c Ctrl-comma``).
  Whenever you make this command whilst having your cursor in a hole,
  ``agda`` will check the *goal*, i.e. what kind of thing you need to stick in.
  The goal (``*Agda information*`` window) should now be more focused :

.. code-block::

   Goal: S¹
   —————————————————————————
   i : I
   ———— Constraints ——————————————
   ?0 (i = i1) = base : S¹ (blocked on _3, belongs to problem 4)
   ?0 (i = i0) = base : S¹ (blocked on _3, belongs to problem 4)
   _4 := λ i → ?0 (i = i) (blocked on problem 4)

* since this is the constant path, write ``base`` in the hole.
* press ``C-c C-SPC`` to fill the hole with ``base``.
  In general when you have some text (and your cursor) in a hole,
  doing ``C-c C-SPC`` will tell ``agda`` to replace the hole with that text.
  ``agda`` will give you an error if it can't make sense of your text.
* the number of holes in the ``*Agda Information*``
  window should have gone down by one,
  this means ``agda`` has accepted what you filled this hole with.
  Just to be sure you can also reload the ``agda`` file and check
  that ``agda`` has no complaints.
* if you want to play around with this you can start again
  by replacing what you wrote with ``?`` and doing
  ``C-c C-l``.
