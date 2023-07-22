### Follow all commands on Master

# Install Maria db 

`[root@master ~]# yum install mariadb-server mariadb-devel -y`

    Loaded plugins: fastestmirror, langpacks
    Loading mirror speeds from cached hostfile
     * base: centos.mirror.net.in
     * epel: repo.extreme-ix.org
     * extras: centos.mirror.net.in
     * updates: centos.mirror.net.in
    Package 1:mariadb-devel-5.5.68-1.el7.x86_64 already installed and latest version
    Resolving Dependencies
    --> Running transaction check
    ---> Package mariadb-server.x86_64 1:5.5.68-1.el7 will be installed
    --> Processing Dependency: mariadb(x86-64) = 1:5.5.68-1.el7 for package: 1:mariadb-server-5.5.68-1.el7.x86_64
    --> Processing Dependency: perl-DBI for package: 1:mariadb-server-5.5.68-1.el7.x86_64
    --> Processing Dependency: perl-DBD-MySQL for package: 1:mariadb-server-5.5.68-1.el7.x86_64
    --> Processing Dependency: perl(DBI) for package: 1:mariadb-server-5.5.68-1.el7.x86_64
    --> Running transaction check
    ---> Package mariadb.x86_64 1:5.5.68-1.el7 will be installed
    ---> Package perl-DBD-MySQL.x86_64 0:4.023-6.el7 will be installed
    ---> Package perl-DBI.x86_64 0:1.627-4.el7 will be installed
    --> Processing Dependency: perl(RPC::PlServer) >= 0.2001 for package: perl-DBI-1.627-4.el7.x86_64
    --> Processing Dependency: perl(RPC::PlClient) >= 0.2000 for package: perl-DBI-1.627-4.el7.x86_64
    --> Running transaction check
    ---> Package perl-PlRPC.noarch 0:0.2020-14.el7 will be installed
    --> Processing Dependency: perl(Net::Daemon) >= 0.13 for package: perl-PlRPC-0.2020-14.el7.noarch
    --> Processing Dependency: perl(Net::Daemon::Test) for package: perl-PlRPC-0.2020-14.el7.noarch
    --> Processing Dependency: perl(Net::Daemon::Log) for package: perl-PlRPC-0.2020-14.el7.noarch
    --> Processing Dependency: perl(Compress::Zlib) for package: perl-PlRPC-0.2020-14.el7.noarch
    --> Running transaction check
    ---> Package perl-IO-Compress.noarch 0:2.061-2.el7 will be installed
    --> Processing Dependency: perl(Compress::Raw::Zlib) >= 2.061 for package: perl-IO-Compress-2.061-2.el7.noarch
    --> Processing Dependency: perl(Compress::Raw::Bzip2) >= 2.061 for package: perl-IO-Compress-2.061-2.el7.noarch
    ---> Package perl-Net-Daemon.noarch 0:0.48-5.el7 will be installed
    --> Running transaction check
    ---> Package perl-Compress-Raw-Bzip2.x86_64 0:2.061-3.el7 will be installed
    ---> Package perl-Compress-Raw-Zlib.x86_64 1:2.061-4.el7 will be installed
    --> Finished Dependency Resolution
    
    Dependencies Resolved
    
    ================================================================================
     Package                      Arch        Version               Repository
                                                                               Size
    ================================================================================
    Installing:
     mariadb-server               x86_64      1:5.5.68-1.el7        base       11 M
    Installing for dependencies:
     mariadb                      x86_64      1:5.5.68-1.el7        base      8.8 M
     perl-Compress-Raw-Bzip2      x86_64      2.061-3.el7           base       32 k
     perl-Compress-Raw-Zlib       x86_64      1:2.061-4.el7         base       57 k
     perl-DBD-MySQL               x86_64      4.023-6.el7           base      140 k
     perl-DBI                     x86_64      1.627-4.el7           base      802 k
     perl-IO-Compress             noarch      2.061-2.el7           base      260 k
     perl-Net-Daemon              noarch      0.48-5.el7            base       51 k
     perl-PlRPC                   noarch      0.2020-14.el7         base       36 k
    
    Transaction Summary
    ================================================================================
    Install  1 Package (+8 Dependent packages)
    
    Total download size: 21 M
    Installed size: 110 M
    Downloading packages:
    (1/9): perl-Compress-Raw-Bzip2-2.061-3.el7.x86_64.rpm      |  32 kB   00:00
    (2/9): perl-DBI-1.627-4.el7.x86_64.rpm                     | 802 kB   00:00
    (3/9): perl-IO-Compress-2.061-2.el7.noarch.rpm             | 260 kB   00:00
    (4/9): perl-Net-Daemon-0.48-5.el7.noarch.rpm               |  51 kB   00:00
    (5/9): perl-PlRPC-0.2020-14.el7.noarch.rpm                 |  36 kB   00:00
    (6/9): perl-Compress-Raw-Zlib-2.061-4.el7.x86_64.rpm       |  57 kB   00:00
    (7/9): perl-DBD-MySQL-4.023-6.el7.x86_64.rpm               | 140 kB   00:00
    (8/9): mariadb-5.5.68-1.el7.x86_64.rpm                     | 8.8 MB   00:00
    (9/9): mariadb-server-5.5.68-1.el7.x86_64.rpm                                             |  11 MB  00:00:18
    ----------------------------------------------------------------------------------------------------------------
    Total                                                                            1.2 MB/s |  21 MB  00:00:18
    Running transaction check
    Running transaction test
    Transaction test succeeded
    Running transaction
      Installing : 1:perl-Compress-Raw-Zlib-2.061-4.el7.x86_64                                                   1/9
      Installing : 1:mariadb-5.5.68-1.el7.x86_64                                                                 2/9
      Installing : perl-Net-Daemon-0.48-5.el7.noarch                                                             3/9
      Installing : perl-Compress-Raw-Bzip2-2.061-3.el7.x86_64                                                    4/9
      Installing : perl-IO-Compress-2.061-2.el7.noarch                                                           5/9
      Installing : perl-PlRPC-0.2020-14.el7.noarch                                                               6/9
      Installing : perl-DBI-1.627-4.el7.x86_64                                                                   7/9
      Installing : perl-DBD-MySQL-4.023-6.el7.x86_64                                                             8/9
      Installing : 1:mariadb-server-5.5.68-1.el7.x86_64                                                          9/9
      Verifying  : perl-Compress-Raw-Bzip2-2.061-3.el7.x86_64                                                    1/9
      Verifying  : perl-Net-Daemon-0.48-5.el7.noarch                                                             2/9
      Verifying  : 1:mariadb-server-5.5.68-1.el7.x86_64                                                          3/9
      Verifying  : perl-DBD-MySQL-4.023-6.el7.x86_64                                                             4/9
      Verifying  : 1:mariadb-5.5.68-1.el7.x86_64                                                                 5/9
      Verifying  : 1:perl-Compress-Raw-Zlib-2.061-4.el7.x86_64                                                   6/9
      Verifying  : perl-DBI-1.627-4.el7.x86_64                                                                   7/9
      Verifying  : perl-IO-Compress-2.061-2.el7.noarch                                                           8/9
      Verifying  : perl-PlRPC-0.2020-14.el7.noarch                                                               9/9
    
    Installed:
      mariadb-server.x86_64 1:5.5.68-1.el7
    
    Dependency Installed:
      mariadb.x86_64 1:5.5.68-1.el7                          perl-Compress-Raw-Bzip2.x86_64 0:2.061-3.el7
      perl-Compress-Raw-Zlib.x86_64 1:2.061-4.el7            perl-DBD-MySQL.x86_64 0:4.023-6.el7
      perl-DBI.x86_64 0:1.627-4.el7                          perl-IO-Compress.noarch 0:2.061-2.el7
      perl-Net-Daemon.noarch 0:0.48-5.el7                    perl-PlRPC.noarch 0:0.2020-14.el7
    
    Complete!

