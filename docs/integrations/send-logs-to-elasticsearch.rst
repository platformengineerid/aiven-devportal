Send logs to Elasticsearch®
======================================

You can store logs from one of your Aiven services in an external Elasticsearch service.

You'll need the following values for the connection:

============================     ==========================================================================================================
Variable                         Description
============================     ==========================================================================================================
``ELASTICSEARCH_USER``           User name to access the Elasticsearch service.
``ELASTICSEARCH_PASSWORD``       Password to access the Elasticsearch service.
``ELASTICSEARCH_HOST``           HTTPS service host of your external Elasticsearch service.
``ELASTICSEARCH_PORT``           Port to use for the connection.
``CA_CERTIFICATE``               CA certificate in PEM structure (if necessary).
``CONNECTION_NAME``              Name of your choosing for this external connection, that will be used with Aiven services.
============================     ==========================================================================================================

Create external Elasticsearch integration
-------------------------------------------

Start by setting up an external service integration for Elasticsearch.

1. Log in to the `Aiven Console <https://console.aiven.io/>`_.
#. Navigate to **Service Integration** from the menu on the left.
#. You'll see a list of external services you can integrate with Aiven.
#. Select **External Elasticsearch** from the list.
#. Select **Add new endpoint**.
#. Set a preferred *endpoint name*, we'll call it ``CONNECTION_NAME`` later.
#. In the connection URL field set the connection string in a format ``https://ELASTICSEARCH_USER:ELASTICSEARCH_PASSWORD@ELASTICSEARCH_HOST:ELASTICSEARCH_PORT``, using your own values for those parameters.
#. Set desired index prefix, that doesn't overlap with any of already existing indexes in your Elasticsearch service.
#. If you need a certificate to access the endpoint, add the body of your CA certificate in PEM format. This field is optional.
#. Set other fields based on your requirements, or leave the default values there.
#. Select **Create**.

A new service integration will be added. You can now reference your service by the ``CONNECTION_NAME`` you chose.


Send logs to an external service
---------------------------------

#. Navigate to **Services** from the menu on the left.
#. Select the service which logs you want to send to the external Elasticsearch service.
#. On the **Overview** page of your service, navigate to the **Service integrations** section.
#. Select **Manage integrations**. 
#. Select **Elasticsearch Logs** from the list.
#. In the newly-appeared modal window, select the endpoint with name ``CONNECTION_NAME`` from the list and select **ENABLE**. Close the modal window.
#. Observe the status change for newly-added integration in the **Service integrations** section on the **Overview** page of your service.
#. Verify that the logs are flowing into your Elasticsearch.

.. note:: Logs are split per day with index name consisting of your desired index prefix and a date in a format year-month-day, for example ``logs-2022-08-30``.

.. note:: You can also set up the integration using Aiven CLI and the following commands:
  
   - :ref:`avn service integration-endpoint-create <avn_service_integration_endpoint_create>`
   - :ref:`avn service integration-endpoint-list <avn_service_integration_endpoint_list>` 
   - :ref:`avn service integration-create <avn_service_integration_create>`


.. warning:: Integration are not available on Hobbyist plans. If you want to enable integrations please select at least a startup plan.


