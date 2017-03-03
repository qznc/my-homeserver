
My Home Server v0.1
===================

I plan to build and maintain a cheap home server.
Here I document my design choices,
configuration, and maybe even teach a few Linux things.

For version 0.x,
I will only use a VM.
When I actually buy hardware and use it,
that will be version 1.0.
If I buy new hardware,
it will be version 2.0.
And so forth.

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


Contents
--------

.. toctree::
   :maxdepth: 2

   os
   hardening
   notifications
   webserver
   sandstorm
   fineprint

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

