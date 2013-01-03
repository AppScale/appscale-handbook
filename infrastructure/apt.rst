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


------------------------
Configuring APT packages
------------------------

Debian packages are controlled with configuration files in the ``debian/``
directory.  The following documentation walks through creating a .deb package
for HAProxy.  The resulting repository can be viewed at:

https://github.com/AppScale/appscale-haproxy

First grab the pristine HAProxy source from:

http://appscale.cs.ucsb.edu/appscale_packages/pool/haproxy-1.4.4.tar.gz

Debian has a strict naming convention for package names.  Because haproxy is a
3rd party source (i.e. a non-native Debian tool) the name of the initial tar
ball needs to conform to the convention with a dash in between the package name
and version.  Native packages, on the other hand, contain an underscore here::

  <package_name>-<major version>.<minor version>.<revision version>.tar.gz

Because we are maintaining this particular package in-house, rename the
extracted directory to::

  appscale-haproxy-1.4.4

And then re-tar it::

  tar czf appscale-haproxy-1.4.4.tar.gz appscale-haproxy-1.4.4

Snapshot the pristine contents in a git repo.  In the extracted package::

  git init
  git add -A
  git commit -m'Added pristine copy of all files from tarball'

Now, create the ``debian`` directory using ``dh_make``::

  dh_make -f ../appscale-haproxy-1.4.4.tar.gz

Answer the question with the ``s`` option since HAProxy is a single binary install.

The command above creates the ``debian`` directory and the file ``../appscale-haproxy_1.4.4.orig.tar.gz``.

Commit the pristine ``debian/`` contents to git::

  git add -A
  git commit -m'Added pristine debian directory after running command: dh_make -f ../appscale-haproxy-1.4.4.tar.gz'

Now make changes to the auto-generated files in the ``debian/`` directory:

 - customize the Section, Priority, Homepage, Description, and Depends in debian/control
 - where applicable, create the build, binary, install targets in debian/rules
 - where applicable, copy the debian/postinst.ex template and add the necessary commands to install the package
 - where applicable, create the debian/install file to define any additional files to install

The result of these customizations can be seen at:

https://github.com/AppScale/appscale-haproxy/tree/master/debian

---------------------
Building APT packages
---------------------

Before building a package, make sure you have setup GPG.  For more information see the :ref:`gpg` documentation.

Build a package using the command::

  debuild -i'.git|.swp'

Note ``-i'.git|.swp'``.  This will ignore the ``.git`` directory and any vim
swap files lying around or else a slew of errors will pour onto the screen.

When this command attempts to sign the package, you will be prompted for your
GPG key's passphrase twice.

This will create the .deb files along with some tarballs and a .changes file.
The .changes file is used with the ``dupload`` command to publish an APT
package.


----------------------
Uploading APT packages
----------------------

Finally, the packages can be uploaded to the APT repository by running::

  dupload --to appscale /path/to/package.changes


----------------------
Repository maintenance
----------------------

Packages are automatically maintained on the APT repository with the tool
`reprepro_watch`_.  Upon uploading with ``dupload``, ``reprepro_watch`` will
add it to the repository and sign the package with the AppScale Releases key.

For more information on ``reprepro_watch`` take a look at the `reprepro_watch readme`_.

.. _The GNU Privacy Handbook: http://www.gnupg.org/gph/en/manual.html
.. _reprepro_watch: https://github.com/baremetal/reprepro_watch
.. _reprepro_watch readme: https://github.com/baremetal/reprepro_watch#readme
