# Ansible Inventory

- The web servers are linux, but the db server is windows. Add additional parameters in each line to add `ansible_connection`, `ansible_user` and `password`. Use the below table for information about credentials.

| Alias |        Host         | Connection |     User      |   Password   |
| :---- | :-----------------: | :--------: | :-----------: | :----------: |
| web1  | server1.company.com |    SSH     |     root      | Password123! |
| web2  | server2.company.com |    SSH     |     root      | Password123! |
| web3  | server3.company.com |    SSH     |     root      | Password123! |
| db1   | server4.company.com |  Windows   | administrator | Password123! |

> Note: For linux use `ansible_ssh_pass` and for windows use `ansible_password`. Connector for windows is `winrm`

```ini
# Web Servers
web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!

# Database Servers
db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Password123!
```

- Let us now create a group of groups. Create a new group called `all_servers` and add the previously created groups `web_servers` and `db_servers` to it.

  > Note: Syntax:
  > [parent_group:children]
  > child_group1
  > child_group2

```ini
# Web Servers
web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!

# Database Servers
db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Password123!


[web_servers]
web1
web2
web3

[db_servers]
db1

[all_servers:children]
web_servers
db_servers
```

- Try and represent the data given in the below table in Ansible Inventory format

  | Server Alias | Server Name   | OS    | User          | Password |
  | :----------- | :------------ | :---- | :------------ | :------- |
  | sql_db1      | sql01.xyz.com | Linux | root          | Lin$Pass |
  | sql_db2      | sql02.xyz.com | Linux | root          | Lin$Pass |
  | web_node1    | web01.xyz.com | Win   | administrator | Win$Pass |
  | web_node2    | web02.xyz.com | Win   | administrator | Win$Pass |
  | web_node3    | web03.xyz.com | Win   | administrator | Win$Pass |

  Group the servers together based on this table

  | Group        | Members                         |
  | :----------- | :------------------------------ |
  | db_nodes     | sql_db1, sql_db2                |
  | web_nodes    | web_node1, web_node2, web_node3 |
  | boston_nodes | sql_db1, web_node1              |
  | dallas_nodes | sql_db2, web_node2, web_node3   |
  | us_nodes     | boston_nodes, dallas_nodes      |

```ini
# Web Servers
web_node1 ansible_host=web01.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
web_node2 ansible_host=web02.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
web_node3 ansible_host=web03.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass

# DB Servers
sql_db1 ansible_host=sql01.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass
sql_db2 ansible_host=sql02.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass

# Groups
[db_nodes]
sql_db1
sql_db2

[web_nodes]
web_node1
web_node2
web_node3

[boston_nodes]
sql_db1
web_node1

[dallas_nodes]
sql_db2
web_node2
web_node3

[us_nodes:children]
boston_nodes
dallas_nodes
```
*see example [hosts](hosts)*<br>*see example [inventory.ini](inventory.ini)*
