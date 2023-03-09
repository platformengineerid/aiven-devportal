Manage a backup to another region |beta|
========================================

The backup to another region (BTAR) feature allows you to create a service backup outside the serivce-hosting region. This article describes how to work with an Aiven service that has a backup in another region. Learn how to monitor such a service for its status, replication lag, and more. Check out how forking and restoring BTAR services is different from forking and restoring services with primary backups only. Finally, find out how to migrate a service with BTAR enabled.

Prerequisites
-------------

You have at least one Aiven service with BTAR enabled.

Monitor a service with BTAR
---------------------------

There are a few things you may want to check for your Aiven service in the context for BTAR:

* Does your service has a backup in another region created?
* What is the status of the secondary backup?
* What is the target region of the secondary backup?
* What is the replication lag between the primary region and the secondary region?

Use the console
'''''''''''''''

To check out the availability, the status, and the target region of a secondary (BTAR) backup in the `Aiven console <https://console.aiven.io/>`_, navigate to your service's homepage and access the following:

* **Overview** tab > **Advanced configuration** section > ``additional_backup_regions`` parameter
* **Backups** tab > **Database backups** > **Primary backup location** and **Secondary backup location**

Use API
'''''''

To check out the target region and the replication lag for a secondary (BTAR) backup of your service, call the `ServiceBackupToAnotherRegionReport <https://api.aiven.io/doc/#tag/Service/operation/ServiceBackupToAnotherRegionReport>`_ endpoint.

Configure the call as follows:

1. Enter YOUR-PROJECT-NAME and YOUR-SERVICE-NAME into the URL.
2. Specify DESIRED-TIME-PERIOD depending on the time period you need the metrics for: select one of the following values for the ``period`` key: ``hour``, ``day``, ``week``, ``month``, or ``year``.

.. code-block:: bash

    curl --request POST                                                                                                      \
        --url https://api.aiven.io/v1/project/YOUR-PROJECT-NAME/service/YOUR-SERVICE-NAME/backup_to_another_region/report    \
        --header 'Authorization: Bearer YOUR-BEARER-TOKEN'                                                                   \
        --header 'content-type: application/json'                                                                            \
        --data '{"period":"DESIRED-TIME-PERIOD"}'

.. topic:: Output

    As an output, you get metrics including replication lags at specific points in time.

.. _fork-and-restore:

Fork and restore a service with BTAR
------------------------------------

You can use the `Aiven web console <https://console.aiven.io/>`_ to recover your service from a backup in another region. To restore your service using BTAR, :doc:`create a fork </docs/platform/howto/console-fork-service>` of the original service in the region where the secondary backup resides.

.. note::

   When you use the secondary-backup region for **Fork & restore**, the secondary backup is used. Otherwise, the service is recovered from the primary backup.

1. Open the `Aiven console <https://console.aiven.io/>`_ and navigate to your service homepage.
2. Open the **Backups** view and select **Fork & restore**.
3. In the **New Database Fork** window, apply the following settings:

   1. Specify a name for your new service and select a project to host the service.
   2. Select a scope of data to be forked: **Latest transaction** or **Point in time**.

      .. note::

         For the point-in-time recovery (PITR) option, set up the time to no later than the time of taking the latest backup.

   3. Select the cloud provider that your existing service uses.
   4. Select the cloud region corresponding to the target region of your secondary backup.
   5. Select a service plan and click on **Create fork**.

.. topic:: Result

    A new service has been created based on the secondary backup of your primary service.

Migrate a service with BTAR
---------------------------

You can migrate a service with BTAR the same way you :doc:`migrate a service with a regualar backup </docs/platform/howto/migrate-services-cloud-region>`.

.. topic:: Backup location vs service migration

   When you migrate your service, locations of service backups, both primary and secondary ones, do not change.
