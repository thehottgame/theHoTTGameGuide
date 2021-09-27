.. _installation:

*********************************
Installation on MacOS and Windows
*********************************

Installation on MacOS
=====================

``https://sourceforge.net/projects/git-osx-installer/``
``brew link --overwrite git``
``rm -r .emacs.d``

``export PATH="/usr/local/bin:$PATH"``
in ``.bash_profile`` (if old)

.. code::

   # required dependencies
   brew install git ripgrep

   # makes computer use latest git
   export PATH=/usr/local/bin:$PATH

   # optional dependencies
   brew install coreutils fd

   # Installs clang
   xcode-select --install

   # For fonts
   brew install fontconfig

   # Installs emacs-mac wth sexy icon and no title bars
   brew tap railwaycat/emacsmacport
   brew install emacs-mac --with-modules --with-emacs-sexy-icon --with-no-title-bars

   # Make an app link in Applications
   ln -s /usr/local/opt/emacs-mac/Emacs.app /Applications/Emacs.app

   # doom emacs
   git clone https://github.com/hlissner/doom-emacs ~/.emacs.d
   ~/.emacs.d/bin/doom install

   # so that you can use 'doom' anywhere
   export PATH=”$HOME/.emacs.d/bin:$PATH”

How to Install the HoTT Game on Windows
=======================================

Prerequisites
-------------

.. warning::

   ALWAYS USE POWERSHELL AS ADMIN

- install chocolatey: follow instructions on
  `their page <https://chocolatey.org/install>`_
- In (admin) powershell do (via chocolatey, cabal)
  - ``choco install ghc``
  - ``choco install cabal``
  - ``cabal install happy``
  - ``cabal install alex``

..
   <!-- ## The Damned Paths -->

   <!-- Something something need to add new system environment variables, -->
   <!-- need to ask Samuel again. -->

Doom Emacs
----------

Get doom emacs following instructions made
`here <earvingad.github.io/posts/doom_emacs_windows/>`_


..
   <!-- IN POWERSHELL LOCAL TO USER -->

   <!-- - Prerequisites -->
   <!--   ``` -->
   <!--   choco install git emacs ripgrep -->
   <!--   choco install fd llvm -->
   <!--   ```  -->
   <!-- - Doom Emacs itself -->
   <!--   ``` -->
   <!--   git clone https://github.com/hlissner/doom-emacs ~/.emacs.d -->
   <!--   ~/.emacs.d/bin/doom install -->
   <!--   ``` -->
   <!--   **Icons will be missing for windows sadly** -->

Development Version of Agda
---------------------------

IN POWERSHELL
.. local?

- Directly clone the repo for development version.
  *You can choose where to put this*.
  .. code::

    git clone https://github.com/agda/agda.git

- We need to install ``make`` for windows. Easiest via cabal.
  .. code::

     cabal install make

- Go into folder of agda repo then do
  .. code::

     cabal update
     make install

- Once installation is finished, try typing ``agda`` in powershell to check version.

Getting ``agda-mode`` in ``doom emacs``:

- to install ``agda2-mode`` open ``doom emacs``,
  do the shortcut ``M-x`` (``alt-x``) and type in
  .. code::

     package-install

  Press enter and type in ``agda2-mode``.

- In ``.doom.d/init.el``, uncomment ``agda`` in ``lang``.
- ``doom sync`` to update. Then ``SPC-q-R`` to restart.

To test things, make a ``test.agda`` file anywhere you'd like.
- Using Doom Emacs, open ``test.agda``.
- Type in

  .. code:: agda

     open import Agda.Builtin.Nat

- Use ``C-c C-d`` then enter ``Nat``.
  The output in the agda info window should be ``Set``.

Congratulations, you now have Agda and
can use emacs bindings for Agda.
However, you have nothing more than the
builtin types.

The Cubical Library
-------------------

The HoTT Game currently requires the ``cubical-0.3`` library.
We walk through an *example* of an installation of the ``cubical-0.3`` library.
See the
`Agda documentation <https://agda.readthedocs.io/en/latest/tools/package-system.html>`_
for more about libraries.

