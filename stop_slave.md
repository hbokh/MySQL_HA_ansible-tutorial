## Stop the slave being a slave

`slave mysql> show slave status\G;`

```
*************************** 1. row ***************************
               Slave_IO_State: Connecting to master
                  Master_Host: master
                  Master_User: replication
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysqld-bin.000001
          Read_Master_Log_Pos: 3375018
               Relay_Log_File: mysqld-relay-bin.000003
                Relay_Log_Pos: 4
        Relay_Master_Log_File: mysqld-bin.000001
             Slave_IO_Running: Connecting
            Slave_SQL_Running: Yes
```


`slave mysql> stop slave io_thread;`

```
slave mysql> show slave status\G;
*************************** 1. row ***************************
               Slave_IO_State:
                  Master_Host: master
                  Master_User: replication
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysqld-bin.000001
          Read_Master_Log_Pos: 3375018
               Relay_Log_File: mysqld-relay-bin.000003
                Relay_Log_Pos: 4
        Relay_Master_Log_File: mysqld-bin.000001
             Slave_IO_Running: No
            Slave_SQL_Running: Yes
```

```
slave mysql> show processlist\G;
*************************** 1. row ***************************
           Id: 2
         User: system user
         Host:
           db: NULL
      Command: Connect
         Time: 258
        State: Slave has read all relay log; waiting for the slave I/O thread to update it
         Info: NULL
    Rows_sent: 0
Rows_examined: 0
```

```
slave mysql> stop slave;
Query OK, 0 rows affected (0.00 sec)

slave mysql> reset master;
Query OK, 0 rows affected (0.01 sec)

slave mysql> SET GLOBAL read_only = OFF;
Query OK, 0 rows affected (0.00 sec)

slave mysql> exit
Bye
```

[root@slave ~]# vi /etc/my.cnf

read-only = 0
