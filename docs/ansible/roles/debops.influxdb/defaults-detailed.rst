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

``state``
  Optional. If value is ``present``, the database will be created; if ``absent``,
  the database will be removed. It is defaulted to ``present``.

Examples
~~~~~~~~

Create databases, remove some of the existing ones:

.. code-block:: yaml

   influxdb__databases:

     - hostname: 'ip or {{ influxdb__hostname }}'
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
  If this argument is not provided, the current grants will be left alone. If an empty list is provided, all grants for the user will be removed. It is added in 2.8.

``hostname``
  The hostname or IP address on which InfluxDB server is listening.
  Since Ansible 2.5, defaulted to ``localhost``.

``password``
  Password that will be used to authenticate against InfluxDB server.
  Alias ``login_password`` added in Ansible 2.5.

``state``
  Optional. If value is ``present``, the user will be created; if ``absent``,
  the user will be removed. It is defaulted to ``present``.

``user_name``
  Name of the user, required.

``user_password``
  Password to be set for the user.

``username``
  Username that will be used to authenticate against InfluxDB server.
  Alias ``login_username`` added in Ansible 2.5, defaulted to ``root``.

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
       hostname: '{{ influxdb__hostname }}'
       login_username: 'someuser1'
       login_password: 'somepassword1'

Create an admin user on a remote host using custom login credentials

.. code-block:: yaml

   influxdb__users:
     - user_name: 'someuser'
       user_password: 'somepassword'
       admin: yes
       hostname: '{{ influxdb__hostname }}'
       login_username: 'someuser1'
       login_password: 'somepassword1'

Create a user on localhost with privileges

.. code-block:: yaml

   influxdb__users:
     - user_name: 'someuser'
       user_password: 'somepassword'
       admin: yes
       login_username: 'someuser1'
       login_password: 'somepassword1'
       grants:
         - database: 'db'
           privilege: 'WRITE'

