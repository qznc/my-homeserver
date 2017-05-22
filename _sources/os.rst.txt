Operating System
================

I will use Ubuntu Server LTS 16.04.
Ubuntu is widely used operating system,
which means it is relatively easy to get help online.
It also gives 5 years of security updates for LTS versions,
which is important if we want to let the server run unattended for years.

Docker Playground
-----------------

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

Headless Server
~~~~~~~~~~~~~~~

For a more realistic feeling,
we can disable qemu's virtual display.
Instead, we ssh into the guest system.
Boot it with ``-nographic`` and some port forwarding:

.. code:: sh

   qemu-system-x86_64 -hda playground.img -enable-kvm -m 1G -nographic -net user,hostfwd=tcp::7777-:22 -net nic

Now on the host,
use ssh to port 7777.

.. code:: sh

   ssh localhost -p 7777

Converting a Desktop Ubuntu
---------------------------

With my laptop-to-homeserver conversion,
there is a full desktop system running.
It might be nice,
to access the server directly with a GUI,
but a few things are removed nonetheless.

.. code:: sh

   sudo apt remove google-chrome gnucash #...

NetworkManager provides a DNS resolver on port 53.
To disable this,
edit ``/etc/NetworkManager/NetworkManager.conf``
and comment out the ``dns=dnsmasq`` line.
Then restart NetworkManager.
Afterwards the port is free and
we could setup our own DNS server.

.. code:: sh

   sudo systemctl restart NetworkManager

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

Trimming
--------

Ubuntu is actually too generous in my opinion.
This is why I remove a few packages.

.. code:: sh

   apt remove byobu info tcpdump telnet tasksel screen laptop-detect ftp fuse install-info plymouth xauth

This also removes packages like ``ubuntu-server``,
which is ok,
because these are empty and only used to pull in other packages.

Snap
----

For installing software,
I like the Ubuntu Snap system.

.. code:: sh

   sudo apt install snapd

Ubuntu Server
-------------

The desktop Ubuntu was 32bit,
although it is a 64bit processor.
When I tried to convert the system,
I broke apt.
Then I installed Ubuntu from scratch.

Another mistake was to enabled home directory encryption.
That is not a good idea,
if you want to login with an ssh public key.
The ssh server cannot access the ``authorized_keys`` file,
if it is encrypted.
