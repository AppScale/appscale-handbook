.. AppScale Handbook - Code Style - Python

======
Python
======

--------------------
Virtual Environments
--------------------

A useful utility when developing and deploying python projects is
`Virtualenv`_.  It provides a way of creating containers for Python libraries
and executables on a per-project / installation basis.  When the installation
is no longer necessary the container can be removed without polluting the
system-level Python installation.

~~~~~~~~~~~~~~~~~
Virtualenv Primer
~~~~~~~~~~~~~~~~~

First install virtualenv::

  sudo apt-get install python-virtualenv

Once installed, a virtualenv can be created by running the command::

  virtualenv venv

This will create the ``venv`` directory within the directory it was run.  So,
if you're in ``${HOME}`` you should see ``${HOME}/venv``.  At this point, the
virtual environment needs to be "activated", or tied to your shell environment.
This can be done by running the command::

  source ${HOME}/venv/bin/activate

At this point Python will execute from that environment using any packages that
are installed in it.  Installing packages is simply a matter of runing ``pip
install <package name>`` or running ``python setup.py install`` within a
downloaded project.


----------
Code Style
----------

~~~~~~~~~~
Docstrings
~~~~~~~~~~

Docstrings are Python's way to add documentation to your code.  The docstring format AppScale uses for functions is covered in the `Comments`_ section of `Google's Python style`_.

.. _Google's Python style: http://google-styleguide.googlecode.com/svn/trunk/pyguide.html
.. _Comments: http://google-styleguide.googlecode.com/svn/trunk/pyguide.html?showone=Comments#Comments
.. _Virtualenv: http://www.virtualenv.org/en/latest/
