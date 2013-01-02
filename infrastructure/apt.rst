.. AppScale Handbook - Infrastructure - APT

===
APT
===

An APT repository to serve AppScale debian packages.  Currently the
appscale-tools Vagrant configuration uses it to install packages necessary to
run AppScale Tools.

The APT repository can be accessed at http://apt.appscale.com by adding the
following to ``/etc/apt/sources.list.d/appscale.list``::

  deb http://apt.appscale.com lucid main


Once that is in the system's configuration, run ``apt-get update`` followed by
``apt-get install <package name>``.


~~~~~~~~~~~~~~~~~
Environment Setup
~~~~~~~~~~~~~~~~~

The appscale-tools Vagrant VM is fully configured for building APT packages.
This includes the configuration for the ``dupload`` command to work, hooking
the host's GPG configuration directory and setting up the ``DEBEMAIL`` and
``DEBFULLNAME`` variables via ``~/.appscale-tools/appscale_config.sh``.

Please make sure you have a working GPG installation with a GPG key for
yourself.  More information on GPG can be found in the `The GNU Privacy
Handbook`_.

Make sure your environment contains the following variables::

  DEBEMAIL="your.email.address@example.org"
  DEBFULLNAME="Firstname Lastname"


---------------------
Building APT packages
---------------------

Build a package using the command::

  debuild -us -uc -i'.git|.swp'

----------------------
Uploading APT packages
----------------------

Finally, the packages can be uploaded to the AppScale repository by running::

  dupload --to appscale appscale_1.0_amd64.changes 


.. _The GNU Privacy Handbook: http://www.gnupg.org/gph/en/manual.html
