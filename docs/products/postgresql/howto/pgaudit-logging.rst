Use PGAudit for logging on Aiven for PostgreSQL®
================================================

Discover the PostgreSQL® audit logging extension and its capabilities. Learn how to enable PGAudit on your Aiven for PostgreSQL service and how to access and visualize your logs.

About PGAudit
-------------

`PGAudit <https://www.pgaudit.org/#>`_ is a PostgreSQL extension that allows comprehensive session and object audit logging. PGAudit is particularly useful when you need detailed logs for state, financial, or ISO certification audits, where *audits* are official inspections conducted by independent institutions authorized to examine specific subjects.

What distinguishes PGAudit from other logging facilities is the level of detail it supports for logs. With a regular logging tool, you can get a list of all the operations performed against the database, which is good enough for monitoring purposes. However, audits often require getting to specific statements as requested by auditors, which is what PGAudit helps you achieve.

Set up PGAudit
--------------

Prerequisites
'''''''''''''

* Aiven for PostgreSQL version 11 or higher
* ``avnadmin`` role for enabling and configuring PGAudit
* :doc:`Aiven CLI </docs/tools/cli>` / ``psql``

Enable audit logs
'''''''''''''''''

To configure PGAudit, use the ``aiven-extras`` extension and its ``set_pgaudit_parameter()`` function on a per-database level.

1. Use :doc:`Aiven CLI </docs/tools/cli>` (or :doc:`psql </docs/products/postgresql/howto/connect-psql>`) to connect to your instance.

   .. code-block:: bash

      avn service cli --project $PG_PROJECT $PG_SERVICE_NAME

2. Enable ``pgaudit`` and ``aiven-extras`` extensions.

   .. code-block:: bash

      CREATE EXTENSION pgaudit;
      CREATE EXTENSION aiven_extras CASCADE;

3. Use ``aiven_extras.set_pgaudit_parameter()`` to configure PGAudit.

   .. note:::

      By default, PGAudit does not send emit any audit records.

    To enable the logging and start getting audit records, configure relevant parameters using ``set_pgaudit_parameter`` with the parameter and the target database name.

   .. code-block:: bash

      SELECT aiven_extras.set_pgaudit_parameter('log', 'defaultdb', 'all, -misc');

.. topic:: Parameters

   For detailed information on all the parameters used for configuring PGAudit, see `Settings <https://github.com/pgaudit/pgaudit/tree/6afeae52d8e4569235bf6088e983d95ec26f13b7#readme>`_.

   There are a few parameter that you may want to check out first:

   ``log`` (default: none)
     Classes of statements to be logged
   ``log_catalog`` (default: on)	
     Whether to include logs for ``pg_catalog`` queries 
   ``log_parameter`` (default: off)
     Include CSV-formatted parameters in the log records
   ``log_relation`` (default: off)
     Create separate log messages for each relation in a (SELECT or DML) query
   ``log_statement`` (default: on)
     Include the statement text and parameters in log messages
   ``log_statement_once`` (default: off)
     Include the statement text and parameters with (only) the first log entry for a statement/   sub-statement combination
