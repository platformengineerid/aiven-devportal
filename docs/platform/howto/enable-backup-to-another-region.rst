Add a backup to another region |beta|
=====================================

On top of a regular service backup stored in the region where your service is hosted, you can create an additional backup located in a different region. Learn how to enable the backup to another region (BTAR) feature in Aiven.

About enabling BTAR
-------------------

Tools
'''''

To add an additional service backup for your service, you can use the following tools:

* `Aiven console <https://console.aiven.io/>`_
* CLI
* API

Limitations
'''''''''''

Currently, BTAR is supported for Aiven for MySQL and Aiven for PostgreSQL only.

Prerequisites
-------------

* Aiven account
* Depending on the method you choose to use for enabling CCR

  * Access to the `Aiven console <https://console.aiven.io/>`_
  * `cURL` CLI tool
  * `Aiven CLI tool <https://github.com/aiven/aiven-client>`_

Back up to another region via console
-------------------------------------

1. Log in to the `Aiven console <https://console.aiven.io/>`_.
2. From the **Current services** view, select an Aiven service on which you'd like to enable BTAR.
3. In the **Overview** tab of your service's page, navigate to **Advanced configuration** and select **Change**.
4. In the **Edit advanced configuration** window, select **Add configuration option** > **additional_backup_regions**.
5. Enter a region name into the **additional_backup_regions** field and select **Save advanced configuration**.

.. image:: /images/platform/howto/add-advanced-parameter.png
   :width: 100%
   :alt: Add advanced parameter

.. topic:: Result
   
   Your new additional backup is visible in
   
   * **Overview** tab > **Advanced configuration** section > **additional_backup_regions**
   * **Backups** tab > **Secondary backup location**.

Back up to another region with CLI
----------------------------------

.. note::
    
   In this instruction, the :doc:`Aiven CLI client </docs/tools/cli>` is used to interact with Aiven APIs.

Using CLI, you can enable BTAR for

* :ref:`New Aiven service <new-service-cli>` or
* :ref:`Existing Aiven service <existing-service-cli>`.

.. topic:: additional_backup_regions

   To enable BTAR on an Aiven service, you need to add the `additional_backup_regions` parameter to relevant commands.

.. _new-service-cli:

Create a new service with BTAR via CLI
''''''''''''''''''''''''''''''''''''''

Use the :ref:`avn service create <avn-cli-service-create>` command to create a new service. Include ``additional_backup_regions`` as a parameter to the command and set its value to the name of desired cloud region.

.. code-block:: bash

    avn service create                                    \
        --service-type service_type_name                  \
        --cloud cloud_region_name                         \
        --plan service_plan_name                          \
        -c additional_backup_regions=name_of_cloud_region \
        new_service_name

.. _existing-service-cli:

Enable BTAR on an existing service via CLI
''''''''''''''''''''''''''''''''''''''''''

Use the :ref:`avn service update <avn-cli-service-update>` command to configure your service so that it supports BTAR. Include ``additional_backup_regions`` as a parameter to the command and set its value to the name of desired cloud region.

    .. code-block:: bash

    avn service update name_of_existing_service                    \
        -c additional_backup_regions=name_of_cloud_region

Back up to another region with API
----------------------------------

.. note::
    
   In this instruction, the `curl` command line tool is used to interact with Aiven APIs.

Using :doc:`Aiven APIs </docs/tools/api>`, you can enable BTAR for

* :ref:`New Aiven service <new-service-api>` or
* :ref:`Existing Aiven service <existing-service-api>`.

.. topic:: additional_backup_regions

   To enable BTAR on an Aiven service, you need to include the `additional_backup_regions` parameter in relevant calls.

.. _new-service-api:

Create a new service with BTAR via API
''''''''''''''''''''''''''''''''''''''

Use the `ServiceCreate <https://api.aiven.io/doc/#tag/Service/operation/ServiceCreate>`_ API to create a new service with BTAR enabled. When constructing the API request, add the ``user_config`` object to the request body and nest the ``additional_backup_regions`` field inside.

.. code-block:: bash

    curl --request POST                                                   \
        --url https://api.aiven.io/v1/project/YOUR_PROJECT_NAME/service    \
        --header 'Authorization: Bearer YOUR_BEARER_TOKEN'                 \
        --header 'content-type: application/json'                          \
        --data
            '{
            "cloud": "string",
            "plan": "string",
            "service_name": "service_2_name",
            "service_type": "cassandra",
            "user_config": {
                "additional_backup_regions": ["cloud-region-name"]
            }
        }'

.. _existing-service-api:

Add a CCR peer to an existing service
'''''''''''''''''''''''''''''''''''''

Use the `ServiceUpdate <https://api.aiven.io/doc/#tag/Service/operation/ServiceUpdate>`_ API to configure your existing service so that it supports BTAR. When constructing the API request, add the ``user_config`` object to the request body and nest the ``additional_backup_regions`` field inside. Set the value of the ``additional_backup_regions`` parameter to the name of desired cloud region.

.. code-block:: bash

    curl --request PUT                                                                       \
        --url https://api.aiven.io/v1/project/YOUR_PROJECT_NAME/service/YOUR_SERVICE_NAME    \
        --header 'Authorization: Bearer YOUR_BEARER_TOKEN'                 \
        --header 'content-type: application/json'                          \
        --data
            '{
            "user_config": {
                "additional_backup_regions": ["cloud-region-name"]
            }
        }'

..
    What's next
    -----------

    * :doc:`Manage BTAR for your Aiven service </docs/platform/howto/howto/manage-backup-to-another-region>`
    * :doc:`Disable BTAR for your Aiven service </docs/platform/howto/howto/disable-backup-to-another-region>`

    Related reading
    ---------------

    * :doc:`About the backup to another region feature in Aiven </docs/platfrom/concepts/backup-to-another-region>`
