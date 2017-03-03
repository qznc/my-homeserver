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
   DebianBanner no

Also,
rate limiting via firewall.

.. code:: sh

   sudo ufw limit OpenSSH

Fail2ban
--------

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
