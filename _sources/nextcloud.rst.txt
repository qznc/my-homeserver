NextCloud
=========

NextCloud is a fork of OwnCloud,
which seems to be more supported by the community.

For an easy installation and maintenance,
I use snap.
Due to my dynamic DNS service,
Lets Encrypt will not provide an SSL certificate.
So fallback is self-signed.

.. code:: sh

   sudo snap install nextcloud
   sudo snap run nextcloud.enable-https self-signed

You have to wait a few minutes,
but without further ado,
it works.
Of course,
your browser will complain about the self-signed certificate being unsafe,
but you ignore that.
Follow the website's instructions for further configuration.

Out of the box,
NextCloud comes as a Dropbox clone.
Activating features like addressbook and calendar
requires more configuration as admin.

.. warning::

   Disabled NextCloud.
   Must use IP internally and DynDNS externally,
   because my router cannot fix the intern DNS.
