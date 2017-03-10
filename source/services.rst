Services
========

A few simple services my home server provides.

SyncThing
---------

A dropbox replacement.
SyncThing provides an apt repository for installation and updates.

For configuration open ``~/.config/syncthing/config.xml`` with an editor.
Change the IP address at the ``<address>127.0.0.1:8384</address>`` part,
so you can access the config remotely.

.. warning::

   At this point anybody can change your config.
   Could we set a passwort via config.xml?
   This would require knowing the hashing mechanism.

When setting the password,
I also enabled HTTPS,
so my password is transfered safely.

AppArmor Profile
~~~~~~~~~~~~~~~~

Using ``aa-genprof``, I made a AppArmor profile,
which lives in ``/etc/apparmor.d/usr.bin.syncthing``:

.. code::

    #include <tunables/global>

    /usr/bin/syncthing flags=(complain) {
      #include <abstractions/base>

      network inet dgram,
      network inet stream,
      network inet6 dgram,
      network inet6 stream,
      network netlink raw,

      /etc/hosts r,
      /etc/nsswitch.conf r,
      /etc/ssl/certs/ca-certificates.crt r,
      /home/*/.config/syncthing/ rw,
      /home/*/.config/syncthing/** rw,
      /home/*/Sync/ rw,
      /home/*/Sync/** rw,
      /proc/sys/net/core/somaxconn r,
      /run/resolvconf/resolv.conf r,
      /usr/bin/syncthing mr,
    }

.. warning::

    Still in complain mode,
    which means it will log stuff,
    but not prevent anything.
    I need to review this after a while.
