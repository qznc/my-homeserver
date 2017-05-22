Introduction
============

Scope
-----

* It will be a Linux server,
  because I don't know Windows.
* Nearly no maintenance.
  The server should be capable of running for years with me doing something.
  It is tempting to constantly tweak it,
  but that should happen on my schedule.
  If stuff breaks often,
  I am forced to fix it,
  because I will start to rely on my home server being available.
* Admin access via ssh.
  I do not care about some web interface or GUI for my server.
* It will be very cheap in version 1,
  so no RAID NAS setup or high end media server.
* I see no need for automation via Puppet, Salt, cfengine, whatever.

.. seealso::

   `Reddit r/HomeServer <https://www.reddit.com/r/HomeServer/>`_

Audience
--------

While the goal is to be generic,
there are a few assumptions about you, the reader.

* You can use a command line interface.
  This tutorial covers compilation from raw compiler invocations.
  Using an IDE would hide this build process.
  Since I am using Linux,
  examples will assume a bash shell,
  which is the default on all major distributions (including BSD and OS X).
