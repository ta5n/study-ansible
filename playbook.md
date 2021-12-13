# Playbook.yml file

This file is a list of configurations to be run on listed environments. Every list item has these key-values.`tasks` section is an ordered list!

```yaml
# playbook-1.yml
- name: Play 1
  hosts: localhost
  tasks:
    - name: Execute a command
      command: date

    - name: execute a script
      script: test_script.sh

    - name: install httpd service
      yum:
        name: httpd
        state: present

    - name: start web server
      service:
        name: httpd
        state: started
```
*see [playbook-1.yml](playbook-1.yml)*

host should be defined in the `inventory` file:

```ini
; inventory-1.ini file
localhost

server1.company.com
server2.company.com

[mail]
server3.company.com
server4.company.com

[db]
server5.company.com
server6.company.com

[web]
server7.company.com
server8.company.com
```
*see [inventory-1.ini](inventory-1.ini)*
---

- Update the task to execute the command cat /etc/hosts and change task name to Execute a command to display hosts file

```yml
# initial
-
    name: 'Execute a command to display hosts file on localhost'
    hosts: localhost
    tasks:
        -
            name: 'Execute a date command'
            command: date
# solution
```yml
-
    name: 'Execute a command to display hosts file on localhost'
    hosts: localhost
    tasks:
        -
            name: 'Execute a command to display hosts file'
            command: cat /etc/hosts
```

- Update the playbook to add a second task. The new task must execute the command cat /etc/hosts and change new task name to Execute a command to display hosts file

```yml
# initial
-
    name: 'Execute two commands on localhost'
    hosts: localhost
    tasks:
        -
            name: 'Execute a date command'
            command: date
# solution
```yml
-
    name: 'Execute two commands on localhost'
    hosts: localhost
    tasks:
        -
            name: 'Execute a date command'
            command: date

        -
            name: Execute a command to display hosts file
            command: cat /etc/hosts
```

- We have been running all tasks on localhost. We would now like to run these tasks on the web_node1. Update the play to run the tasks on web_node1.

```yml
# initial
-
    name: 'Execute two commands on localhost'
    hosts: localhost
    tasks:
        -
            name: 'Execute a date command'
            command: date
        -
            name: 'Execute a command to display hosts file'
            command: 'cat /etc/hosts'

# solution
```yml
-
    name: 'Execute two commands on localhost'
    hosts: web_node1
    tasks:
        -
            name: 'Execute a date command'
            command: date
        -
            name: 'Execute a command to display hosts file'
            command: 'cat /etc/hosts'
```

- Refer to the attached inventory file. We would like to run the tasks defined in the play on all servers in boston.

```ini
# Sample Inventory File

# Web Servers
sql_db1 ansible_host=sql01.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass
sql_db2 ansible_host=sql02.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass
web_node1 ansible_host=web01.xyz.com ansible_connection=ssh ansible_user=administrator ansible_ssh_pass=Win$Pass
web_node2 ansible_host=web02.xyz.com ansible_connection=ssh ansible_user=administrator ansible_ssh_pass=Win$Pass
web_node3 ansible_host=web03.xyz.com ansible_connection=ssh ansible_user=administrator ansible_ssh_pass=Win$Pass

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

```yml
-
    name: 'Execute two commands on boston_nodes'
    hosts: boston_nodes
    tasks:
        -
            name: 'Execute a date command'
            command: date
        -
            name: 'Execute a command to display hosts file'
            command: 'cat /etc/hosts'
```

- Create a new play named Execute a command to display hosts file contents on web_node2 to execute cat /etc/hosts command on second node web_node2 and name the task Execute a command to display hosts file. 
> Refer to the inventory file above

```yml
-
    name: 'Execute command to display date on web_node1'
    hosts: web_node1
    tasks:
        -
            name: 'Execute a date command'
            command: date

- 
    name: 'Execute a command to display hosts file contents on web_node2'
    hosts: web_node2
        -
            name: 'Execute a command to display hosts file'
            command: cat /etc/hosts
```

- You are assigned a task to restart a number of servers in a particular sequence. The sequence and the commands to be used are given below. Note that the commands should be run on respective servers only. Refer to the inventory file and update the playbook to create the below sequence.
> Note: Use the description below to name the plays and tasks. Refer to the inventory file above
> ! Do not use this examples in production, since it is educational purposes only.

1. Stop the web services on web server nodes - service httpd stop
2. Shutdown the database services on db server nodes - service mysql stop
3. Restart all servers (web and db) at once - /sbin/shutdown -r
4. Start the database services on db server nodes - service mysql start
5. Start the web services on web server nodes - service httpd start

```yml
-
    name: 'Stop the web services on web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: 'Stop the web services on web server nodes'
            command: 'service httpd stop'

-
    name: 'Shutdown the database services on db server nodes'
    hosts: db_nodes
    tasks:
        -
            name: 'Shutdown the database services on db server nodes'
            command: 'service mysql stop'

-
    name: 'Restart all servers (web and db) at once'
    hosts: all_nodes
    tasks:
        -
            name: 'Restart all servers (web and db) at once'
            command: '/sbin/shutdown -r'

-
    name: 'Start the database services on db server nodes'
    hosts: db_nodes
    tasks:
        -
            name: 'Start the database services on db server nodes'
            command: 'service mysql start'

-
    name: 'Start the web services on web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: 'Start the web services on web server nodes'
            command: 'service httpd start'
```
