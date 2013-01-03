.. AppScale Handbook - Tools - Vagrant

=======
Vagrant
=======

Vagrant provides an easy way to create reproducible development environments.
It's useful to distribute environment changes to a team of users.  Currently
the AppScale Tools repository makes full use of Vagrant to provision and deploy
a working environment for developing the project as well as providing a command
console for AppScale clouds.

More information about Vagrant can be found on the `Vagrant Website`_.

-----------------
How Vagrant works
-----------------

Vagrant's magic is two-fold.  First, it allows provisioning based on
pre-configured "boxes".  AppScale currently uses the ``lucid`` base box that is
provided by the Vagrant project.  With this as the base box a project specifies
additional configuration in a ``Vagrantfile`` within the projects's root.

The ``Vagrantfile`` defines network configuration, shared folders and
additional provisioning once the VM is up and running.  When changes are made
the ``Vagrantfile``, they can be applied to an existing VM by running the
``vagrant reload`` command.

~~~~~~~~~~~~
Provisioning
~~~~~~~~~~~~

Provisioning tools reconfigure a machine so that it matches a defined state.
This state includes the contents of files, installed packages, running
services, etc.  Vagrant has built-in support for Puppet or Chef; AppScale Tools
uses Puppet.

When the command ``vagrant up`` is run, Vagrant will create the VM based on the
settings in the ``Vagrantfile`` and once the VM is running Puppet completes
VM's configuration.  At any point in time, the machine can be re-provisioned by
running the ``vagrant provision`` command.

-------------
Using Vagrant
-------------

Please refer to the `Getting Started`_ section of the AppScale Tools
documentation for more information on how to use Vagrant specifically for
AppScale Tools (but the instructions are generally applicable to any
Vagrant-based project).

.. _Getting Started: https://appscale-tools.readthedocs.org/en/latest/getting_started.html#getting-started
.. _Vagrant Website: http://vagrantup.com
