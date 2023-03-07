Remove a backup from another region |beta|
==========================================

..
    BTAR via console

    DISABLE ???

    Ticket to be created
    BTAR via CLI

    DISABLE ???

    avn service update mysql-btar-cli-test        \
    -c additional_backup_regions=null

    avn-service-update https://docs.aiven.io/docs/tools/cli/service#avn-service-update ???

    https://aiven.atlassian.net/browse/AD-1315
    BTAR via API

    DISABLE ???

    Existing service > `Update service configuration <https://api.aiven.io/doc/#tag/Service/operation/ServiceUpdate>`_

    curl --request PUT                                                                     \
    --url https://api.aiven.io/v1/project/YOUR_PROJECT_NAME/service/YOUR_SERVICE_NAME   \
    --header 'Authorization: Bearer YOUR_BEARER_TOKEN'                                  \
    --header 'content-type: application/json'                                           \
    --data
        '{
            "user_config": {
            "additional_backup_regions": []]
            }
        }'
