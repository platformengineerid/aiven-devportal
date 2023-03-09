Backup to another region for Aiven services |beta|
==================================================

In  this article, you can discover the backup to another region (BTAR) feature, which you can enable on your Aiven services. Check out what BTAR allows and how you can benefit from its capabilities.

About the backup to another region
----------------------------------

The backup to another region (BTAR) feature allows existing backup files to be copied from the service's priamry backup region to one or more regions different from the primary one. As a disaster recovery feature, BTAR is particularly useful when the service-hosting region is down since it allows forking the service from an additional copy of the backup residing outside the service-hosting region.

BTAR is currently supported for the following services:

* Aiven for PostgreSQL®
* Aiven for MySQL®

How it works
------------

By enabling the backup to another region (BTAR) feature, you create an additional backup on top of the default one located in a primary region where the service is hosted. Contrary to the primary backup, the additional secondary backup is located outside the primary region.

Secondary backups are taken from primary backups, not from the service itself.

.. mermaid::

    flowchart LR
        subgraph Primary_region
            direction TB
            subgraph Service_X
                end
            subgraph Primary_backups
                direction LR
                PB1
                PB2
                PB3
                PBn
                end
            end
        subgraph Secondary_region
            direction TB
            subgraph Forked_service_X
                end
            subgraph Secondary_backups
                direction LR
                SB1
                SB2
                SB3
                SBn
                end
            end
        Service_X -- Default \n backups --> Primary_backups
        Primary_backups -- Cross-region backups \n if BTAR  enabled --> Secondary_backups
        Secondary_backups -- Secondary backups \n to restore service X --> Forked_service_X
        Service_X -- Forking service X \n if primary region down --> Forked_service_X

Benefits
--------

* Additional level of security for your data useful in case of outages or downtimes on the primary region

Limitations
-----------

* When selecting a cloud region for your additional backup, you need to use the same cloud provider that your service uses.
* When you want to :ref:`restore your service from an additional backup <fork-and-restore>` and you use a point in time to specify the scope of data to be backed up, you need to set up the time to no later than the time of taking the latest backup.
* To restore a service from an additional (not default) backup, you need to create a fork of the original service in the region where the additional backup resides.

What's next
-----------

* :doc:`Enable BTAR </docs/platform/howto/enable-backup-to-another-region>`
* :doc:`Manage BTAR </docs/platform/howto/manage-backup-to-another-region>`
* :doc:`Disable BTAR </docs/platform/howto/disable-backup-to-another-region>`
