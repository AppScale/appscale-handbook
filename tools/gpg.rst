.. AppScale Handbook - Tools - GPG

.. _gpg:

===
GPG
===

GPG is used to sign Debian packages.  All the standard debian tools such as
``dupload`` and ``reprepro`` as well as package management tools like
``apt-get`` expect packages to be signed.  In order to sign packages, you will
need to create a GPG key.  If you don't have one, please see below for
instructions.


------------------
Creating a GPG key
------------------

A GPG key can be created using the ``gpg --gen-key`` command.  When asked to
select what kind of key you want, select the default, ``RSA and RSA```.  The
default key size of ``2048`` is good as well.  Your first key should never
expire, so select ``0``, confirm that this is really what you want.  Proceed to
enter in your name, your email address, and a comment or description for this
key.

Depending on how much the system is used, coming up with enough randomness to
create the key may take some time.  If this happens, try to use the system:
navigate the filesystem, compile an application, download stuff, etc.


--------------------------
Building software packages
--------------------------

At this point you are ready to use your GPG key to build software packages.
Note, the AppScale Tools Vagrant VM links your Mac's GPG configuration
directory with the vagrant user on the VM, which means you do not have to do
any additional work to use the key in the appscale-tools VM.
