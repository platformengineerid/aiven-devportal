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

   There are a few parameters that you might want to check out first:

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

Access your logs
----------------

To access audit logs from Aiven for PostgreSQL, you need to create an integration with a service that allows monitoring and analyzing logs. For that purpose, you can seamlessly integrate Aiven for PostgreSQL with the Aiven for OpenSearch® service.

Use the console
'''''''''''''''

For instructions on how to integrate your service with Aiven for OpenSearch, see :ref:`Enable log integration <enable-log-integration>`.

Use Aiven CLI
'''''''''''''

You can also use Aiven CLI to create the service integration.

.. code-block:: bash

   avn service integration-create --project $PG_PROJECT \
     -t logs                                            \
     -s $PG_SERVICE_NAME                                \
     -d $OS_SERVICE_NAME

.. topic:: Results

   After the service integration is set up and propagated to the service configuration, the logs are available in Aiven for OpenSearch.

Visualize your logs
-------------------

Since your logs are already available in Aiven for OpenSearch, you can use :doc:`OpenSearch Dashboards </docs/products/opensearch/dashboards>` to visualize them. Check out how to access OpenSearch Dashboards in :ref:`Access OpenSearch Dashboards <access-os-dashboards>`.

For instructions on how to start using OpenSearch Dashboards, see :doc:`Getting started </docs/products/opensearch/dashboards/getting-started>`.

Note: If the Index Pattern in Kibana was configured before you enable the service integration, the audit-specific AIVEN_AUDIT_FROM field is not available for filtering. In such cases, you can refresh the field list for the index in Kibana under Stack Management → Index Patterns → <Your Index Pattern> → Refresh field list.
For audit logs of an Aiven for PostgreSQL instance, the AIVEN_AUDIT_FROM field is set to pg to allow you to easily filter those records:

This shows only the relevant results. Each log record emitted by PGAudit is stored in  Elasticsearch as a single message. Note that this cannot be guaranteed for external integrations such as Remote Syslog, as the line breaks within a given record can be interpreted as record separators. 