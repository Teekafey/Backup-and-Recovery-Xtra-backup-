# Backup-and-Recovery-Xtra-backup-
MySQL backup and recovery involves creating copies of your database to restore from in case of data loss, corruption, or hardware failure. The primary methods are logical backups (using mysqldump) and physical backups (copying raw data files). 

In this project we will be working with percona xtraBackup; popular third-party tool for physical backups, known for online, non-blocking backups of InnoDB tables.

## Environment Setup
* **OS**: Ubuntu (VirtualBox)
* **Database**: MySQL 8.0
* **Backup Tool**: Percona XtraBackup
* **Client Tool**: DBeaver

**Datadir:**
```
/var/lib/mysql
```

**Backup directory used:**

```
/var/backups/mysql
```

#### [Full Documentation](https://github.com/Teekafey/Backup-and-Recovery-Xtra-backup-/blob/main/Backup%20%26%20Restore.md)
