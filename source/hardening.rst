Hardening
=========

We really do not want arbitrary people hacking into our server.
Since we also do not want to spend time on maintaining it,
we need a very conservative security configuration.
This might complicate other things,
but the peace of mind is worth it.

Automatic Security Updates
--------------------------

Keeping a system up to date is one of the most important security aspects.
We want the system to install updates automatically.
Not only security updates.
All updates.

.. code:: sh

   sudo apt install unattended-upgrades update-notifier-common

The update-notifier package is only needed for the automatic reboots.

Edit ``/etc/apt/apt.conf.d/50unattended-upgrades`` to look like this:

.. code::

   Unattended-Upgrade::Allowed-Origins {
      "${distro_id} stable";
      "${distro_id} ${distro_codename}-security";
      "${distro_id} ${distro_codename}-updates";
   };
   Unattended-Upgrade::Remove-Unused-Dependencies "true";
   Unattended-Upgrade::Automatic-Reboot "true";
   Unattended-Upgrade::Automatic-Reboot-Time "04:00";

Additionally, ``/etc/apt/apt.conf.d/10periodic``:

.. code::

   APT::Periodic::Update-Package-Lists "1";
   APT::Periodic::Download-Upgradeable-Packages "1";
   APT::Periodic::AutocleanInterval "7";
   APT::Periodic::Unattended-Upgrade "1";

AppArmor
--------

.. code:: sh

   sudo apt install apparmor-utils

Check, if it is enabled.

.. code:: sh

   sudo apparmor_status

Firewall
--------

.. code:: sh

   apt install ufw

SSH Configuration
-----------------

First, *make sure you have public-key authentication*,
because now we disable password authentication.
To copy your public key from your desktop/laptop,
use:

.. code:: sh

   ssh-copy-id benutername@remotehost

Now we can edit ``/etc/ssh/sshd_config``:

.. code::

   PasswordAuthentication no
   AuthorizedKeysFile     %h/.ssh/authorized_keys
   Protocol 2
   PermitRootLogin no
   AllowUsers qznc

Also,
rate limiting via firewall.

.. code:: sh

   sudo ufw limit OpenSSH

Fail2ban
--------

Just install it.
Out of the box, it is configured ok.

.. code:: sh

   sudo apt install fail2ban

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

.. seealso::

   `Ubuntu Server Guide <https://help.ubuntu.com/lts/serverguide/>`_

EtcKeeper
---------

Keeping ``/etc`` in version control can be convenient.
It is presented in this hardening chapter,
because looking at history can be interesting in terms of security.

Install and initialize it.
It will autocommit daily and in sync with apt.

.. code::

   sudo apt install etckeeper
   cd /etc
   sudo etckeeper init
   sudo etckeeper commit "initial"

AIDE
----

The `Advanced Intrusion Detection Environment <http://aide.sourceforge.net>`_ tool
looks at the system and sends a mail, when anything suspicious changes.

.. code::

   sudo apt install aide
   sudo aideinit -y -f

AIDE scans the whole system,
so it takes a while.

.. warning::

   Not yet working.
   My laptop might be too slow?

.. seealso::

   `Ubuntu documentation on stricter defaults <https://help.ubuntu.com/community/StricterDefaults>`_,
   `My First 5 Minutes On A Server; Or, Essential Security for Linux Servers <https://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers>`_


