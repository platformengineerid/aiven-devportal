Aiven for PostgreSQL audit logging
==================================

.. important::

   Aiven for PostgreSQL® audit logging is a :doc:`limited availability feature </docs/platform/concepts/beta_services>`. If you're interested in trying out this feature, contact the sales team at `sales@Aiven.io <mailto:sales@Aiven.io>`_.

Discover the Aiven for PostgreSQL® audit logging feature and its capabilities. Learn why use it and how it works.

.. seealso::

   For information on how to enable the audit logging on your Aiven for PostgreSQL service and how to access and visualize your logs, check out :doc:`Collect audit logs in Aiven for PostgreSQL® </docs/products/postgresql/howto/pgaudit-logging>`.

About the audit logging
-----------------------

Why use the audit logging
-------------------------

Use cases
---------

Typically, capabilities of the audit logging feature are most appreciated by organizations or enterprises that value data security, integrity, and compliance. The audit logging has a wide range of applications in various industries:

Finance and Banking
  Financial institutions often use audit logging to ensure compliance with regulatory requirements, track financial transactions, and detect fraudulent activities.
Healthcare
  In the healthcare industry, audit logging is crucial for maintaining the confidentiality and integrity of patient records as well as complying with privacy regulations like HIPAA (Health Insurance Portability and Accountability Act).
Government and Public Sector
  Government agencies implement audit logging to track changes in critical systems, secure sensitive data, and meet legal and regulatory requirements.
Information Technology (IT) and Software Companies
  IT and software companies utilize audit logging to monitor access to their systems, track software changes, and identify potential security breaches.
Retail and E-commerce
  Companies in this industry use audit logging to track customer data, transactions, and inventory management to ensure data integrity and prevent unauthorized access.
Manufacturing
  Manufacturers often use audit logging to track changes to production processes, monitor equipment performance, and maintain data integrity for quality control.
Education
  Educational institutions use audit logging to protect sensitive student data, track changes to academic records, and monitor system access for security purposes.

How it works
------------

To use the audit logging for your service (database) for collecting logs in Aiven for PostgreSQL, you need to :doc:`enable and configure this feature </docs/products/postgresql/howto/pgaudit-logging>` in `Aiven Console <https://console.aiven.io>`_ or using `Aiven API <https://api.aiven.io/doc/>`_ or :doc:`Aiven CLI </docs/tools/cli>`.

When enabled on your service, the audit logging can be configured so that it addresses your specific needs. There are a few `audit logging parameters <https://github.com/pgaudit/pgaudit/tree/6afeae52d8e4569235bf6088e983d95ec26f13b7#readme>`_ that you might want to configure for that purpose:

``pgaudit.targetDatabases``
  Names of databases where the audit logging is to be enabled
``pgaudit.log`` (default: none)
  Classes of statements to be logged by the session audit logging
``pgaudit.log_catalog`` (default: on)	
  Whether the session audit logging should be enabled for a statement with all relations in pg_catalog
``pgaudit.log_client``
  Whether log messages should be visible to a client process, such as ``psql``
``pgaudit.log_level``
  Log level that should be used for log entries
``pgaudit.log_parameter`` (default: off)
  Whether audit logs should include the parameters passed with the statement
``pgaudit.log_parameter_max_size`` 
  Maximum size (in bytes) of a parameter's value that can be logged
``pgaudit.log_relation`` (default: off)
  Whether a separate log entry for each relation (for example, TABLE or VIEW) referenced in a SELECT or DML statement should be created
``pgaudit.log_rows``
  Whether the audit logging should include the rows retrieved or affected by a statement (with the rows field located after the parameter field)
``pgaudit.log_statement`` (default: on)
  Whether the audit logging should include the statement text and parameters
``pgaudit.log_statement_once`` (default: off)
  Whether the audit logging should include the statement text and parameters in the first log entry for a statement/ sub-statement combination (as opposed to including them in all the entries)
``pgaudit.role``
  Master role to use for an object audit logging

.. topic:: Audit logging parameters

    For information on all the parameters available for configuring the audit logging, see `Settings <https://github.com/pgaudit/pgaudit/tree/6afeae52d8e4569235bf6088e983d95ec26f13b7#readme>`_.

To disable the audit logging on your service (database), you can use `Aiven Console <https://console.aiven.io>`_, `Aiven API <https://api.aiven.io/doc/>`_, or :doc:`Aiven CLI </docs/tools/cli>` for :ref:`modifying your service's advanced configuration <disable-pgaudit>`. 

Limitations
-----------

To be able to enable, configure, and use the audit logging, you need the following:

* Aiven for PostgreSQL Pro Plan
* PostgreSQL version 11 or higher
* ``avnadmin`` superuser role

What's next
-----------

:doc:`Set up the audit logging on your Aiven for PostgreSQL service </docs/products/postgresql/howto/pgaudit-logging>` and start collecting audit logs.
