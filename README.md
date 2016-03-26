# MySQL HA ansible-tutorial

Taken and adapted from robertbarabas/ansible-tutorial  
https://github.com/robertbarabas/ansible-tutorial/tree/master/demo

## Getting started

You need to have Vagrant, Virtualbox and Ansible installed

- `vagrant up` will create 2 VMs: a master and a slave
- `ansible-playbook site.yml` will provision both nodes, incl. the database (MySQL's "sakila" database)

master has IP-address 192.168.10.100  
slave has IP-address 192.168.10.101

## Check master and slave status

### Check master status

`ssh root@192.168.10.100 -i ~/.vagrant.d/insecure_private_key "mysql -e \"SHOW MASTER STATUS\G\"" `

```
*************************** 1. row ***************************
             File: mysqld-bin.000001
         Position: 3374933
     Binlog_Do_DB:
 Binlog_Ignore_DB:
Executed_Gtid_Set:
```

### Check slave status

`ssh root@192.168.10.101 -i ~/.vagrant.d/insecure_private_key "mysql -e \"SHOW SLAVE STATUS\G\"" `

```
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: master
                  Master_User: replication
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysqld-bin.000001
          Read_Master_Log_Pos: 3374933
               Relay_Log_File: mysqld-relay-bin.000002
                Relay_Log_Pos: 284
        Relay_Master_Log_File: mysqld-bin.000001
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB:
          Replicate_Ignore_DB:
           Replicate_Do_Table:
       Replicate_Ignore_Table:
      Replicate_Wild_Do_Table:
  Replicate_Wild_Ignore_Table:
                   Last_Errno: 0
                   Last_Error:
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 3374933
              Relay_Log_Space: 458
              Until_Condition: None
               Until_Log_File:
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File:
           Master_SSL_CA_Path:
              Master_SSL_Cert:
            Master_SSL_Cipher:
               Master_SSL_Key:
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error:
               Last_SQL_Errno: 0
               Last_SQL_Error:
  Replicate_Ignore_Server_Ids:
             Master_Server_Id: 4603
                  Master_UUID: 7483364d-f348-11e5-bfee-0800275ff74d
             Master_Info_File: /var/lib/mysql/master.info
                    SQL_Delay: 0
          SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State: Slave has read all relay log; waiting for the slave I/O thread to update it
           Master_Retry_Count: 86400
                  Master_Bind:
      Last_IO_Error_Timestamp:
     Last_SQL_Error_Timestamp:
               Master_SSL_Crl:
           Master_SSL_Crlpath:
           Retrieved_Gtid_Set:
            Executed_Gtid_Set:
                Auto_Position: 0
```


## TODO

```
TASK [Remove data in /var/lib/mysql] *******************************************
changed: [192.168.10.101]
 [WARNING]: Consider using file module with state=absent rather than running rm
```
