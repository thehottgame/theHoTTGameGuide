.. _about-hott-game:

*************
The HoTT Game
*************

The Homotopy Type Theory (HoTT) Game is a project written by mathematicians
for mathematicians interested in HoTT and no experience in proof verification,
with the aim of introducing
`cubical agda <https://agda.readthedocs.io/en/v2.6.0/language/cubical.html>`_
as a tool for trying out mathematics in HoTT.
This page will help you get the Game working for you.

Installing Agda and the Cubical Agda library
============================================

If you are a MacOS or Windows user we recommend you follow instructions on
:ref:`installation`.
More general instructions follow:

Our Game is in ``agda``, which can be installed following instructions
`on their site <https://agda.readthedocs.io/en/latest/getting-started/installation.html>`_.
It is recommended that you use ``agda`` in the text editor
`emacs <https://www.gnu.org/software/emacs/tour/index.html>`_,
specifically we recommend
`doom emacs <https://github.com/hlissner/doom-emacs>`_,
if you can't be bothered to do a bunch of configuration.

Once you have ``emacs`` and ``agda``, clone the
`cubical library repository <https://github.com/agda/cubical>`_ (version 0.3)
and make sure ``agda`` knows where your copy of the cubical library is
by following instructions on the
`library management page <https://agda.readthedocs.io/en/latest/tools/package-system.html?highlight=library%20management>`_.
In short: locate (or create) your ``libraries`` file and add a line

.. code::

   the-directory/cubical.agda-lib

to it, where ``the-directory`` is the location of ``cubical.agda-lib`` on your computer.

Get the HoTT Game by
`cloning our repository <https://github.com/thehottgame/TheHoTTGame>`_
into a folder and then making sure that ``agda`` knows where the HoTT Game is
by adding a line

.. code::

   the-directory/HoTTGameLib.agda

to your ``libraries`` file as above.

Try opening ``1FundamentalGroup/Quest0.agda`` in Emacs
and do ``Ctrl-c Ctrl-l``.
Some text should be highlighted, and any ``{!!}`` should turn into ``{ }``.

Where to start?
===============

You can start with ``0Trinitarianism`` if you are interested in
how logic, type theory and category theory come together
as different ways to view the same thing.
If you are more interested in homotopy theory,
try ``1FundamentalGroup`` where we show that the
fundamental group of ``S¹`` is ``ℤ``.

How to start?
=============

To start with ``1FundamentalGroup`` (for example),
go to :ref:`1-quest-0`
and follow the instructions there.
Any ``agda`` should happen in your copy of the Hott Game repository.

Emacs issues
============

If you can't figure out ``emacs`` or forget some command, then
try consulting the :ref:`emacs-commands` page.
