----------
APT Cacher
----------

APT Cacher is a way to cache packages locally so that multiple builds don't
have to go out to the internet to pull down packages.

To start using APT cacher, install the apt-cacher package::

  sudo apt-get install apt-cacher

Next, make sure the proxy is set to start automatically by setting ``AUTOSTART=1``
in ``/etc/default/apt-cacher``.

Next, let any host use the cache by changing the following line in
``/etc/apt-cacher/apt-cacher.conf``::

  allowed_hosts = *

Finally, on any machine that should use the cache, create the file
``/etc/apt/apt.conf.d/01proxy`` with the contents::

  Acquire::http::Proxy "http://<IP address or hostname of the apt-cacher server>:3142";

More information can be found at:

https://help.ubuntu.com/community/Apt-Cacher-Server
