# mysql-cluster-quickstart
Provides a vagrant file, an Ansible playbook, and a script that downloads the .deb file, setups up Ansible, and boots the cluster. 

To use, just run:

```bash
boot.sh
```

The cluster should provision itself like this:

```
Cluster Configuration
---------------------
[ndbd(NDB)]     2 node(s)
id=2    @192.168.77.22  (mysql-5.6.28 ndb-7.4.10, Nodegroup: 0, *)
id=3    @192.168.77.23  (mysql-5.6.28 ndb-7.4.10, Nodegroup: 0)

[ndb_mgmd(MGM)] 1 node(s)
id=1    @192.168.77.21  (mysql-5.6.28 ndb-7.4.10)

[mysqld(API)]   2 node(s)
id=4    @192.168.77.24  (mysql-5.6.28 ndb-7.4.10)
id=5    @192.168.77.25  (mysql-5.6.28 ndb-7.4.10)
```

Once the provisioning script has finished you should be able to connect to
machine4 or machine5 (192.168.77.24 and 192.168.77.25) with any standard MySQL
client and create tables.

To ensure that they are distributed you have to create them using
the ENGINE=NDB syntax. For example:

```sql
CREATE TABLE ctest (i INT) ENGINE=NDB;
```

or


```sql
CREATE TABLE ctest (i INT) ENGINE=NDBCLUSTER;
```

They should function identically, since NDB is an alias for NDBCLUSTER.

The management node is machine1 (192.168.77.21). You can run the management tool via:

```bash
vagrant ssh machine1
/opt/mysql/server-5.6/bin/ndb_mgm
```

For more information you can read the manual here: https://dev.mysql.com/doc/refman/5.7/en/mysql-cluster.html

