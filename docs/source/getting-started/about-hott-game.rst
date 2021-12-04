.. _theHoTTGame:

*************
The HoTT Game
*************

The Homotopy Type Theory (HoTT) Game is a project written by mathematicians
for mathematicians interested in HoTT and no experience in proof verification,
with the aim of introducing
`cubical agda <https://agda.readthedocs.io/en/v2.6.0/language/cubical.html>`_
as a tool for trying out mathematics in HoTT.
This page will help you get the Game working for you.

Agdapad
=======

The HoTT Game can be played entirely in your browser using
`Agdapad <https://agdapad.quasicoherent.io/>`_.
If you want a quick start to the game without installing anything,
you can go straight to the site, create your own ``agda`` session, and start playing.

Specifically, once you open an ``agda`` session, you should see a welcome page
(which contains useful information).
Around the top left of the screen there is a folder icon.
Click on the folder icon, and open the directory ``TheHoTTGame``.
This contains everything you need for the game.

Many thanks to Ingo Blechschmidt for incorporating the game into ``Agdapad``.

Installing Agda and the Cubical Agda library
============================================

If you are would like detailed instructions on how to install agda and a supportive text editor - 
in particular if you use MacOS or Windows - we recommend you follow instructions on
:ref:`installation`.
More general instructions follow:

Our Game is in ``agda``, which can be installed following instructions
`on their site <https://agda.readthedocs.io/en/latest/getting-started/installation.html>`_.
It is recommended that you use ``agda`` in the text editor
`emacs <https://www.gnu.org/software/emacs/tour/index.html>`_,
specifically we recommend
`doom emacs <https://github.com/hlissner/doom-emacs>`_,
but it can also work in
`atom <https://atom.io/packages/agda-mode>`_ and
`vs-code <https://github.com/banacorn/agda-mode-vscode#agda-language-server>`_.

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

You can start with :ref:`trinitarianism` if you are interested in
how logic, type theory and category theory come together
as different ways to view the same thing.
If you are more interested in homotopy theory,
try :ref:`fundamentalGroupOfTheCircle` where we show that the
fundamental group of ``S¹`` is ``ℤ``.
We recommend starting with :ref:`fundamentalGroupOfTheCircle` for intuition,
then going to :ref:`trinitarianism`.

How to start?
=============

To start with :ref:`fundamentalGroupOfTheCircle` (for example),
go to :ref:`quest0WorkingWithTheCircle`
and follow the instructions there.
Any ``agda`` should happen in ``Agdapad``
or your local copy of the repository.

Emacs issues
============

If you can't figure out ``emacs`` or forget some command, then
try consulting the :ref:`emacsCommands` page.
