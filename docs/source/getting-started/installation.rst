.. _installation:

************
Installation
************

Overview
========

To get things up and running you will need three things

1. ``agda`` installed on your computer. This is what will check if your code makes sense.
2. A text editor, for example ``doom emacs``, ``atom``, ``vs-code``, or ``vim``.
   This is an environment for you to edit files in.
3. ``agda2-mode`` or ``agda-mode``. This should do syntax highlighting for your code (pretty colours)
   and make sure the text editor has the right shortcuts.
4. A clone of the cubical library, where all the existing ``agda`` code is.
5. A clone of the HoTT Game, which is our code.

There are many ways to install agda and a couple of text editors in which it could be made working.
Here we will try to describe some ways to install it.

.. admonition:: Important

   Depending on the text editor you choose to use, the instructions will be different.
   However, *no matter the text editor you chooce*, you will need ``emacs`` installed somewhere on your computer,
   as ``agda-mode`` relies on ``emacs`` in the background.

.. _textEditors:

Text editors
------------

Whilst we will assume you use ``doom emacs`` (since it is the hardest to get used to),
you do not have to use ``emacs``, and many people can't get used to the way it works.
Here are three alternatives

- ``atom`` :

  `Here <https://sites.google.com/site/wakelinswan/teaching/installing-agda>`_
  is a set of instructions by Andrew Swan for getting ``agda`` working in ``atom``.

