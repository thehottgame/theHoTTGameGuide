.. _installation:

************
Installation
************

Overview
========

To get things up and running you will need five things

1. ``agda`` installed on your computer. This is what will check if your code makes sense.
   (Automatic if using the ``Nix`` installation)
2. A text editor, for example ``doom emacs``, ``atom``, ``vs-code``, or ``vim``.
   This is an environment for you to edit files in.
3. support for ``agda2-mode`` or ``agda-mode`` in your text editor.
   This should do syntax highlighting for your code (pretty colours)
   and make sure the text editor has the right shortcuts.
4. A clone of the cubical library. (Automatic if using the ``Nix`` installation.)
5. A clone of the HoTT Game, which is our code.

There are many ways to install ``agda``.
On this page we will try to describe some ways to install it.
There are roughly three ways:

- Using ``Nix``. If you use windows this is probably the easiest and most
  rewarding method.
  It involves getting the ``NixOS``, a linux operating system inside
  your computer, so you also get to try out linux.
- Installing ``agda`` and the cubical library yourself
- Using VS-code and getting the ``agdaLanguageServer``

.. _textEditors:

Text editors
------------

.. admonition:: Important

   *No matter the text editor you choose*, you will need ``emacs`` installed somewhere on your computer,
   as ``agda-mode`` relies on ``emacs`` in the background.

Whilst we will assume you use ``doom emacs`` in our guides (since it is the hardest to get used to),
there are other options :

- ``atom`` :

  `Here <https://sites.google.com/site/wakelinswan/teaching/installing-agda>`_
  is a set of instructions by Andrew Swan for getting ``agda`` working in ``atom``.

