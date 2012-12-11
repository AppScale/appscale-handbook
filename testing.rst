.. AppScale Handbook - Testing

Testing
=======

When the ``appscale`` repository is updated on Github, a notification is sent
to Jenkins via `Webhook`_.  Jenkins then processes the updates by creating an
EC2 image for testing.


Cleaning up
-----------

At the moment, there is no automated process for removing EC2 images created by
the CI server.  This should be done manually to prevent unnecessary clutter and
charges.  The command below will shows all images created by Jenkins::

    (appscale-tools)vagrant@lucid64:~$ ec2-describe-images | grep appscale_cluster | awk '{print $2,$3}' | sort -n -k 2
    ami-df79fab6 839953741869/appscale_cluster-1354931524-jenkins-c827882

The images can then be removed using the `ec2-deregister`_ command::

    (appscale-tools)vagrant@lucid64:~$ ec2-deregister ami-df79fab6
    IMAGE	ami-df79fab6


.. _Webhook: https://help.github.com/articles/post-receive-hooks
.. _ec2-deregister: http://aws.amazon.com/articles/637
