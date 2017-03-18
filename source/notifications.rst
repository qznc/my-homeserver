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

   apt install ssmtp mailutils

Now, we must configure it by editing ``/etc/ssmtp/ssmtp.conf``.

.. code::

   # Where to forward mail to root and other sys accounts
   root=something+homeserver@gmail.com

   # Use web.de for sending
   mailhub=smtp.web.de:587
   UseTLS=Yes
   UseSTARTTLS=Yes

   # Login for web.de
   AuthUser=qznc
   AuthPass=blablabla

   # Freemailer usually allow only specific FROM fields
   rewriteDomain=web.de
   FromLineOverride=YES

Then also ``/etc/ssmtp/revaliases``,
to set senders correctly.

.. code::

   # FROM:root -> FROM:qznc@web.de
   root:qznc@web.de

Also, your hostname must be valid for the E-Mail provider.
This might require to write it into
`/etc/mailname` and `/etc/hostname`.

Now cron and other should be able to send email.
You can try it manually.

.. code:: sh

   echo "$HOSTNAME Email Ready" | mail -s 'Email test' root

.. warning::

   The ``+homeserver`` tagging in gmail seems not to work?

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
