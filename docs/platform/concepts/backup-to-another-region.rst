Backup to another region for Aiven services |beta|
==================================================

..
    About BTAR
    https://aiven.slab.com/posts/backup-to-another-region-uskzhy8d

    Backup to another region (BTAR) allows existing backup files to be copied from the service's default backup region to one or more new regions. This is a disaster recovery feature, it allows forking the service from an additional copy of the backup, residing on a different region and/or cloud provider, useful if the original region is completely down.
    Supported services
    OpenSearch (Beta)MySQL (Beta)
    PG (Beta)
    Limitations

    Cross-regions but within the same cloud provider?

    <Response [400]>
    ERROR	command failed: Error: {"errors":[{"message":"Cannot use 'azure-uae-north' for the authorized tenant","status":400}],"message":"Cannot 
    use 'azure-uae-north' for the authorized tenant"}

    PITR - Fork and restore

    You can fork/restore from PITR
    Prim backup after the last backup time - ok
    Sec backup after the last backup time - NOK
    When migrating a service

    When I move a service:
    Change of the primary backup location?? - never changes!!
    Secondary/additional backup location?? - never changes!!
    Fork and restore

    Recovery from an additional location > Fork and restore
    A backup in another region can be restored by creating a fork of the original service in the region where the secondary backup resides.
    When restoring in the sec region, the sec backup is taken. Otherwise, we use the primary backup.
