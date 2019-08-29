Default variable details
========================

Some of ``debops.influxdb`` default variables have more extensive configuration
than simple strings or lists, here you can find documentation and examples for
them.

.. contents::
   :local:
   :depth: 1

.. _influxdb_database:

influxdb__databases
-------------------

List of databases that should be present or absent on a given InfluxDB server.
Each database is defined as a YAML dict with the following keys:

``database_name``
  Name of the database, required. Should be composed from alphanumeric
  characters and underscore (``_``) only. Max length: 16 for MySQL databases,
  80 for MariaDB databases.

``hostname``
  The hostname or IP address on which InfluxDB server is listening. Since version 2.5, defaulted to ``localhost``.

``password``
  Password that will be used to authenticate against InfluxDB server. Alias ``login_password`` added in version 2.5, defaulted to ``localhost``.

``port``
  The port on which InfluxDB server is listening, defaulted to ``8086``.

``proxies``
  HTTP(S) proxy to use for Requests to connect to InfluxDB server added in version 2.5.

``retries``
  Number of retries client will try before aborting. ``0`` indicates try until success and it is defaulted to ``3``.

``ssl``
  Uses https instead of http to connect to InfluxDB server., added in 2.5.

``state``
  Optional. If value is ``present``, the database will be created; if ``absent``,
  the database will be removed. It is defaulted to ``present``.

``timeout``
  Number of seconds Requests will wait for client to establish a connection, added in ``2.5``.

``udp_port``
  UDP port to connect to InfluxDB server, defaulted to ``4444``.

``use_udp``
  Use UDP to connect to InfluxDB server.

``username``
  Username that will be used to authenticate against InfluxDB server.
  Alias ``login_username`` added in version 2.5, defaulted to ``root``.

``validate_certs``
  If set to ``no``, the SSL certificates will not be validated.
  This should only set to ``no`` used on personally controlled sites using self-signed certificates. It is defaulted to ``yes``.

Examples
~~~~~~~~

Create databases, remove some of the existing ones:

.. code-block:: yaml

   influxdb__databases:

     - hostname: 'ip or hostname'
       database_name: 'dbname'


.. _influxdb_user:

influxdb__users
---------------

List of user accounts that should be present or absent on a given InfluxdDB
server. Each user account is defined as a dict with a set of keys and values.

User account parameters
~~~~~~~~~~~~~~~~~~~~~~~

``admin``
  Whether the user should be in the admin role or not. Since version 2.8, the role will also be updated. It is defaulted to ``no``.

``grants``
  Privileges to grant to this user. Takes a list of dicts containing the ``database`` and ``privilege`` keys.
  If this argument is not provided, the current grants will be left alone. If an empty list is provided, all grants for the user will be removed.

``hostname``
  The hostname or IP address on which InfluxDB server is listening.
  Since Ansible 2.5, defaulted to ``localhost``.

``password``
  Password that will be used to authenticate against InfluxDB server.
  Alias ``login_password`` added in Ansible 2.5.

``port``
  The port on which InfluxDB server is listening, defaulted to ``8086``.

``proxies``
  HTTP(S) proxy to use for Requests to connect to InfluxDB server.

``retries``
  Number of retries client will try before aborting. ``0`` indicates try until success.

``ssl``
  Use https instead of http to connect to InfluxDB server.

``state``
  Optional. If value is ``present``, the user will be created; if ``absent``,
  the user will be removed. It is defaulted to ``present``.

``timeout``
  Number of seconds Requests will wait for client to establish a connection.

``udp_port``
  UDP port to connect to InfluxDB server.

``use_udp``
  Use UDP to connect to InfluxDB server.

``user_name``
  Name of the user, required.

``user_password``
  Password to be set for the user.

``username``
  Username that will be used to authenticate against InfluxDB server.
  Alias ``login_username`` added in Ansible 2.5, defaulted to ``root``.

``validate_certs``
  If set to ``no``, the SSL certificates will not be validated.
  This should only set to ``no`` used on personally controlled sites using self-signed certificates. It is defaulted to ``yes``.

Examples
~~~~~~~~

Create a user on localhost using default login credentials

.. code-block:: yaml

   influxdb__users:
     - user_name: 'someuser'
       user_password: 'somepassword'

Create a user on localhost using custom login credentials

.. code-block:: yaml

   influxdb__users:
     - user_name: 'someuser'
       user_password: 'somepassword'
       hostname: '192.168.3.203'
       login_username: 'ceva'
       login_password: 'altceva'

Create an admin user on a remote host using custom login credentials

.. code-block:: yaml

   influxdb__users:
     - user_name: 'someuser'
       user_password: 'somepassword'
       admin: yes
       hostname: '192.168.3.203'
       login_username: 'ceva'
       login_password: 'altceva'

Create a user on localhost with privileges

.. code-block:: yaml

   influxdb__users:
     - user_name: 'someuser'
       user_password: 'somepassword'
       admin: yes
       login_username: 'ceva'
       login_password: 'altceva'
       grants:
         - database: 'db'
           privilege: 'WRITE'
         - database: 'db2'
           privilege: 'READ'
