Cron Jobs
=========

I do have a few scripts,
which cron should execute regularly.
For safety, let us wall them off.

First, a separate user called "cronbot".

.. code:: sh

   sudo adduser --disabled-password cronbot

This guide is not about my scripts,
so I will just use a dummy script here.
As user "cronbot" create ``~/example.sh``.

.. code:: sh
  cat >~/example.sh <<EOF
  #!/bin/sh
  echo "Success!"
  EOF
  chmod +x ~/example.sh
  ~/example.sh

I use anacron, because it is simpler to dump scripts into ``/etc/cron.hourly``.

.. code:: sh

   sudo apt install anacron cronic

Only Mail on Failure
--------------------

Since I enabled email notifications,
cron spams a lot.
We only want mail,
if something goes wrong.
Cron violates the `rule of silence <https://en.wikipedia.org/wiki/Unix_philosophy>`_.

The solution:
Cron sends mail, if there was output from stdout or stderr,
so we must silence the script.
The `cronic <http://habilis.net/cronic/>`_ wrapper is available via apt,
but it has a wrong idea about "fail".

  Cronic is a small shim shell script for wrapping cron jobs
  so that cron only sends email when an error has occurred.
  Cronic defines an error as any non-trace error output or a non-zero result code.

While "stderr" sounds like it is for errors,
it is actually for meta output in general.
Fortunately, we can easily redirect stderr to stdout,
then cronic can only rely on the result code,
which is correct.

Example
-------

Put the following script into ``/etc/cron.hourly/example``.

.. code:: sh

   #!/bin/sh
   su - cronbot -- cronic /home/cronbot/example.sh 2>&1

To check log files,
when your script has run.

.. code:: sh

   sudo journalctl -u cron
