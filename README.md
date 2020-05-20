# mariadb-galera-cluser on docker-compose 

Note:- In Mariadb Galera cluer every node work like a Master and as well as Slave. 

This mariadb galera cluster is of three nodes or machies 

To create this galera cluster take three different nodes or machines, Here i took 
node1(192.168.1.111), node2(192.168.1.112), and node3(192.168.1.112)

Step 1 :- clone the project file (docker-compose.yml and galera.conf) on your machine.

Step 2 :- On node1 copy and configure docker-compose file and galera.conf, add your node1 machine IP in the place of 'node1 IP'

docker-compose.yml 

version: '3'
services:
  mariadb:
    image: mariadb
    network_mode: host
    ports:
      - "node1 IP:3306:3306" 
      - "node1 IP:4444:4444"
      - "node1 IP:4567:4567"
      - "node1 IP:4568:4568"
    hostname: galera-n1
    container_name: mariadb-galera-n1
    restart: always
    command: 
      - --wsrep-new-cluster
    environment:
      - MYSQL_ROOT_PASSWORD=password
    volumes:
      - /dbdata/mysql/data:/var/lib/mysql
      - /dbdata/mysql/logs:/var/log/mysql
      - ./conf.d:/etc/mysql/mariadb.conf
      


Note :- After starting node1 wait for 2-3 minutes to prepare databse. 

Note :- Add Ip of node1, node2 and node3 in the place of IP1, IP2 and IP3. this file will be same for node1, node2 and node3
except "Galera Node Configuration" Section, In "Galera Node Configuration" we need to change node address and name. 

#Galera Node Configuration
wsrep_node_address="node1 IP" 
wsrep_node_name="galera-n1" 


galera.conf

[mysqld]
binlog_format=ROW
default-storage-engine=innodb
innodb_autoinc_lock_mode=2
bind-address=0.0.0.0

#Galera Provider Configuration
wsrep_on=ON

#for base VM
#wsrep_provider=/usr/lib64/galera-4/libgalera_smm.so
#for Docker
wsrep_provider=/usr/lib/libgalera_smm.so


#Galera Cluster Configuration
wsrep_cluster_name="test_cluster"
wsrep_cluster_address="gcomm://IP1,IP2,IP3"


#wsrep_sst_auth="password"


#Galera Synchronization Configuration
wsrep_sst_method=rsync

#Galera Node Configuration
wsrep_node_address="node1 IP"
wsrep_node_name="galera-n1"



and run this docker-compose.yml file.  
command: docker-compose up -d


Note :- On node2 and node3 this line(command parameter) will not be present.
"
   command: 
      - --wsrep-new-cluster
"
Because on node1 we need to initilize the cluster and reset we need to connect only.

Step 3:- On Node2 copy and configure docker-compose file and galera.conf file. add your node2 machine IP in the place of 'node2 IP' 

docker-compose.yml 

version: '3' 
services: 
  mariadb: 
    image: mariadb
    network_mode: host
    ports:
      - "node2 IP:3306:3306"
      - "node2 IP:4444:4444"
      - "node2 IP:4567:4567"
      - "node2 IP:4568:4568"
    hostname: galera-n2
    container_name: mariadb-galera-n2
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=password
    volumes:
      - /dbdata/mysql/data:/var/lib/mysql
      - /dbdata/mysql/logs:/var/log/mysql
      - ./conf.d:/etc/mysql/mariadb.conf

Note :- Add Ip of node1, node2 and node3 in the place of IP1, IP2 and IP3. this file will be same for node1, node2 and node3
except "Galera Node Configuration" Section, In "Galera Node Configuration" we need to change node address and name. 

#Galera Node Configuration
wsrep_node_address="node2 IP" 
wsrep_node_name="galera-n2" 


galera.conf

[mysqld]
binlog_format=ROW
default-storage-engine=innodb
innodb_autoinc_lock_mode=2
bind-address=0.0.0.0

#Galera Provider Configuration
wsrep_on=ON

#wsrep_provider=/usr/lib64/galera-4/libgalera_smm.so
wsrep_provider=/usr/lib/libgalera_smm.so

#Galera Cluster Configuration
wsrep_cluster_name="test_cluster"
wsrep_cluster_address="gcomm://IP1,IP2,IP3"


#wsrep_sst_auth="password"


#Galera Synchronization Configuration
wsrep_sst_method=rsync

#Galera Node Configuration
wsrep_node_address="node2 IP"
wsrep_node_name="galera-n2"


Step 4:- On Node3 copy and configure docker-compose file and galera.conf file. add your node3 machine IP in the place of 'node3 IP' 

docker-compose.yml 

version: '3'
services:
  mariadb:
    image: mariadb
    network_mode: host
    ports:
      - "node2 IP:3306:3306"
      - "node2 IP:4444:4444"
      - "node2 IP:4567:4567"
      - "node2 IP:4568:4568"
    hostname: galera-n3
    container_name: mariadb-galera-n3
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=password
    volumes:
      - /dbdata/mysql/data:/var/lib/mysql
      - /dbdata/mysql/logs:/var/log/mysql
      - ./conf.d:/etc/mysql/mariadb.conf

Note :- Add Ip of node1, node2 and node3 in the place of IP1, IP2 and IP3. this file will be same for node1, node2 and node3
except "Galera Node Configuration" Section, In "Galera Node Configuration" we need to change node address and name. 

#Galera Node Configuration
wsrep_node_address="node3 IP" 
wsrep_node_name="galera-n3" 


galera.conf

[mysqld]
binlog_format=ROW
default-storage-engine=innodb
innodb_autoinc_lock_mode=2
bind-address=0.0.0.0

#Galera Provider Configuration
wsrep_on=ON

#wsrep_provider=/usr/lib64/galera-4/libgalera_smm.so
wsrep_provider=/usr/lib/libgalera_smm.so


#Galera Cluster Configuration
wsrep_cluster_name="test_cluster"
wsrep_cluster_address="gcomm://IP1,IP2,IP3"


#wsrep_sst_auth="password"


#Galera Synchronization Configuration
wsrep_sst_method=rsync

#Galera Node Configuration
wsrep_node_address="node3 IP"
wsrep_node_name="galera-n3"



