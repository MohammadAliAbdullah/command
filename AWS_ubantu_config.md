connect with mysqli:
--------------------
without port: mysql -u turnbddb21 -p -h turnbddatabase.crcg60yeg6df.ap-south-1.rds.amazonaws.com
witho port: mysql -u turnbddb21 -p -h turnbddatabase.crcg60yeg6df.ap-south-1.rds.amazonaws.com -p 3306/25060

database export (home/ubantu folder):
-------------------------------------
mysqldump -u turnbddb21 -p -h turnbddatabase.crcg60yeg6df.ap-south-1.rds.amazonaws.com --routines --triggers --single-transaction --set-gtid-purged=OFF GPH_V2 > abdullah_gph_v2_backup.sql

database import (home/ubantu folder):
-------------------------------------
mysql -u turnbddb21 -p -h turnbddatabase.crcg60yeg6df.ap-south-1.rds.amazonaws.com GPH_V2 < abdullah_gph_v2_backup.sql