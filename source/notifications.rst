Notifications
=============

Sometimes things go wrong.
Hardware fail.
Updates fail.
Space runs out.

In such cases,
the server must be able to contact you, the admin.

E-Mail
------

The usual way is to send emails.
To avoid a full mail server, there is a simple forward tool.

.. highlight:: sh

   apt install ssmtp

Now, we must configure it by editing ``/etc/ssmtp/ssmtp.conf``.

.. code::

   root=something+homeserver@gmail.com
   mailhub=smtp.gmail.com:587
   rewriteDomain=gmail.com
   FromLineOverride=NO

   AuthUser=qznc
   AuthPass=blablabla
   UseTLS=Yes
   UseSTARTTLS=Yes


Instant Message
---------------

A more modern way is use some instant messenger.

Usually, I recommend `Signal <https://whispersystems.org/>`_,
but they do not like bots.
Likewise, WhatsApp, which is really popular.

Threema? Wire? Matrix? Jabber?

Supervising
-----------

Nagios and Munin?
