.. _emacsCommands:

Emacs Commands
==========================

Notation
--------

- ``SPC`` means space bar
- ``C-x`` means ``Ctrl-x``
- ``M-x`` means ``Alt-x`` for non-Macs and ``Option-x`` for Macs
- ``S-x`` means ``Shift-x``
- ``RET`` means enter

Example ``C-c C-l`` in Agda files is ``Ctrl-c``, let go, ``Ctrl-l``

General Doom Emacs usage
------------------------

The 'ambient mode' is called *evil mode* and follows
vim-like bindings.
The following commands are for *evil mode*:

- ``SPC h b b`` to look for bindings (keyboard shortcuts)
- ``SPC f f`` to find files. can use ``TAB``
  for auto-completing paths and ``Backspace`` to go up a directory
- ``h j k l`` for left down up right
- ``SPC b k`` to kill 'buffers' (any little window is a buffer).
  In general ``SPC b`` gives you many options for buffers.
- ``SPC w k`` to kill unwanted windows
  (emacs can get split up into many windows)
  In general ``SPC w`` gives you many options for windows.
- ``i`` to go into *insert mode* (in insert mode you can insert text)
  and ``ESC`` or ``C-g`` to go back to *evil mode*.
- ``C-_`` to undo (be careful with this, undo can go too far;
  going into and out of insert mode is considered "one change"
  in *evil mode*, so undoing might undo a
  lot of changes made in *insert mode*).
- ``r`` to redo (be careful with this, redo can go too far).
- ``SPC h '`` to look up how to write a symbol.
  (Put your cursor on the symbol first.)


For beta users, to get the latest patch

- do ``SPC g g`` for "git status"
- then ``F`` for pull (whilst in "git status")

Agda usage
----------

.. important::

   To insert text in the ``agda`` file use ``i`` to enter *insert mode*.
   To escape *insert mode* do ``ESC`` or ``C-g``.
   All the commands below should be done whilst in *insert mode*.

- Load : ``C-c C-l`` loads the file
- Check the goal : ``C-c C-,`` checks goal of the hole your cursor is in.
- Fill the goal : ``C-c C-SPC`` fills hole your cursor is in.
- Refine the goal : ``C-c C-r`` refines the hole your cursor is in.
- Case on ``x`` : ``C-c C-c`` does cases on ``x``, where ``x`` is in the hole your cursor is in.
- Deduce : ``C-c C-d`` asks you to give it term / point ``x``,
  it deduces the type / space that ``x`` belongs to
- Normalise : ``C-c C-n`` asks you to give it term / point ``x``,
  it 'reduces' ``x`` to its 'simplest (normalised) form'
- Combo : ``C-c C-.`` does ``C-c C-,`` and ``C-c C-d``
- Looking up definitions : ``M-SPC c d`` looks up the definition of the thing you are hovering over.
- Look up unicode commands : ``M-SPC h '`` looks up how to type the unicode character you are hovering over.

Unicode commands
----------------

In general follow the guidance given above to learn unicode commands.
However here are some commonly used ones to get you started

- insert ``\to`` for ``→``
- insert ``\==`` for ``≡``
- insert ``\==n`` for ``≢``
- insert ``\bot`` for ``⊥``
- insert ``\neg`` for ``¬``
- insert ``\GS`` for ``Σ``
- insert ``\cong`` for ``≅``
- insert ``\^`` for superscript, e.g. ``S\^1`` for ``S¹``
- insert ``\bN`` for ``ℕ`` and ``\bZ`` for ``ℤ``
