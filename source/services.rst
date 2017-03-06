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

.. warning::

   SyncThing does not come with an AppArmor profile.
   Maybe I should create one.
