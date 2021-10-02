Quest 2 - Side Quests
=====================

.. _aTautologyCurryingCartesianClosed:

A Tautology / Currying / Cartesian Closed
-----------------------------------------

In this side quest,
you will construct the following functions.

.. code:: agda

   uncurry : (A → B → C) → (A × B → C)
   uncurry f x = {!!}

   curry : (A × B → C) → (A → B → C)
   curry f a b = {!!}

These have three interpretations :

- ``uncurry`` is a proof that
  "if ``A`` implies (``B`` implies ``C``)",
  then "(``A`` and ``B``) implies ``C``".
  A proof of the converse is ``curry``.
- `currying <https://en.wikipedia.org/wiki/Currying#:~:text=In%20mathematics%20and%20computer%20science,each%20takes%20a%20single%20argument>`_
- this is a commonly occuring example of an *adjunction*.
  See `here <https://kl-i.github.io/posts/2021-07-12/#product-and-maps>`_
  for a more detailed explanation.

Note that we have *postulated* the types ``A, B, C`` for you.

.. code:: agda

   private
     postulate
       A B C : Type

In general, you can use this to
introduce new constants to your ``agda`` file.
The ``private`` ensures ``A, B, C`` can only be used
within this ``agda`` file.

.. tip::

   ``agda`` is space-and-indentation sensitive,
   i.e. the ``private`` applies to anything beneath it
   that is indented two spaces.
