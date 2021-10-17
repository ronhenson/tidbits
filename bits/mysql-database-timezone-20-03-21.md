---
title: "Mysql Database timezone 20 03 21"
created: 2003-11-10 10:10:10
---

tags: #database, #issue
keywords: java, spring-boot, mysql, date, time, datetime

# Mysql Database timezone 20 03 21

Connecting to local database through Spring when Pacific Standard time moved to PDT:

```log
SQL Error: 0, SQLState: 01S00
The server time zone value 'PDT' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the 'serverTimezone' configuration property) to use a more specific time zone value if you want to utilize time zone support.
```

Made the following changes to MySql:

(these changes reverted back to SYSTEM with reboot
)

```sql
mysql -u root -p
SELECT @@global.time_zone

<returned> SYSTEM

SET GLOBAL time_zone = -7:00;
SELECT @@global.time_zone

<returned> -7:00
```

# Option 2 (Works and persistent)

MySQL settings can be changed by editing the main my.cnf configuration file. Open the file for editing:

```bsh
sudo nano /etc/mysql/my.cnf
```

Scroll down to the [mysqld] section, and find the default-time-zone = "+00:00" line. Change the +00:00 value to the GMT value for the time zone you want. Save the file and exit.

In the example below we set the MySQL Server time zone to +08:00 (GMT +8).

```bsh
[mysqld]     # Had to add this statement
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mysql.conf.d/
default-time-zone = "-7:00"  # had to add this statement
```

Added this to `my.cnf`.

## Solved the problem
