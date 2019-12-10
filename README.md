mysql_cluster
=============

Small role to set up a master and a slave MySQL nodes on docker containers.

Requirements
------------

Docker and docker python module. Great fit to geerlingguy.docker and geerlingguy.pip.

Role Variables
--------------

Variables with their default values:

mysql_user: mysql  
mysql_group: mysql  
mysql_host_files_path: "/mysql"  
  
mysql_nodes:  
  master:  
    config_file: master_conf.j2  
    startup_file: master_startup  
    mysql_user: master_user  
    mysql_password: "{{mysql_master_user_password}}"  
  slave:  
    config_file: slave_conf.j2  
    startup_file: slave_startup  
    mysql_replication_user: replication_user  
    mysql_rpl_password: "{{mysql_repl_user_password}}"  
    mysql_user: other_user  
    mysql_password: "{{mysql_slave_user_password}}"  
mysql_network_name: mysql  
mysql_image: "mysql:5.7.28"  
mysql_db: "my-db"  

Secrets, no defaults, **must be defined!**:

* mysql_root_password
* mysql_master_user_password	_#PW for the user created on the master_
* mysql_slave_user_password		_#PW for the user created on the slave_
* mysql_repl_user_password		_#PW for the replication user_

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename }

Test if it works with
---------------------

Master node:
```bash
(host) docker exec -it mysql_master bash
(master) mysql -p
(password)
(master) use my-db;
(master) create table code(code int);
(master) insert into code values (100), (200);
```

Slave node:
```bash
(host) docker exec -it mysql_slave bash
(slave) mysql -p
(password)
(slave) use my-db;
(slave) select * from code \G
```

License
-------

GNU-GPL V3.0

Author Information
------------------

[Francisco Fregona](franciscofregona@gmail.com)
