# MySQL Configuration and Management on Ubuntu (AWS)

This document includes essential MySQL commands to connect, export, and import a database from an AWS RDS instance on Ubuntu.

---

## 🔗 Connect with MySQLi

### ➤ Without Port
```bash
mysql -u turnbddb21 -p -h turnbddatabase.crcg60yeg6df.ap-south-1.rds.amazonaws.com
mysql -u turnbddb21 -p -h turnbddatabase.crcg60yeg6df.ap-south-1.rds.amazonaws.com -P 3306
mysql -u turnbddb21 -p -h turnbddatabase.crcg60yeg6df.ap-south-1.rds.amazonaws.com -P 25060

---

## 🔗 Export MySQLi

mysqldump -u turnbddb21 -p -h turnbddatabase.crcg60yeg6df.ap-south-1.rds.amazonaws.com \
--routines --triggers --single-transaction --set-gtid-purged=OFF GPH_V2 > abdullah_gph_v2_backup.sql

---

## 🔗 Import MySQLi

mysql -u turnbddb21 -p -h turnbddatabase.crcg60yeg6df.ap-south-1.rds.amazonaws.com GPH_V2 < abdullah_gph_v2_backup.sql
