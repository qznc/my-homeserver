Debugging
=========

This is a random collection of things,
which help maintaining the server.
It contains stuff,
you should check from time to time.

First, install a bunch of helpful utilities.

.. code:: sh

   sudo apt install iotop htop sysstat net-tools

First minutes
-------------

If you have not looked at your server for a long time,
then you should have a suspicious look around first.
If you trust your server,
still take a look.
If it has not been a long time,
still take a look.

.. code:: sh

   # look if anybody else is logged in
   w
   # check the last logins
   last
   # check the kernel logs
   dmesg
   # check the RAM use
   free -m
   # check uptime and load
   uptime
   # check active processes
   htop
   # check disk IO
   iotop
   # check network connections
   netstat -pant
   # check disk use
   df -h
