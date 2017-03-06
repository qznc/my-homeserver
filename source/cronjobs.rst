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

   sudo apt install anacron

Now put a script into ``/etc/cron.hourly/example``.

.. code:: sh

   #!/bin/sh
   su - cronbot -- /home/cronbot/example.sh

To check log files,
when your script has run.

.. code:: sh

   sudo journalctl -u cron
