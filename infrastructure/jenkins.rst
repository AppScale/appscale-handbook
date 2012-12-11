.. AppScale Handbook - Infrastructure - Jenkins

=======
Jenkins
=======

Jenkins is a continuous integration system used to monitor and execute repeated
jobs.  Internally, AppScale uses Jenkins to build images and run automated
tests.


-------------
Configuration
-------------

Jenkins is currently installed on a small Amazon EC2 instance.  Maintenance can
be done by connecting to ubuntu@ci.appscale.com via SSH.  Most of the time
interaction with Jenkins will be done through its web UI, which is accessible
by visiting http://ci.appscale.com/.


The ``appscale`` repository
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Github repository is configured with a `Webhook`_ to notify Jenkins
whenever changes are pushed to the repository.


.. _Webhook: https://help.github.com/articles/post-receive-hooks
