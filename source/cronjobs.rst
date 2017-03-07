Cron Jobs
=========

I do have a few scripts,
which cron should execute regularly.
For safety, let us wall them off.

First, a separate user called cronbot.

.. code:: sh

   sudo adduser --disabled-password cronbot

This guide is not about my scripts,
so I will just use a dummy script here.
As user cronbot create ``~/example.sh``.

.. code:: sh
  cat >~/example.sh <<EOF
  #!/bin/sh
  exec 2>&1
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
but it has a wrong idea about stderr output meaning failure.

  Cronic is a small shim shell script for wrapping cron jobs
  so that cron only sends email when an error has occurred.
  Cronic defines an error as any non-trace error output or a non-zero result code.

While stderr sounds like it is for errors,
it is actually for meta output in general.
Fortunately, we can redirect stderr to stdout,
then cronic can only rely on the result code,
which is correct.
This is the reason for the ``exec 2>&1`` line in the ``example.sh`` script.

Example
-------

Put the following script into ``/etc/cron.hourly/example``.

.. code:: sh

   #!/bin/sh
   su - cronbot -- cronic /home/cronbot/example.sh

To check log files,
when your script has run,
use journalctl.

.. code:: sh

   sudo journalctl -u cron

Longterm
--------

I do not like cron and I want to replace it.
For now it works.
On issue is efficiency.
For the example above, there is

* at the top ``cron``, which
* executes ``anacron``, which
* executes ``/etc/cron.hourly/example``, which
* executes ``cronic``, which
* executes ``/home/cronbot/example.sh``.

In this stack, anacron and cronic are crutches for cron shortcomings.

One alternative could be systemd,
which includes `timers <https://wiki.archlinux.org/index.php/Systemd/Timers>`_.
They can be misused as a cron replacement,
but it is obviously a workaround,
especially if you want to get mail on failure.

Maybe `jobber <https://dshearer.github.io/jobber/>`_ could be worthwhile.
While I dislike the YAML syntax,
it seems to be fine in its general design.
