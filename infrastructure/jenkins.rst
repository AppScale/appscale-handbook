.. AppScale Handbook - Infrastructure - Jenkins

=======
Jenkins
=======

Jenkins is a continuous integration system used to monitor and execute repeated
jobs.  Internally, AppScale uses Jenkins to build images and run automated
tests.


-----
Setup
-----

Start with a vanilla Ubuntu server and install git and Puppet::

  sudo apt-get install -y git nginx puppet

In order for the system to build and test Python, install the following::

  sudo apt-get install -y python-dev python-virtualenv build-essential libpq-dev

Once the packages are installed, download and apply the Jenkins Puppet module::

  cd /etc/puppet/modules
  git clone git://github.com/baremetal/puppet-jenkins.git jenkins
  sudo puppet apply /etc/puppet/modules/jenkins/manifests/init.pp

Create an SSH key for the ``jenkins`` user by logging in as jenkins and running
the following command::

  sudo su - jenkins
  ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa -P ''


-------------
Configuration
-------------

Jenkins is currently installed on a small Amazon EC2 instance.  Maintenance can
be done by connecting to ``ubuntu@ci.appscale.com`` via SSH.  Most of the time
interaction with Jenkins will be done through its web UI, which is accessible
by visiting http://ci.appscale.com/.

~~~~~~~~~~~~~~~
Adding Security
~~~~~~~~~~~~~~~

By default Jenkins allows any user, even anonymous, to do anything on the
system.  The first thing to do is update Jenkins to only allow logged in users
to manage the system.  To do this, first click ``Jenkins -> Manage Jenkins ->
Configure Global Security``.  The check the box next to "Enable Security".
Under ``Access Control`` select "Jenkins's own user database" Under
``Authorization`` select "Anyone can do anything" for now.  Click on save.

Once that's enabled, go to ``Jenkins -> Manage Jenkins -> Manage Users``.
Create a user account and proceed to login with that username.  Then navigate
back to ``Jenkins -> Manage Jenkins -> Configure Global Security``.

Under ``Access Control`` select "Matrix-based Security".  Create an entry for
the user just created, then click on the little icon next to the red X, which
toggles all the checkboxes at once.  Click save or apply.

At this point Jenkins is only accessible to users that are logged in.

Repeat the steps above to create more users.

~~~~~~~~~~~~~~~~~~~~~~~~~~~
Adding Git & Github Support
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once that's enabled, go to ``Jenkins -> Manage Jenkins -> Manage Plugins``.

Click on the Available Tab and select the "Git Plugin" and "GitHub Plugin"
checkboxes.  At the bottom click on ``Download now and install after restart``.
On the subsequent page, click the checkbox next to the restart option.

Go to ``Jenkins -> Manage Jenkins -> Configure System``.  Under ``Git plugin``
set the ``Global Config user.name Value`` and ``Global Config user.email
Value`` fields.


~~~~~~~~~~~
Setup nginx
~~~~~~~~~~~

Configure nginx as the webserver in front of Jenkins.  The configuration below
proxies requests to ci.example.com over to the Jenkins installation, which
listens on port 8080 by default::

  upstream jenkins_server {
      server 127.0.0.1:8080 fail_timeout=0;
  }

  server {
      listen 80;
      listen [::]:80 default ipv6only=on;
      server_name ci.example.com;
  
      location / {
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header Host $http_host;
          proxy_redirect off;
  
          proxy_pass http://jenkins_server;
      }
  }


---------------
Add a build job
---------------

Jenkins is now ready to be configured to perform tasks.  Click on ``Jenkins ->
New Job``.  Give the job a name.  It's best to use names without spaces.
Select ``Build a free-style software project`` and click ok.

Under ``Source Code Management`` select Git and under the ``Repository URL``
enter the HTTP url found in the Github project page.

Under ``Build Triggers`` select ``Build when a change is pushed to GitHub``.

Finally, under build select ``Add build step -> Execute shell``.  Use the box
that appears to call an executable that is found within the project repository.
For example, create a script in the project at ``bin/run_tests``.  In the box,
simply enter ``bin/run_tests``.  The reason for this is two-fold.  First, you
are able to version control your script and secondly, you can run the same test
procedure Jenkins runs without having to use Jenkins.

Click Save at the bottom of the page.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
How jobs and build steps work
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A job is a collection of build steps that can be performed on a project.  A
build step, as the name implies, is one or more tasks that should be done to
build a project.  Jenkins expects a build step to return 0 when it's successful
and non-zero when an error occurs.  This means that if subsequent steps will
not process if a preceeding step exits with a non-zero status.


--------------------
Add a Github WebHook
--------------------

The Github repository is configured with a `Webhook`_ to notify Jenkins
whenever changes are pushed to the repository.

On Github's appscale project page, click on ``Settings -> Service Hooks ->
WebHook URLs`` and add the following URL::

  http://ci.appscale.com/github-webhook/

Now, whenever changes are pushed to Github's git repository, the project will
build.


.. _Webhook: https://help.github.com/articles/post-receive-hooks
