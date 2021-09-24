.. _emacs-commands:

Emacs Commands
==========================

Notation
--------

- ``SPC`` means space bar
- ``C-`` means hold down ``Ctrl``
- ``M-`` means hold down ``Alt`` for non-Macs and ``Option`` for Macs
- ``S-`` means hold down ``Shift``
- ``RET`` means enter

Example ``C-c C-l`` in Agda files is ``Ctrl-c``, let go, ``Ctrl-l``

General Doom Emacs usage
------------------------

The 'ambient mode' is called *evil mode* and follows
vim-like bindings.
The following commands are for *evil mode*:

- ``SPC h b b`` to look for bindings (keyboard shortcuts)
- ``SPC f f`` to find files. can use ``TAB`` for auto-completing paths
- ``h j k l`` for left down up right
- ``SPC b k`` to kill 'buffers'
- ``i`` to go into *insert mode* (in insert mode you can insert text)
  and ``ESC`` or ``C-g`` to go back to *evil mode*.
- ``C-_`` to undo
- ``SPC h '`` to look up how to write a symbol.
  (Put your cursor on the symbol first.)

For beta users, to get the latest patch

- do ``SPC g g`` for "git status"
- then ``F`` for pull (whilst in "git status")

Agda usage
----------

To insert text in the ``agda`` file use ``i`` to enter *insert mode*.

- ``C-c C-l`` loads the file
- ``C-c C-,`` checks goal of the hole your cursor is in.
- ``C-c C-SPC`` fills hole your cursor is in.
- ``C-c C-r`` refines the hole your cursor is in.
- ``C-c C-c`` does cases on terms in the hole your cursor is in.
- ``C-c C-d`` used for checking types of terms
- ``C-c C-n`` used for 'reducing' terms to their 'simplest form'
- ``C-c C-.`` does ``C-c C-,`` and ``C-c C-d``
- ``M-SPC c d`` looks up the definition of the thing you are hovering over.
