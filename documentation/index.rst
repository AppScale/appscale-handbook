.. AppScale Handbook - Documentation

=============
Documentation
=============

Documentation has been written using the `Sphinx`_ project.  Documentation can
be started by installing the ``python-sphinx`` package and running the command
``sphinx-quickstart``.  This command will guide you through the setup process.

One of the many advantages of documenting using Sphinx is its ability to
generate documentation in multiple formats.  The majority of the time, HTML is
going to be the primary format, but it's good to be able to generate a PDF if
necessary, and Sphinx has that ability built in.


----------------------
Building documentation
----------------------

Building HTML is simple, just run the command ``make html`` in the
documentation directory.  This will create the directory ``_build/html`` which
is a static site that can be deployed with any webserver.

.. _Sphinx: http://sphinx-doc.org/