# Enable mariadb service

`[root@master ~]# systemctl enable mariadb`

    Created symlink from /etc/systemd/system/multi-user.target.wants/mariadb.service to /usr/lib/systemd/system/mariadb.service.
    [root@master ~]# systemctl start mariadb
    [root@master ~]# systemctl status mariadb
    ● mariadb.service - MariaDB database server
       Loaded: loaded (/usr/lib/systemd/system/mariadb.service; enabled; vendor preset: disabled)
       Active: active (running) since Tue 2023-07-18 20:25:22 IST; 1s ago
      Process: 12885 ExecStartPost=/usr/libexec/mariadb-wait-ready $MAINPID (code=exited, status=0/SUCCESS)
      Process: 12802 ExecStartPre=/usr/libexec/mariadb-prepare-db-dir %n (code=exited, status=0/SUCCESS)
     Main PID: 12884 (mysqld_safe)
        Tasks: 20
       CGroup: /system.slice/mariadb.service
               ├─12884 /bin/sh /usr/bin/mysqld_safe --basedir=/usr
               └─13049 /usr/libexec/mysqld --basedir=/usr --datadir=/var/lib/mysql --plugin-dir=/usr/lib64/mysql/...

    Jul 18 20:25:20 master mariadb-prepare-db-dir[12802]: MySQL manual for more instructions.
    Jul 18 20:25:20 master mariadb-prepare-db-dir[12802]: Please report any problems at http://mariadb.org/jira
    Jul 18 20:25:20 master mariadb-prepare-db-dir[12802]: The latest information about MariaDB is available at...g/.
    Jul 18 20:25:20 master mariadb-prepare-db-dir[12802]: You can find additional information about the MySQL ...at:
    Jul 18 20:25:20 master mariadb-prepare-db-dir[12802]: http://dev.mysql.com
    Jul 18 20:25:20 master mariadb-prepare-db-dir[12802]: Consider joining MariaDB's strong and vibrant community:
    Jul 18 20:25:20 master mariadb-prepare-db-dir[12802]: https://mariadb.org/get-involved/
    Jul 18 20:25:20 master mysqld_safe[12884]: 230718 20:25:20 mysqld_safe Logging to '/var/log/mariadb/maria...og'.
    Jul 18 20:25:20 master mysqld_safe[12884]: 230718 20:25:20 mysqld_safe Starting mysqld daemon with databa...ysql
    Jul 18 20:25:22 master systemd[1]: Started MariaDB database server.
    Hint: Some lines were ellipsized, use -l to show in full.

 # Enter into mariadb for config:
 
