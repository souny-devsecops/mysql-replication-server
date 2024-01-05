# Configure Master Server
### 1. Log in to the MySQL on the master:
        mysql -u root -p

### 2. Create a replication user:
        CREATE USER 'replication_user'@'192.168.1.2' IDENTIFIED BY 'your_password';

Replace 'replication_user' with the desired username and 'your_password' with the actual password you want to set for the user and 192.168.1.2 is the slave host from which the user is allowed to connect.

### 3. Grant replication privileges:
        GRANT REPLICATION SLAVE ON *.* TO 'replication_user'@'192.168.1.2';

This grants the necessary replication privileges to the user.

### 4. Flush privileges to apply changes:
        FLUSH PRIVILEGES;

This ensures that the changes take effect immediately.

### 5. Get MASTER_LOG_FILE and MASTER_LOG_POS from master:
        SHOW MASTER STATUS;

 File     | Position | Binlog_Do_DB

+----------+----------+----------

| 1.000003 |     4312 | db_demo 

MASTER_LOG_FILE = '1.000003'

MASTER_LOG_POS = 4312


# Configure Slave Server
### 1. Log in to the MySQL on the slave:
        mysql -u root -p

### 2. Configure the slave to connect to the master:
        CHANGE MASTER TO MASTER_HOST='10.150.1.85',MASTER_USER='replication_user',MASTER_PASSWORD='your_password',MASTER_LOG_FILE='1.000003',MASTER_LOG_POS=4312,MASTER_PORT=3307;

Make sure to replace '192.168.1.1' is mysql master, 'replication_user', and 'your_password' with the appropriate values.

### 3. Start the slave:
        START SLAVE;

This starts the replication process.

### 4. Check the slave status:
        SHOW SLAVE STATUS;

Look for the "Slave_IO_Running" and "Slave_SQL_Running" fields. If both are set to "Yes," replication is working.


# How to Reset MySQL Master-Slave Replication
### Step 1 - Login to MySql slave and stop the slave
        STOP SLAVE;
### Step 2 - Reset the slave
        RESET SLAVE;

### Step 3 - Reset the master
        RESET MASTER;
# mysql-replication-server
