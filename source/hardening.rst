Hardening
=========

We really do not want arbitrary people hacking into our server.
Since we also do not want to spend time on maintaining it,
we need a very conservative security configuration.
This might complicate other things,
but the peace of mind is worth it.

Automatic Security Updates
--------------------------

.. code:: sh

   sudo apt install unattended-upgrades

AppArmor
--------

Check, if it is enabled.

.. code:: sh

   sudo apparmor_status

Firewall
--------

SSH Configuration
-----------------

No password auth.

.. code::

   Protocol 2
   PermitRootLogin no
   DebianBanner no

Restrict access to certain users.

Fail2ban

Checking for Root Kits
----------------------

.. code:: sh

   sudo apt install rkhunter chkrootkit

Disk Encryption
---------------

Secure Shared Memory
--------------------

The following line in '/etc/fstab':

.. code::

   tmpfs /run/shm tmpfs defaults,noexec,nosuid 0 0

Prevent IP Spoofing
-------------------

I need to edit '/etc/host.conf', but why?

.. code::

   order bind,hosts
   nospoof on
