## 🛑 Below are all the errors I hit and how I fixed them 

### **1. DBD::mysql undefined symbol error**
```
undefined symbol: mysql_sqlstate
```

**Why?**

> DBD::mysql was compiled against a different MySQL client library version than the one installed on my system.

> So the Perl driver loads…

> …but can’t talk to the MySQL client library.

***System package dependencies matter.***

#### 🛠 How I fixed it 

Disable the Perl version check in XtraBackup using the flag
```
xtrabackup --no-version-check \ ...
```

### **2. Access denied for backup user**
```
Access denied for user 'backup'
```

**Why?**
> Missing privileges.

***Backups require specific MySQL permissions.***

#### 🛠 How I fixed it 
Granted BACKUP_ADMIN + required privileges.

### 3. Permission denied on /var/lib/mysql
```
Can't change dir to '/var/lib/mysql/' (OS errno 13 - Permission denied)
```

**Why**
> Linux filesystem permissions. XtraBackup needs to read MySQL’s data directory /var/lib/mysql

***Database permissions ≠ OS permissions.***

#### 🛠 How I fixed it 
Used sudo.

### 4. Cannot mkdir /backups/full_test
```
cannot mkdir: 2 /backups/full_test/
```

**Why?**
> Directory did not exist.

***Always create parent directories***

#### 🛠 How I fixed it 
```
sudo mkdir -p /var/backups/mysql
```

### 5. datadir must be specified
```
[ERROR] datadir must be specified
```

**Why?**
> `--copy-back` requires explicit datadir.

***Restores require source AND destination.***

#### 🛠 How I fixed it 
```
--datadir=/var/lib/mysql
```
