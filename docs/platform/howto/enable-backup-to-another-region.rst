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

.. topic:: Result
   
   Your new additional backup is visible in
   
   * **Overview** tab > **Advanced configuration** section > **additional_backup_regions**
   * **Backups** tab > **Secondary backup location**.

..
    Back up to another region with CLI
    ----------------------------------

    Using CLI, you can enable CCR for

    * New Aiven for Apache Cassandra service by :ref:`creating a totally new CCR service pair <new-ccr-service-pair>` or
    * Existing Aiven for Apache Cassandra service by :ref:`adding a CCR peer service in another region to an existing service <new-ccr-peer-service>`.

    .. note::
    
    In this instruction, the :doc:`Aiven CLI client </docs/tools/cli>` is used to interact with Aiven APIs.

    .. topic:: Understand parameters to be supplied

    * ``service_to_join_with`` parameter value needs to be set to a name of an existing service in the same project. The supplied service name indicates the service you connect to for enabling CCR. The two connected services create a CCR service pair.
    * ``cassandra.datacenter`` is a datacenter name used to identify nodes from a particular service in the cluster's topology. In CCR for Aiven for Apache Cassandra, all nodes of either of the two services belong to a single datacenter; therefore, a value of the ``cassandra.datacenter`` parameter needs to be unique for each service. It's recommended to set it equal to the service name.

    .. _new-ccr-service-pair:

    Create a new CCR service pair
    '''''''''''''''''''''''''''''

    1. Use the :ref:`avn service create <avn-cli-service-create>` command to create a new service (``service_1``).

    .. code-block:: bash

        avn service create                                   \
            --service-type cassandra                          \
            --cloud cloud_region_name                         \
            --plan service_plan_name                          \
            -c cassandra.datacenter=datacenter_1_name         \
            service_1_name

    2. Create another new service (``service_2``). This time, include the ``service_to_join_with`` parameter to connect it to ``service_1`` and create a CCR pair. Set the value of the ``service_to_join_with`` parameter to the name of ``service_1``.

    .. important::

        See :ref:`Limitations <ccr-limitations>` before you set the parameters.

    .. code-block:: bash

        avn service create                                   \
            --service-type cassandra                          \
            --cloud cloud_region_name                         \
            --plan service_plan_name                          \
            -c cassandra.datacenter=datacenter_2_name         \
            -c service_to_join_with=service_1_name            \
            service_2_name

    .. _new-ccr-peer-service:

    Add a CCR peer to an existing service
    '''''''''''''''''''''''''''''''''''''

    Use the :ref:`avn service create <avn-cli-service-create>` command to create a new service with CCR enabled. Use the ``service_to_join_with`` parameter to connect your new service to an existing service creating a CCR pair. Set the value of the ``service_to_join_with`` parameter to the name of the existing service.

    .. important::

    See :ref:`Limitations <ccr-limitations>` before you set the parameters.

    .. code-block:: bash

    avn service create                                   \
        --service-type cassandra                          \
        --cloud cloud_region_name                         \
        --plan service_plan_name                          \
        -c cassandra.datacenter=datacenter_name           \
        -c service_to_join_with=existing_service_name     \
        new_service_name

    Back up to another region with API
    ----------------------------------

    Using :doc:`Aiven APIs </docs/tools/api>`, you can enable CCR for

    * New Aiven for Apache Cassandra service by :ref:`creating a totally new CCR service pair <new-ccr-pair>` or
    * Existing Aiven for Apache Cassandra service by :ref:`adding a CCR peer service in another region to an existing service <new-ccr-peer>`.

    .. note::
    
    In this instruction, the `curl` command line tool is used to interact with Aiven APIs.

    .. topic:: Understand parameters to be supplied

    * ``service_to_join_with`` parameter value needs to be set to a name of an existing service in the same project. The supplied service name indicates the service you connect to for enabling CCR. The two connected services create a CCR service pair.
    * ``cassandra.datacenter`` is a datacenter name used to identify nodes from a particular service in the cluster's topology. In CCR for Aiven for Apache Cassandra, all nodes of either of the two services belong to a single datacenter; therefore, a value of the ``cassandra.datacenter`` parameter needs to be unique for each service. It's recommended to set it equal to the service name.

    .. _new-ccr-pair:

    Create a new CCR service pair
    '''''''''''''''''''''''''''''

    Use the `ServiceCreate <https://api.aiven.io/doc/#tag/Service/operation/ServiceCreate>`_ API to create a new service with CCR enabled. When constructing the API request, add the ``user_config`` object to the request body and nest inside it the ``service_to_join_with`` and ``datacenter`` fields.

    1. Use the `ServiceCreate <https://api.aiven.io/doc/#tag/Service/operation/ServiceCreate>`_ API to create a new service (``service_1``).

    .. code-block:: bash

        curl --request POST                                                   \
            --url https://api.aiven.io/v1/project/YOUR_PROJECT_NAME/service    \
            --header 'Authorization: Bearer YOUR_BEARER_TOKEN'                 \
            --header 'content-type: application/json'                          \
            --data
                '{
                "cloud": "string",
                "plan": "string",
                "service_name": "service_1_name",
                "service_type": "cassandra"
                }'

    2. Create another new service (``service_2``). This time when constructing the API request, add the ``user_config`` object to the request body and nest inside it the ``service_to_join_with`` and ``datacenter`` fields. Set the value of the ``service_to_join_with`` parameter to the name of ``service_1`` to connect both services and create a CCR pair.

    .. important::

        See :ref:`Limitations <ccr-limitations>` before you set the parameters.

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
                    "cassandra": {
                        "datacenter": "datacenter_name"
                    },
                    "service_to_join_with": "service_1_name"
                }
                }'

    .. _new-ccr-peer:

    Add a CCR peer to an existing service
    '''''''''''''''''''''''''''''''''''''

    Use the `ServiceCreate <https://api.aiven.io/doc/#tag/Service/operation/ServiceCreate>`_ API to create a new service with CCR enabled. When constructing the API request, add the ``user_config`` object to the request body and nest inside it the ``service_to_join_with`` and ``datacenter`` fields. Set the value of the ``service_to_join_with`` parameter to the name of your existing service to connect it to your new service and create a CCR pair.

    .. important::

    See :ref:`Limitations <ccr-limitations>` before you set the parameters.

    .. code-block:: bash

        curl --request POST                                                   \
            --url https://api.aiven.io/v1/project/YOUR_PROJECT_NAME/service    \
            --header 'Authorization: Bearer YOUR_BEARER_TOKEN'                 \
            --header 'content-type: application/json'                          \
            --data
                '{
                "cloud": "string",
                "plan": "string",
                "service_name": "new_service_name",
                "service_type": "cassandra",
                "user_config": {
                    "cassandra": {
                        "datacenter": "datacenter_name"
                    },
                    "service_to_join_with": "existing_service_name"
                }
                }'

    What's next
    -----------

    * :doc:`Manage CCR on Aiven for Apache Cassandra </docs/products/cassandra/howto/manage-cross-cluster-replication>`
    * :doc:`Disable CCR on Aiven for Apache Cassandra </docs/products/cassandra/howto/disable-cross-cluster-replication>`

    Related reading
    ---------------

    * :doc:`About cross-cluster replication on Aiven for Apache Cassandra </docs/products/cassandra/concepts/cross-cluster-replication>`
    * `Multi-master Replication: Versioned Data and Tunable Consistency <https://cassandra.apache.org/doc/latest/cassandra/architecture/dynamo.html#multi-master-replication-versioned-data-and-tunable-consistency>`_
    * :doc:`OpenSearch® cross-cluster replication</docs/products/opensearch/concepts/cross-cluster-replication-opensearch>`
    * :doc:`Set up cross-cluster replication for OpenSearch</docs/products/opensearch/howto/setup-cross-cluster-replication-opensearch>`
    * :doc:`Enabling cross-cluster replication for Apache Kafka® via Terraform</docs/tools/terraform/reference/cookbook/kafka-mirrormaker-recipe>`
