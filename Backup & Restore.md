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

## PHASE 1 - Full Physical Backup
1. **Create backup user**
```sql
CREATE USER 'backup'@'localhost' IDENTIFIED BY 'backup123';

GRANT BACKUP_ADMIN, PROCESS, RELOAD,
LOCK TABLES, REPLICATION CLIENT
ON *.* TO 'backup'@'localhost';

FLUSH PRIVILEGES;
```

Using "backup123" for an example.

2. **Take Full Backup**
```bash
sudo xtrabackup --backup \
--target-dir=/var/backups/mysql/full_test \
--user=backup --password=backup123
``` 

![full_backup](https://github.com/user-attachments/assets/c6586977-e8d4-49e6-9837-e33a60dda4e6)


3. **Prepare Backup (apply redo logs)**
```
sudo xtrabackup --prepare \
--target-dir=/var/backups/mysql/full_test
```
This makes backup consistent.

![prepare_backup](https://github.com/user-attachments/assets/eff2650f-1780-462e-8dff-735698a56f33)

## PHASE 2 - Disaster
```sql
DROP TABLE customers;
```
<img width="487" height="68" alt="image" src="https://github.com/user-attachments/assets/5a4821eb-46b1-4b7c-8c7e-c38cfad0d34c" />

## PHASE 3 - Full Restore
1. **Stop MySQL**
```
sudo systemctl stop mysql
```

2. **Copy Back**
```
sudo xtrabackup --copy-back \
--target-dir=/var/backups/mysql/full_test \
--datadir=/var/lib/mysql
```

3. **Fix Permissions**
```
sudo chown -R mysql:mysql /var/lib/mysql
```

4. **Start Mysql**
```
sudo systemctl start mysql
```

Database restored.

<img width="526" height="187" alt="image" src="https://github.com/user-attachments/assets/b64b9ca2-8098-46f4-a17a-71accd234637" />
