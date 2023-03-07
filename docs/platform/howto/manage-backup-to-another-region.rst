Manage a backup to another region |beta|
========================================

Learn how to work with an Aiven service that has a backup in another region. This article details how to monitor such a service for its status, replication lag, and more. Check out how forking and restoring BTAR services is different from forking and restoring services with primary backups only. Finally, find out how to migrate a service with BTAR enabled.

Prerequisites
-------------

You have at least one Aiven service with BTAR enabled.

Monitor BTAR
------------

..
    Indicator to monitor: replication lag (e.g. WAL files), status - enabled/diabled, + which region is the target 

    Once a region is defined, the backup data is transferred on an ongoing basis, the lag between the primary and secondary region can be queried via the following endpoint.

    Backup to prim
    Copy prim backup to sec region

    if the transfer is fast enough it could also just return zero meaning 'no lag'

    API call

      curl --request POST \
    --url https://api.aiven.io/v1/project/dev-sandbox/service/mysql-star-api-test/backup_to_another_region/report \
    --header 'Authorization: Bearer 0PKPik8HZDuR+LJ8ZyGdIdS79yF0Tzq5SGwfF5OAfqLn8n1ab9mrfhf8SqZaOS6xqp2/cLYT3HOoj86FCjQUecaiK8Vf5n/G2pwYlnBatRkl17FjRFg7AmZKvVgqQR4H6T6tb1lRb4Bm72ep+9WJzfet9nZbK9sD+/D1zFw4fbyFWxVNaemMAcumP5eOh8JfSafuTmVO0LHSvVcJCt6WVliIcQZ6tBLVB6EOPlA05kK5f24fQmDTvtYZYObXQzbosnpNln2qM9GbtkKs5JQOD7Sv9VP7sHLzWvvbCxN5+mdW+aYiI+vr3Vh31xqxs0ct6HwHyo65xbiKEs8UzfSUAPhwMI/6uM1afqvjwlYyjlYAfBMRQQ==' \
    --header 'content-type: application/json' \
    --data '{"period":"hour"}'

    As an output, you get metrics for replication lags at speficis points in time.

    Fork and restore
    ----------------

    Recovery from an additional location > Fork and restore

    A backup in another region can be restored by creating a fork of the original service in the region where the secondary backup resides.

    When restoring in the sec region, the sec backup is taken. Otherwise, we use the primary backup.

    PITR - Fork and restore

    You can fork/restore from PITR
    Prim backup after the last backup time - ok
    Sec backup after the last backup time - NOK

    Migrate a service with BTAR
    ---------------------------

    You can migrate a service with BTAR the same way you migrate a service with a regualr backup.

    .. topic:: Location of a backup


    When I move a service:
    Change of the primary backup location?? - never changes!!
    Secondary/additional backup location?? - never changes!!