- ``vs-code`` :
  `Here <https://github.com/banacorn/agda-mode-vscode#agda-language-server>`_
  is a set of instructions for getting ``agda`` working in ``vs-code`` (scroll down to ``installation``).
  You might be able to skip steps ``1``, ``2`` and ``3`` by enabling
  ``agdaMode.connection.agdaLanguageServer`` in the settings.
  (We haven't tried this - feedback is welcome.)

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

Now you need to set up ``agda-mode``
(is this necessary if you can get ``agda-mode`` in ``emacs``? - feedback welcome) :

.. code::

   sudo apt install agda-mode

followed by

.. code::

   agda-mode setup

You can check the version of ``agda`` by doing ``agda --version`` in the terminal.

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
  you can check ``agda`` is installed and its version by doing the following in ``terminal`` :

  .. code::

     agda --version

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

In either case we should have ``agda`` *and* ``agda-mode``.
So we should just need to get a text editor.

.. _installingDoomEmacs:
Installing ``doom emacs``
=========================

Here we give instructions for installing ``doom emacs`` on each operating system.
If you have specific advice / issues specific to your operating system then please let us know in
`issues <https://github.com/thehottgame/TheHoTTGame/issues>`_.

Linux
-----

We have experience difficulties with getting ``doom`` on ``ubuntu`` specifically,
so you *might* be better off using :ref:`one of the other options <textEditors>`,
in particular ``atom`` appears to work well.
Try installing ``doom emacs`` according to
the instructions on `their github repository <https://github.com/hlissner/doom-emacs#install>`_.
A quick guide follows:

1. Go to a terminal and type in

.. code:: bash

   git clone --depth 1 https://github.com/hlissner/doom-emacs ~/.emacs.d

   ~/.emacs.d/bin/doom install

You'll probably want to answer "yes" to the options unless you know better.
We recommend you add ``~/.emacs.d/bin`` to your ``PATH``
so you can call doom directly and from anywhere;
accomplish this by going to the file ``~/.bashrc`` located in your home directory
(or ``~/.zshrc`` file if you use zsh as your shell)
and adding the line ``export PATH=$PATH:~/.emacs.d/bin`` at the end.

This should give you ``doom emacs``.
You might need to restart your computer and or ``emacs`` to make sure everything works correctly.

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

.. admonition:: NixOS and WSL2

   If you came from the NixOS and WSL2 instructions then go to the
   :ref:`linux section<installingDoomEmacs>`.

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

Operating system specific issues
--------------------------------

If you have specific advice or issues specific to your operating system then please let us know in
`issues <https://github.com/thehottgame/TheHoTTGame/issues>`_.

.. _gettingAgda2ModeOrAgdaModeSupportForYourTextEditor:

Getting ``agda2-mode`` or ``agda-mode`` support for your text editor
====================================================================

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
  (If you came from the ``Nix`` installation guide replace ``agda`` with ``(agda +local)``
  instead.)
- Open ``terminal``. To make the configurations of ``doom emacs`` up to date, do

  .. code::

     doom sync

  If there are no errors, you should have ``agda2-mode`` in ``doom emacs``.

Getting ``agda-mode`` on ``atom``
---------------------------------

1. In ``atom`` select
  - Edit > Preferences (GNU/Linux)
  - Atom > Preferences (macOS)
  - File > Settings (Windows)
2. Select Install from the side menu.
3. Type agda into the search box.
4. Install the packages ``agda-mode`` and ``language-agda``

Test it
=======
Once you have installed ``agda``, a text editor,
and support for ``agda-mode`` in your text editor,
you should test it.

Make a ``test.agda`` file anywhere you'd like.

- Open ``test.agda`` in ``doom emacs``.
- Type in

  .. code:: agda

     open import Agda.Builtin.Nat

- Use ``C-c C-l`` to load the file.
  An ``**Agda Information**`` window should pop up
  and if all goes well, there should be nothing in it.
- Use ``C-c C-d`` then enter ``Nat``.
  The output in the agda info window should be ``Set``.

Congratulations, you now have ``agda`` and
can use ``emacs`` bindings for ``agda``.
However, you have nothing more than the
builtin types.
So we need to get the library.


Getting the cubical library
===========================

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

  This is the file that tells ``agda`` "this is a library" when
  ``agda`` looks into this folder.
  You can place the folder (now) called ``cubical-0.3`` anywhere you like.
  For the sake of this guide,
  let's say you put it in a place so that
  the path is ``LOCATION/cubical-0.3``.

Now we need to tell ``agda`` this ``cubical-0.3`` library exists,
so that it will look for it when an ``agda`` file uses code from it.

- Open a terminal and do

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

  Examples in common operating systems :

  - On ``linux`` this might look something like :

    .. code::

       /home/USERNAME/.agda/libraries

    where ``USERNAME`` is your username on your computer.

  - On ``MacOS`` this might look something like :

    .. code::

       /Users/USERNAME/.agda/libraries

    where ``USERNAME`` is your username on your computer.
  - On ``windows`` this might look something like :

    .. code::

       C:\Users\USERNAME\AppData\Roaming\agda\libraries

    where ``USERNAME`` is your username on your computer.

- Navigate to ``home/USERNAME`` or ``Users/USERNAME`` or ``C:\Users\USERNAME\AppData\Roaming\agda``
  using ``cd``.

- Do the following to see hidden files :

  .. code::

     ls -la

- *If there is no* ``.agda`` (``agda`` for windows) *folder*, *simply create one* by doing

  .. code::

     mkdir .agda

     (or mkdir agda for windows)

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
- Restart the terminal.
  Now do ``agda -l fjdsk Dummy.agda`` in the terminal again.
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

Getting The HoTT Game
=====================

The HoTT Game is also an ``agda`` library
so we need to repeat the above process for it.

- In a terminal, navigate to
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
  If all went correctly, the text should be highlighted and you should be ready to go.
  Congratulations, you can now play the HoTT Game.



Installing with Nix
===================

.. _installingOnMacOSWithNix:

Linux and MacOS
---------------

``Nixpkgs`` maintains a set of ``agda`` libraries that can be added to a
derivation managed by the nix package manager,
see `here <https://github.com/NixOS/nixpkgs/blob/master/doc/languages-frameworks/agda.section.md>`_
for details.
The file ``shell.nix`` in our repository contains a derivation that will add ``emacs``, ``agda``, the ``agda standard library``,
and ``cubical agda`` to your local nix store and subsequently to a local shell environment by adding these locations to your ``PATH``.

However, because user configurations for ``emacs`` are mutable,
it will not (easily) manage your (emacs configuration) dot-files,
so we will use the underlying ``emacs`` provided by ``nixpkgs`` but install ``doom emacs`` normally in your local user's environment.

1. Install ``doom emacs`` (or whichever text editor you prefer)
   via the method described for your operating system
   :ref:`here<installingDoomEmacs>`.
   (If you are on Windows with NixOS on WSL2 then you are a linux
   user for the rest of the installation and should do everything in a termial inside NixOS.)

2. Get ``agda2-mode`` support to ``doom`` (or whichever editor you prefer)
   via the method described :ref:`above<gettingAgda2ModeOrAgdaModeSupportForYourTextEditor>`.

3. Clone our repository into a folder by going to some directory using ``cd`` and doing

   .. code::

      git clone https://github.com/thehottgame/TheHoTTGame.git

   This can be done anywhere you like.

4. Install ``Nix`` (*not* ``NixOS``) using following the guidance
   `on the official site <https://nixos.org/download.html#nix-install-linux>`_.
   We install the single-user version for linux
   (compare this with what is written on the official website):

   .. code::

      sh <(curl -L https://nixos.org/nix/install) --no-daemon

   If you are on MacOS this will be different, and if you are on Windows using NixOS
   then this should also be exactly what you need.

5. Open a terminal, and use `cd` to navigate to the folder ``TheHoTTGame``, which was cloned before.
   In ``TheHoTTGame``, do

   .. code:: bash

      nix-shell

   It might be that you need to restart your computer for this to work,
   and you might need to wait a little bit for it to start working,
   it might stay blank for a while.
   Later booting of nix-shell should be faster than the first.

   This should open up a ``Nix`` shell (inside your usual terminal),
   from which you can do all the usual things in a terminal and more.
   The above mentioned packages should automatically be loaded on your ``PATH``.
   The above is all defined by the package set in
   ``shell.nix`` in the folder ``TheHoTTGame``.

6. Each time you wish to use ``agda`` (in particular its libraries),
   you should do step 5 to load the requisite packages onto the ``PATH`` so that they can be found.

7. If you got ``doom``, go back to ``.doom.d/init.el``
   and make sure that instead of uncommenting ``;; agda`` in the ``;; lang``,
   *replace* it with ``(agda +local)`` to tell doom to use the ``agda-mode``
   version specified by the local environment.
   Once the file is saved, sync ``doom`` from within the ``nix-shell`` that was loaded above:

   .. code:: bash

      doom sync

8. You can now load the agda source code in this by starting doom from the nix-shell:

   .. code:: bash

      doom run .

   Open the file ``0Trinitarianism/Quest0.agda`` and tell ``agda-mode`` to load and check it by doing
   ``SPC m l`` (``space``, ``m`` and ``l``, in that order.)
   If everything is configured correctly, you should get nice colors and any ``{!!}``
   will become interactive holes to fill.

Windows
-------
First have a read of the previous section for Linux and MacOS for an overview,
since once you get NixOS with WSL2, you will be using a Linux operating system anyway.

1. Get WSL2 following instructions `here <https://docs.microsoft.com/en-us/windows/wsl/install>`_.
   You might also like to follow a `video guide <https://docs.microsoft.com/en-us/windows/wsl/install>`_.
   Reboot your system.

2. By default WSL2 will get ubuntu, which is fine, but is not the operating system we will use.
   We want to get ``NixOS``, which we can do by following instructions
   in the quick start section of `this github page <https://github.com/Trundle/NixOS-WSL>`_.
   Reboot your system.

3. Reopen ``NixOS`` and follow the
   :ref:`rest of the installation instructions <installingOnMacOSWithNix>` as if you
   are a linux user.