- ``vs-code`` :
  `Here <https://github.com/banacorn/agda-mode-vscode#agda-language-server>`_
  is a set of instructions for getting ``agda`` working in ``vs-code`` (scroll down to ``installation``).
  You might be able to skip steps ``1``, ``2`` and ``3`` by enabling
  ``agdaMode.connection.agdaLanguageServer`` in the settings.
  (We haven't tried this - feedback is welcome).

- ``vim`` :
  `Here <https://github.com/derekelkins/agda-vim>`_
  is a set of instructions for getting ``agda`` working in ``vim``.
  (We haven't tried this - feedback is welcome).

.. _installingAgda:

Installing ``agda``
===================

Here we give instructions for installing ``agda`` on each operating system.
If you have specific advice / issues specific to your operating system then please let us know in
`issues <https://github.com/thehottgame/TheHoTTGame/issues>`_.
Another source for information is
`official installation guide <https://agda.readthedocs.io/en/v2.5.4.2/getting-started/installation.html#prebuilt-packages-and-system-specific-instructions>`_,
but our advice might be more relevant to you.

Debian and Ubuntu
-----------------

Ubuntu already should have a version of ``emacs`` installed.
If not, go to a terminal and type in

.. code::

   sudo apt-get install emacs

To get ``agda``, go to a terminal and type in

.. code::

   sudo apt install agda-bin

This is all you need to get ``agda``.
Now you need to get a text editor and ``agda-mode``.

MacOS
-----

This will give you both ``agda`` and ``agda-mode`` at once.

- Open a terminal.
- We will directly clone the ``agda`` repo for development version.
  First use ``cd`` ("change directory") in the terminal
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

This is all you need to get ``agda`` and ``agda-mode``, now you just need a text editor.

Windows
-------

We used powershell as the terminal, but others probably work too.

.. warning::

   Always use powershell as admin.

For the prerequisites

- install chocolatey: follow instructions on
  `their page <https://chocolatey.org/install>`_
- In (admin) powershell do (via chocolatey, cabal)
  - ``choco install ghc``
  - ``choco install cabal``
  - ``cabal update``
  In order to make ``cabal`` see ``ghc``,
  close and reopen the terminal before doing the next steps.
  You might want to also try ``refreshenv`` for this.
  - ``cabal install happy``
  - ``cabal install alex``

Now to install ``agda``, first try using ``cabal`` by doing ``cabal install make``
in the terminal. If this works then go with "using cabal", if not
then try "using stack"

.. raw:: html

  <p>
  <details>
  <summary>Using ``cabal``</summary>

- You should have installed ``make`` with ``cabal install make`` by this point, if not do so now.
- Directly clone the repo for development version.
  *You can choose where to put this* by navigating to some specific folder in the terminal and doing

  .. code::

    git clone https://github.com/agda/agda.git

- It should create a folder called ``agda`` (a copy of the github repo). You should do ``cd agda``
  to go into that folder, then once you're in there do

  .. code::

     make install

  which installs ``agda`` using ``make`` (it says "run the file called ``MAKEFILE`` from the folder").

- Once installation is finished, try typing ``agda --version`` in powershell to check the version.

.. raw:: html

  </details>
  </p>

.. raw:: html

  <p>
  <details>
  <summary>Using ``stack``</summary>

- Get stack using the installer `here <https://docs.haskellstack.org/en/stable/install_and_upgrade/#windows>`_.
- Run ``stack upgrade`` in the terminal
- Doing ``cabal get Agda`` in the terminal will create a folder called ``Agda-2.6.2`` *where you are at in the terminal*.
  *You can choose where to put this* by navigating to some specific folder in the terminal using ``cd FILENAME``.
- Once you have created this ``Adgda-2.6.2``, go into it by doing ``cd Agda-2.6.2``.
- In the folder ``Agda-2.6.2``, there should be a file called ``stack-9.0.1.yaml``.
  Now you can try doing ``stack --stack-yaml stack-9.0.1.yaml install`` in the terminal (when you're in the folder ``Agda-2.6.2``)
  to run that file.
- Once installation is finished, try typing ``agda --version`` to check the version.

.. raw:: html

  </details>
  </p>

Installing ``doom emacs``
=========================

Here we give instructions for installing ``doom emacs`` on each operating system.
If you have specific advice / issues specific to your operating system then please let us know in
`issues <https://github.com/thehottgame/TheHoTTGame/issues>`_.

Ubuntu
------

Try installing ``doom emacs`` according to
the instructions on `their github repository <https://github.com/hlissner/doom-emacs#install>`_.
However, we have experience difficulties with getting ``doom`` on ``ubuntu`` specifically,
so you *might* be better off using :ref:`one of the other options <textEditors>`,
in particular ``atom`` appears to work well.

MacOS
-----

Make sure you have the `right version of git <gettingGitOnMacOS>`_.

Do the following in a terminal to get ``doom emacs``.

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

This should give you ``doom emacs``.
You might need to restart your computer and or ``emacs`` to make sure everything works correctly.

Windows
-------

There are detailed instructions for getting ``doom emacs`` on windows
`here <https://earvingad.github.io/posts/doom_emacs_windows/>`_.

The advice given there for installing fonts *might not work*.
If it doesn't work, try installing a font (for example
`Iosevka <https://typeof.net/Iosevka/>`_)
by following
`these instructions <https://support.microsoft.com/en-us/office/add-a-font-b7c5f17c-4426-4b53-967f-455339c564c1>`_.
Then go to ``.doom.d/config.el``
and add the line (anywhere)

.. code:: elisp

    (setq doom-font (font-spec :family "Iosevka SS04" :size 18 :weight 'medium))

Here the font name is ``Iosevka SS04``. You can also change the font size and weight.

Other operating systems
-----------------------

Please refer to the instructions on `their github repository <https://github.com/hlissner/doom-emacs#install>`_.
If you have specific advice / issues specific to your operating system then please let us know in
`issues <https://github.com/thehottgame/TheHoTTGame/issues>`_.

Getting ``agda2-mode`` or ``agda-mode``
=======================================

If you have decided to use ``doom emacs`` then you can get ``agda2-mode`` inside ``doom emacs`` (details below).
For other text editors, you must first install ``agda-mode``,
and then find the relevant ad-on to the text editor to support ``agda-mode`` (details below).

Getting ``agda2-mode`` on ``doom emacs``
----------------------------------------

Here we install ``agda2-mode`` in ``Doom Emacs``.
Note that this is *not* ``agda`` itself, but syntax highlighting and shortcuts for ``agda``.

- Do the shortcut ``M-x`` in ``doom emacs``.
  (See :ref:`Emacs Commands <emacs-commands>` for how to do shortcuts in
  ``doom emacs``.)
  A window should pop up where you can type things.
  Type in :

  .. code::

     package-install

  Press enter and type in ``agda2-mode``.
- Now do the shortcut ``SPC f p``.
  A selection of files should appear.
  Type in ``init.el`` and hit enter (``RET``).
- Now you are in ``init.el``. Look for the ``lang`` section and uncomment ``agda``.
  Save the file and close ``doom emacs`` using ``SPC q q``.
- Open ``terminal``. To make the configurations of ``doom emacs`` up to date, do

  .. code::

     doom sync

  If there are no errors, you should have ``agda2-mode`` in ``doom emacs``.

Getting ``agda-mode`` on ``atom``
---------------------------------

The general instruction (you can do either step first) is

-  Get ``agda-mode``
-  Get the right packages on ``atom``

The second half is easy and doesn't depend on your operating system :

1. In ``atom`` select Preferences from the Edit menu.
2. Select Install from the side menu.
3. Type agda into the search box.
4. Install the packages ``agda-mode`` and ``language-agda``

For getting ``agda-mode`` we give instructions per operating system :

Debian and Ubuntu
-----------------

You need to have ``agda`` already from :ref:`installingAgda`.
Go to a terminal and type

.. code::

   sudo apt install agda-mode

followed by

.. code::

   agda-mode setup

This should get ``agda-mode``.

MacOS
-----

We already got ``agda-mode`` when we got ``agda``.

Windows
-------




.. ........................................................................




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

Get doom emacs following instructions
`here. <https://earvingad.github.io/posts/doom_emacs_windows/>`_


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

.. HEREHEREHERE

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