`[root@master ~]# mysql`

    Welcome to the MariaDB monitor.  Commands end with ; or \g.
    Your MariaDB connection id is 2
    Server version: 5.5.68-MariaDB MariaDB Server
    
    Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    MariaDB [(none)]> GRANT ALL ON slurm_accounts.* TO 'slurm'@'localhost' IDENTIFIED BY '1234' with grant option;
    Query OK, 0 rows affected (0.00 sec)
    
    MariaDB [(none)]> SHOW VARIABLES LIKE 'have_innodb';
    +---------------+-------+
    | Variable_name | Value |
    +---------------+-------+
    | have_innodb   | YES   |
    +---------------+-------+
    1 row in set (0.00 sec)
    
    MariaDB [(none)]> FLUSH PRIVILEGES;
    Query OK, 0 rows affected (0.00 sec)
    
    MariaDB [(none)]> CREATE DATABASE slurm_accounts;
    Query OK, 1 row affected (0.00 sec)
    
    MariaDB [(none)]> quit;
    Bye

# Check the databases grants for the slurm user:

 `[root@master ~]# mysql -p -u slurm`
    
    Enter password:
    Welcome to the MariaDB monitor.  Commands end with ; or \g.
    Your MariaDB connection id is 3
    Server version: 5.5.68-MariaDB MariaDB Server
    
    Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    MariaDB [(none)]> show grants;
    +--------------------------------------------------------------------------------------------------------------+
    | Grants for slurm@localhost                                                                                   |
    +--------------------------------------------------------------------------------------------------------------+
    | GRANT USAGE ON *.* TO 'slurm'@'localhost' IDENTIFIED BY PASSWORD '*A4B6157319038724E3560894F7F932C8886EBFCF' |
    | GRANT ALL PRIVILEGES ON `slurm_accounts`.* TO 'slurm'@'localhost' WITH GRANT OPTION                          |
    +--------------------------------------------------------------------------------------------------------------+
    2 rows in set (0.00 sec)
    
    MariaDB [(none)]> quit
    Bye

# Create a new file /etc/my.cnf.d/innodb.cnf containing:

`[root@master ~]# vim  /etc/my.cnf.d/innodb.cnf`
    
    [mysqld]
    innodb_buffer_pool_size=1024M
    innodb_log_file_size=64M
    innodb_lock_wait_timeout=900
    
# To implement this change you have to shut down the database and move/remove logfiles:

    [root@master ~]# systemctl stop mariadb
    
    [root@master ~]# mv /var/lib/mysql/ib_logfile? /tmp/
    
    [root@master ~]# systemctl start mariadb


# You can check the current setting in MySQL like so:

`[root@master ~]# mysql`

    Welcome to the MariaDB monitor.  Commands end with ; or \g.
    Your MariaDB connection id is 2
    Server version: 5.5.68-MariaDB MariaDB Server
    
    Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    MariaDB [(none)]> SHOW VARIABLES LIKE 'innodb_buffer_pool_size';
    +-------------------------+------------+
    | Variable_name           | Value      |
    +-------------------------+------------+
    | innodb_buffer_pool_size | 1073741824 |
    +-------------------------+------------+
    1 row in set (0.00 sec)
    
    MariaDB [(none)]> exit
    Bye


# Create slurmdbd configuration file:

`[root@master ~]# vim /etc/slurm/slurmdbd.conf`

    #
    # Example slurmdbd.conf file.
    #
    # See the slurmdbd.conf man page for more information.
    #
    # Archive info
    #ArchiveJobs=yes
    #ArchiveDir="/tmp"
    #ArchiveSteps=yes
    #ArchiveScript=
    #JobPurge=12
    #StepPurge=1
    #
    # Authentication info
    AuthType=auth/munge
    #AuthInfo=/var/run/munge/munge.socket.2
    #
    # slurmDBD info
    DbdAddr=localhost
    DbdHost=localhost
    #DbdPort=6819
    SlurmUser=slurm
    #MessageTimeout=300
    DebugLevel=verbose
    #DefaultQOS=normal,standby
    LogFile=/var/log/slurm/slurmdbd.log
    PidFile=/var/run/slurmdbd.pid
    #PluginDir=/usr/lib/slurm
    #PrivateData=accounts,users,usage,jobs
    #TrackWCKey=yes
    #
    # Database info
    StorageType=accounting_storage/mysql
    #StorageHost=localhost
    #StoragePort=1234
    StoragePass=1234
    StorageUser=slurm
    StorageLoc=slurm_accounts
    
# Set up files and permissions:

    [root@master ~]# chown slurm: /etc/slurm/slurmdbd.conf
    
    [root@master ~]# chmod 600 /etc/slurm/slurmdbd.conf
    
    [root@master ~]# touch /var/log/slurmdbd.log
    
    [root@master ~]# chown slurm: /var/log/slurmdbd.log

# Fire slurndbd manually to see the log:

`[root@master ~]# slurmdbd -D -vvv`

    slurmdbd: debug:  Log file re-opened
    slurmdbd: debug:  auth/munge: init: Munge authentication plugin loaded
    slurmdbd: debug2: accounting_storage/as_mysql: init: mysql_connect() called for db slurm_accounts
    slurmdbd: debug2: Attempting to connect to localhost:3306
    slurmdbd: accounting_storage/as_mysql: _check_mysql_concat_is_sane: MySQL server version is: 5.5.68-MariaDB
    slurmdbd: debug2: accounting_storage/as_mysql: _check_database_variables: innodb_buffer_pool_size: 1073741824
    slurmdbd: debug2: accounting_storage/as_mysql: _check_database_variables: innodb_log_file_size: 67108864
    slurmdbd: debug2: accounting_storage/as_mysql: _check_database_variables: innodb_lock_wait_timeout: 900
    slurmdbd: accounting_storage/as_mysql: init: Accounting storage MYSQL plugin loaded
    slurmdbd: debug2: ArchiveDir        = /tmp
    slurmdbd: debug2: ArchiveScript     = (null)
    slurmdbd: debug2: AuthAltTypes      = (null)
    slurmdbd: debug2: AuthAltParameters = (null)
    slurmdbd: debug2: AuthInfo          = (null)
    slurmdbd: debug2: AuthType          = auth/munge
    slurmdbd: debug2: CommitDelay       = 0
    slurmdbd: debug2: CommunicationParameters       = (null)
    slurmdbd: debug2: DbdAddr           = localhost
    slurmdbd: debug2: DbdBackupHost     = (null)
    slurmdbd: debug2: DbdHost           = localhost
    slurmdbd: debug2: DbdPort           = 6819
    slurmdbd: debug2: DebugFlags        = (null)
    slurmdbd: debug2: DebugLevel        = 6
    slurmdbd: debug2: DebugLevelSyslog  = 10
    slurmdbd: debug2: DefaultQOS        = (null)
    slurmdbd: debug2: LogFile           = /var/log/slurm/slurmdbd.log
    slurmdbd: debug2: MessageTimeout    = 10
    slurmdbd: debug2: Parameters        = (null)
    slurmdbd: debug2: PidFile           = /var/run/slurmdbd.pid
    slurmdbd: debug2: PluginDir         = /usr/lib64/slurm
    slurmdbd: debug2: PrivateData       = none
    slurmdbd: debug2: PurgeEventAfter   = NONE
    slurmdbd: debug2: PurgeJobAfter     = NONE
    slurmdbd: debug2: PurgeResvAfter    = NONE
    slurmdbd: debug2: PurgeStepAfter    = NONE
    slurmdbd: debug2: PurgeSuspendAfter = NONE
    slurmdbd: debug2: PurgeTXNAfter = NONE
    slurmdbd: debug2: PurgeUsageAfter = NONE
    slurmdbd: debug2: SlurmUser         = slurm(900)
    slurmdbd: debug2: StorageBackupHost = (null)
    slurmdbd: debug2: StorageHost       = localhost
    slurmdbd: debug2: StorageLoc        = slurm_accounts
    slurmdbd: debug2: StorageParameters = (null)
    slurmdbd: debug2: StoragePort       = 3306
    slurmdbd: debug2: StorageType       = accounting_storage/mysql
    slurmdbd: debug2: StorageUser       = slurm
    slurmdbd: debug2: TCPTimeout        = 2
    slurmdbd: debug2: TrackWCKey        = 0
    slurmdbd: debug2: TrackSlurmctldDown= 0
    slurmdbd: debug2: accounting_storage/as_mysql: acct_storage_p_get_connection: acct_storage_p_get_connection: request new connection 1
    slurmdbd: debug2: Attempting to connect to localhost:3306
    slurmdbd: slurmdbd version 20.11.9 started
    slurmdbd: debug2: running rollup at Tue Jul 18 20:38:50 2023
    slurmdbd: debug2: accounting_storage/as_mysql: as_mysql_roll_usage: Everything rolled up
    ^Cslurmdbd: Terminate signal (SIGINT or SIGTERM) received
    slurmdbd: debug:  rpc_mgr shutting down
    slurmdbd: Unable to remove pidfile '/var/run/slurmdbd.pid': Permission denied



# Start the slurmdbd service:

    [root@master ~]# systemctl enable slurmdbd
    
    [root@master ~]# systemctl start slurmdbd
    
    [root@master ~]# systemctl status slurmdbd
    
        ● slurmdbd.service - Slurm DBD accounting daemon
           Loaded: loaded (/usr/lib/systemd/system/slurmdbd.service; enabled; vendor preset: disabled)
           Active: active (running) since Tue 2023-07-18 20:39:06 IST; 712ms ago
         Main PID: 13671 (slurmdbd)
            Tasks: 5
           CGroup: /system.slice/slurmdbd.service
                   └─13671 /usr/sbin/slurmdbd -D
    
        Jul 18 20:39:06 master systemd[1]: Started Slurm DBD accounting daemon.

# Start the slurmctld service:

 
    [root@master ~]# systemctl enable slurmctld.service
    
    [root@master ~]# systemctl start slurmctld.service
    
    [root@master ~]# systemctl status slurmctld.service
    
        ● slurmctld.service - Slurm controller daemon
           Loaded: loaded (/usr/lib/systemd/system/slurmctld.service; enabled; vendor preset: disabled)
           Active: active (running) since Tue 2023-07-18 16:44:35 IST; 3h 54min ago
         Main PID: 3867 (slurmctld)
           CGroup: /system.slice/slurmctld.service
                   └─3867 /usr/sbin/slurmctld -D
        
        Jul 18 16:44:35 master systemd[1]: Started Slurm controller daemon.