- Go
  `here <https://github.com/agda/cubical/releases>`_.
  Under 'version 0.3',
  download the 'Source Code' file in either formats ``zip`` or ``tar.gz``.
- Open the 'Source Code' file.
  It should turn into a folder which contains a folder called
  'cubical'.
  Choose a place for it to permanently stay,
  this can be anywhere you like.
- Rename the folder 'cubical' to 'cubical-0.3'.
  Inside it, there should be a ``cubical.agda-lib`` file
  with contents

  .. code::

     name: cubical-0.3
     include: .
     depend:
     flags: --cubical --no-import-sorts

  This is the file that tells Agda "this is a library" when
  Agda looks into this folder.
  You can place the folder (now) called ``cubical-0.3`` anywhere you like.
  For the sake of this guide,
  let's say you put it in a place so that
  the path is ``LOCATION/cubical-0.3``.

Now we need to tell ``agda`` this ``cubical-0.3`` library exists,
so that it will look for it when an ``agda`` file uses code from it.

- Open Powershell and do

  .. code::

     agda -l fjdsk Dummy.agda

- Assuming you don't already have an ``agda`` library called ``fjdsk``,
  you should see an error message of the form

  .. code::

     Library 'fjdsk' not found.
     Add the path to its .agda-lib file to
       'BLAHBLAHBLAH/libraries'
     to install.
     Installed libraries:
       none

  The ``BLAHBLAHBLAH/libraries`` is where we tell ``agda`` of
  the location of libraries.
  For Windows, it should look like
  .. code::

     C:\Users\USERNAME\AppData\Roaming\agda\libraries

  where ``USERNAME`` is your username on your computer.
- Navigate to the folder
  ``C:\Users\USERNAME\AppData\Roaming\agda``.
  *If there is no* ``agda`` *folder in*
  ``C:\Users\USERNAME\AppData\Roaming``,
  *simply create one*.
- In ``C:\Users\USERNAME\AppData\Roaming\agda``,
  create a file ``libraries`` if there isn't one already.
  Inside it, put
  .. code::

     LOCATION/cubical-0.3/cubical.agda-lib

- Now do ``agda -l fjdsk Dummy.agda`` in powershell locally again.
  This time the error message should be
  .. code::

     Library 'fjdsk' not found.
     Add the path to its .agda-lib file to
        'BLAHBLAHBLAH/libraries'
     to install.
     Installed libraries:
        cubical-0.3
           (LOCATION/cubical-0.3/cubical.agda-lib)

  Congratulations, ``agda`` is now aware of
  the existence of the ``cubical-0.3`` library.

The HoTT Game
-------------

The HoTT Game is also an Agda library
so we need to repeat the above process for it.

- In Powershell, navigate to
  where you would like to put the HoTT Game,
  as with the cubical library it can go anywhere.
  (You can use ``cd`` to navigate folders.)
- Use ``git clone https://github.com/Jlh18/TheHoTTGame.git``.
  This should copy the HoTT Game repository as
  a folder called ``TheHoTTGame``.
  For the purposes of this guide,
  let's say you have put the HoTT Game in your computer
  at the path
  .. code::

     LOCATION1/TheHoTTGame

  Inside it, you should see many files,
  one of which should be ``TheHoTTGame.agda-lib``.
- Go back to ``BLAHBLAHBLAH/libraries``
  and add the following line
  .. code::

     LOCATION1/TheHoTTGame/TheHoTTGame.agda-lib

- In Powershell, use ``agda -l fjdsk Dummy.agda`` again.
  The error message should now look something like
  .. code::


     Library 'fjdsk' not found.
     Add the path to its .agda-lib file to
       'BLAHBLAHBLAH/libraries'
     to install.
     Installed libraries:
       cubical-0.3
         (LOCATION/cubical-0.3/cubical-0.3.agda-lib)
       TheHoTTGame
         (LOCATION1/TheHoTTGame/TheHoTTGame.agda-lib)

- In Doom Emacs,
  open ``TheHoTTGame/1FundamentalGroup/Quest0.agda`` and do ``C-c C-l``
  (``Control-c Control-l``).
  Congratulations, you can now play the HoTT Game.
