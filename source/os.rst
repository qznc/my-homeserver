Operating System
================

I will use Ubuntu Server LTS 16.04.
Ubuntu is widely used operating system,
which means it is relatively easy to get help online.
It also gives 5 years of security updates for LTS versions,
which is important if we want to let the server run unattended for years.

Assuming you have docker installed,
a sandbox for playing around is simple.

.. code::

   docker run -it ubuntu:16.04 bash

Docker is more restrictive than realistic for a real server, though.
So a virtual machine via QEMU is better.

QEMU Playground
---------------

First, we fetch an Ubuntu image.

.. code:: sh

   curl -O http://releases.ubuntu.com/16.04.2/ubuntu-16.04.2-server-amd64.img

This is the "installation CD".
Now we install Ubuntu to a base harddisk image of 4GiB.

.. code:: sh

   qemu-img create -f qcow2 ubuntu-base.img 4G
   qemu-system-x86_64 -hda ubuntu-base.img -cdrom ubuntu-16.04.2-server-amd64.img -boot d -enable-kvm -m 1G

Enabling KVM makes stuff faster.
The default of 128MiB RAM is not enough,
so we set RAM to 1GiB.

Go through the installation process.
Personally, I use english as a language,
but a german timezone and keyboard layout.

We do not want to modify this base image,
so we can easily reset it.
Then we can play around without remorse.
We use qemu-img to create another image *based on* the stock Ubuntu.

.. code:: sh

   qemu-img create -f qcow2 -b ubuntu-base.img playground.img

Now we can boot into the playground.
Again we use ``-enable-kvm -m 1G``.

.. code:: sh

   qemu-system-x86_64 -hda playground.img -enable-kvm -m 1G

For quick throw-away experiments,
which are not supposed to be permanent,
you can skip the img-create step via ``-snapshot``.
Here qemu will not modify the ``playground.img``.

.. code:: sh

   qemu-system-x86_64 -hda playground.img -enable-kvm -m 1G -snapshot

Networking
----------

My router is responsible for the IP addresses,
so the home server must get one by DHCP.

.. code:: sh

   apt install isc-dhcp-server

Afterwards, networking should work.
However, qemu only allows TCP and UDP by default,
so ping does not work.
Instead we try an update.

.. code:: sh

   apt update

Time
----

Our server should stay in sync automatically,
so we use NTP.
It should be installed by default.
Check via:

.. code:: sh

   timedatectl status

SSH
---

We maintain the server via ssh.
If you did not select it during installation, do it now.

.. code:: sh

   apt install openssh-server
