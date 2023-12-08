MangAdventure Docker
^^^^^^^^^^^^^^^^^^^^

This repository contains a complete setup
of MangAdventure using docker-compose_.

Components
----------

* nginx_
* PostgreSQL_
* Redis_
* MangAdventure_

All images use Alpine Linux.

Installation
------------

First, you will need to clone this repository:

.. code:: bash

   git clone https://github.com/mangadventure/mangadocker
   cd mangadocker

----

Then, use this command to create the required directories:

.. code:: bash

   mkdir -p media static .well-known nginx/{certs,logs}

SSL certificates in ``nginx/certs`` will be available
under ``/etc/letsencrypt`` inside the container.

You can also add `relevant files`__ to ``.well-known``.
Here's an example ``security.txt`` file: ::

   Contact: mailto:admin@yoursite.com
   Canonical: https://yoursite.com/.well-known/security.txt
   Policy: https://github.com/mangadventure/MangAdventure/security/policy

__ https://www.iana.org/assignments/well-known-uris/well-known-uris.xhtml

----

The file ``status_auth.txt`` is required
in order to set up ``/__status__``. |br|
You can create it with the following command:

.. code:: bash

   printf > status_auth.txt '%s:%s\n' \
      "$(read -rp 'Username: '; printf "$REPLY")" \
      "$(openssl passwd -noverify)"

It will ask you to specify the credentials
which you can then use to view the site status.

----

Next, edit the following files as needed:

* ``env/database.env``
* ``env/mangadventure.env``
* ``nginx/sites/mangadventure.nginx``

Some settings are empty and must be set.

----

Now, you can start the containers:

.. code:: bash

   docker compose up

Once the site has been set up, static files will be copied to ``static``. |br|
You can write your own styles in ``static/styles/extra.scss`` and compile it:

.. code:: bash

   docker compose exec mangadventure mangadventure collectstatic --noinput

.. _MangAdventure: https://github.com/mangadventure/MangAdventure
.. _nginx: https://github.com/mangadventure/nginx
.. _docker-compose: https://docs.docker.com/compose/
.. _PostgreSQL: https://hub.docker.com/_/postgres
.. _Redis: https://hub.docker.com/_/redis

.. |br| raw:: html

   <br/>
