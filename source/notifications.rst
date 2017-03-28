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

.. code:: sh

   apt install postfix mailutils

During the installation of postfix,
dpkg will ask a few questions.
My answwers were:

* Install a satellite system
* For mailname my `dynamic selfhost domain <dyndns>`_
* As SMTP relay host, my mail provider ``smtp.web.de``

Now more questions via ``sudo dpkg-reconfigure postfix``.

Now, we must configure it by editing ``/etc/ssmtp/ssmtp.conf``.

* After repeating the first three questions above, there is more.
* My email address, which will receive mail for root etc.
* Destinations to accept mail.
  Removed a few of the proposed ones.
* Force synchronous updates.
* As per default,
  only accept from localhost.
* Use the upstream default of 51200000 bytes.
* Default for local address extension character.
* Use IPv4 and IPv6.

This is still not enough configuration.
Add the following lines to ``/etc/postfix/main.cf``.

.. code::

    smtp_sasl_auth_enable = yes
    smtp_sasl_security_options = noanonymous
    smtp_sasl_password_maps = hash:/etc/postfix/sasl_password
    sender_canonical_maps = hash:/etc/postfix/sender_canonical
    smtp_tls_security_level = encrypt
    smtpd_enforce_tls = yes

The account information for sending emails
is put into ``/etc/postfix/sasl_password``
in the following format.
Also, set its mode to ``0600``,
so it is not readable by everybody on the machine.

.. code::

    smtp.web.de qznc@web.de:password

In ``/etc/postfix/sender_canonical``,
write a list of all mappings for local users.

.. code::

    root someone+root@gmail.com
    qznc someone+qznc@gmail.com

These are for adapting the sender addresses.
Now for changing the recipient addresses,
edit ``/etc/aliases``.

.. code::

    postmaster:    root
    root:          someone+root@gmail.com
    qznc:          someone+qznc@gmail.com


Postfix requires some hashing and a restart now.

.. code:: sh

    sudo postmap /etc/postfix/sasl_password
    sudo postmap /etc/postfix/sender_canonical
    sudo newaliases
    sudo systemctl restart postfix

Now cron and others should be able to send email.
You can try it manually.
Explicitly try a different FROM address via ``-r``
to check if ssmtp correctly rewrites it.

.. code:: sh

   echo "$HOSTNAME Email Ready" | mail -s 'Email test' -r 'bill@microsft.com' root

.. warning::

   The ``+homeserver`` tagging in gmail seems not to work?

Now a nice trick to improve the emails a little
`from Chris Siebenmann <https://utcc.utoronto.ca/~cks/space/blog/sysadmin/IdentifyMachineEmailByRootName>`_:

.. code:: sh

    sudo chfn -f "$HOSTNAME root" root

In your passwd, you can set a "full name" for each user.
Most people leave these field empty.
However, most of those tools sending email will use it.
We can exploit that to add some more information.
For example, the hostname,
because if cron sends you an email,
that information will not be in there.
If you only have one server,
that trick in not necessary,
but maybe you find something else to put in there.

Instant Message
---------------

A more modern way is use some instant messenger.

Usually, I recommend `Signal <https://whispersystems.org/>`_,
but they do not like bots.
Likewise, WhatsApp, which is really popular.

.. warning::

    Matrix looks good. Working on using that...

Supervising
-----------

Nagios and Munin?
