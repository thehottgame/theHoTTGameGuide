*********************
Quest 3 - Side Quests
*********************

.. _decidabilityOfIsEven:

Decidability of ``isEven``
==========================

Try to express and prove in ``agda`` the statement

.. admonition:: Problem statement

   Every natural number is even or not even.

We make a summary of what is needed:

- a definition of the type ``A ⊕ B`` (input ``\oplus``),
  which has interpretations

  - the proposition "``A`` or ``B``"
  - the construction with two ways of making recipes
    ``left : A → A ⊕ B``
    and ``right : B → A ⊕ B``.
  - the disjoint sum of two spaces
  - the coproduct of two objects ``A`` and ``B``.
    The type needs to take in parameters ``A : Type`` and ``B : Type``

    .. code:: agda

       data _⊕_ (A : Type) (B : Type) : Type where
         ???

- a definition of negation. One can motivate it by the following

  - Define ``A ↔ B : Type`` for two types ``A : Type`` and ``B : Type``.
  - Show that for any ``A : Type`` we have ``(A ↔ ⊥) ↔ (A → ⊥)``
  - Define ``¬ : Type → Type`` to be ``λ A → (A → ⊥)``.

- a formulation and proof of the statement above
