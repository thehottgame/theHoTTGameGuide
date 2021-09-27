.. _installation:

*********************************
Installation on MacOS and Windows
*********************************

Installation on MacOS
=====================

The following is a work in progress.

Getting Git
-----------

Macs come with ``git`` pre-installed.
You can open ``terminal`` and type

.. code-block::

   git --version

to see what version of ``git`` you have.
It is most likely outdated if you've never used ``git`` before.
To get the latest version
visit `this site <https://sourceforge.net/projects/git-osx-installer/>`_ .


.. ``brew link --overwrite git``
.. ``rm -r .emacs.d``

To tell your computer to use the correct version of ``git``,
we need to do the following :

- Open ``terminal`` and do the following to bring yourself to your home
  "directory".

  .. code::

     cd
- Do the following to show all files in this "directory".

  .. code::

     ls -la

  Amongst these we are interested in a file called ``.zprofile``
  or ``.bash_profile`` if your mac is older.
- Look at the top of your terminal window and you should see ``zsh``
  or ``bash`` if you're on an older mac.
  This is the "shell" that your mac is using for ``terminal``.
  If it is ``zsh``,

  .. code::

     open .zprofile

  This should open the file ``.zprofile`` with ``text editor``.
  Now add the following to the end of the file

  .. code::
     export PATH="/usr/local/bin:$PATH"

  If you terminal was using ``bash`` instead, do

  .. code::

     open .bash_profile

  This should open ``.bash_profile`` with ``text editor``.
  Now add the the following to the end of the file

  .. code::
     export PATH="/usr/local/bin:$PATH"

  Once you've done this, save the files and close them.
- Restart ``terminal`` and do the following again

  .. code::
     git --version

  You should see the version has now updated.

Installing ``Doom Emacs``
-------------------------

Do the following in ``terminal``.

.. code::

   # required dependencies
   brew install git ripgrep

   # optional dependencies but install them anyway
   brew install coreutils fd

   # Installs clang. This may take a long time.
   xcode-select --install

   # For fonts
   brew install fontconfig

   # Installs emacs-mac wth sexy icon
   brew tap railwaycat/emacsmacport
   brew install emacs-mac --with-modules --with-emacs-sexy-icon

   # Make an app link in Applications
   ln -s /usr/local/opt/emacs-mac/Emacs.app /Applications/Emacs.app

   # doom emacs
   git clone https://github.com/hlissner/doom-emacs ~/.emacs.d
   ~/.emacs.d/bin/doom install

   # so that you can use 'doom' anywhere
   export PATH=”$HOME/.emacs.d/bin:$PATH”

Installing ``agda``
-------------------


- Open ``terminal``.
- We will directly clone the ``agda`` repo for development version.
  First use ``cd`` ("change directory") in ``terminal``
  to navigate to where you want to place the ``agda`` library.
  Then do the following

  .. code::

    git clone https://github.com/agda/agda.git

  This gets a copy of the ``agda`` repo.
- Go into folder of agda repo then do

  .. code::

     cabal update
     make install

  This will compile ``agda`` to make it usable.
- Once process is finished,
  you can check ``agda`` is installed by doing the following in ``terminal`` :

  .. code::
     agda


Getting ``agda-mode`` in ``Doom Emacs``
---------------------------------------

- to install ``agda2-mode`` open ``doom emacs``,
  do the shortcut ``M-x``.
  (See :ref:`Emacs Commands <emacs-commands>` for how to do shortcuts in
  ``doom emacs``.)
  A window should pop up where you can type things.
  Type in :

  .. code::

     package-install

  Press enter and type in ``agda2-mode``.
- Now use ``SPC f p``.
  A selection of files should appear,
  one of which is ``init.el``.
- Open ``init.el`` and in ``lang`` section, uncomment ``agda``.
  Save the file and close ``doom emacs`` using ``SPC q q``.
- Open ``terminal``. To make the configurations of ``doom emacs`` up to date,
  do

  .. code::
     doom sync

  If there are no errors, we all good.

To test things, make a ``test.agda`` file anywhere you'd like.
- Using Doom Emacs, open ``test.agda``.
- Type in

  .. code:: agda

     open import Agda.Builtin.Nat
- Use ``C-c C-l`` to load the file.
  A ``**Agda Information**`` window should pop up
  and if all goes well, there should be nothing in it.
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

- Go `here <https://github.com/agda/cubical/releases>`_.
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

- Open ``terminal`` and do

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
  It should look something like :

  .. code::

     /Users/USERNAME/.agda/libraries

  where ``USERNAME`` is your username on your computer.
- Navigate to ``Users/USERNAME`` by doing the following in ``terminal`` :

  .. code::
     cd

- Do the following to see hidden files :

  .. code::
     ls -la

  *If there is no* ``.agda`` *folder*,
  *simply create one by doing*

  .. code::
     mkdir .agda

  If you do ``ls -la`` again, you should see ``.agda`` in the list.
- Go into that folder by doing

  .. code::
     cd .agda

- Check the contents of ``.agda`` by doing ``ls -la``.
  Create a file ``libraries`` if there isn't one already.
  Inside it, put

  .. code::

     LOCATION/cubical-0.3/cubical.agda-lib

  Save the file and close it.
- Restart ``terminal``.
  Now do ``agda -l fjdsk Dummy.agda`` in ``terminal`` again.
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

- In ``terminal``, navigate to
  where you would like to put the HoTT Game,
  as with the cubical library it can go anywhere.
  (You can use ``cd`` to navigate folders.)
- Use ``git clone https://github.com/thehottgame/TheHoTTGame.git``.
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

- In ``terminal``, use ``agda -l fjdsk Dummy.agda`` again.
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

Installation on Windows
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
