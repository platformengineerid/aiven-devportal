Remove a backup from another region |beta|
==========================================

To disable the backup to another region (BTAR) feature for your service, you can use the Aiven web console, CLI, or API. Learn a few simple steps to remove a backup of your service created in a region different from the region hosting the service.

Disable BTAR via console
------------------------

1. Log in to the `Aiven console <https://console.aiven.io/>`_.
2. From the **Current services** view, select an Aiven service for which you'd like to disable BTAR.
3. In the **Overview** tab of your service's page, navigate to **Advanced configuration** and select **Change**.
4. In the **Edit advanced configuration** window, go to the **additional backup regions** parameter and remove all its values available in the field.
5. Select **Save advanced configuration**.

Disable BTAR via CLI
--------------------

To remove secondary backups for your service, use the :ref:`avn service update <avn-cli-service-update>` command to remove all target regions names from the ``additional_backup_regions`` array.

.. code-block:: bash

    avn service update your-sevice-name                   \
        -c additional_backup_regions=[]

Disable BTAR via API
--------------------

To remove secondary backups for your service, use the `ServiceUpdate <https://api.aiven.io/doc/#tag/Service/operation/ServiceUpdate>`_ endpoint to remove all target regions names from the ``additional_backup_regions`` array.

    Existing service > `Update service configuration <https://api.aiven.io/doc/#tag/Service/operation/ServiceUpdate>`_

.. code-block:: bash

    curl --request PUT                                                                  \
    --url https://api.aiven.io/v1/project/YOUR_PROJECT_NAME/service/YOUR_SERVICE_NAME   \
    --header 'Authorization: Bearer YOUR_BEARER_TOKEN'                                  \
    --header 'content-type: application/json'                                           \
    --data
      '{
        "user_config": {
          "additional_backup_regions": []
          }
      }'

.. topic:: Result

   Your service has no longer secondary backups in regions different from its hosting region.
