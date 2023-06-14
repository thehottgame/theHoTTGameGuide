
.. _gettingGitOnMacOS:

********************
Getting Git on MacOS
********************

On certain older versions of ``MacOS`` one needs to get the right version of ``git``.

Check the version
=================

Check if you have the `right version of ``git`` <gettingGit>`_.
Macs come with ``git`` pre-installed.
You can open ``terminal`` and type

.. code-block::

   git --version

to see what version of ``git`` you have.
It is most likely outdated if you've never used ``git`` before.

Get the right version
=====================

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
