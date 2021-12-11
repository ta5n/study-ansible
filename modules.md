# Ansible Modules

Internal and external library commands that extends ansible capabilities

- System
  - User
  - Group
  - Hostname
  - Iptables
  - Lvg
  - Make
  - Mount
  - Ping
  - Timezone
  - Systemd
  - Service
- Commands
  - Command
  - Expect
  - Raw
  - Script
  - Shell
- Files
  - Acl
  - Archive
  - Copy
  - File
  - Find
  - Lineinfile
  - Replace
  - Stat
  - Template
  - Unarchive
- Database
  - MongoDB
  - Mssql
  - Mysql
  - Postgresql
  - Proxysql
  - vertica
- Cloud
  - AWS
  - Atomic
  - Azure
  - Centrylink
  - Cloudstack
  - Digital Ocean
  - Docker
  - GCP
  - Linode
  - Openstack
  - Rackspace
  - Smartos
  - Softlayer
  - VMware
  - ...
- Windows
  - Win_copy
  - Win_command
  - Win_domain
  - Win_file
  - Win_iis_website
  - Win_msg
  - Win_msi
  - Win_package
  - Win_ping
  - Win_path
  - Win_robocopy
  - Win_regedit
  - Win_shell
  - Win_user
  - ...
- ...
> more detail on [docs.ansible.com](https://docs.ansible.com)


---
## Examples

- Update the playbook with a play to Execute a script on all web server nodes. The script is located at /tmp/install_script.sh
  > Use the Script module, Refer to the inventory file below

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

[all_nodes:children]
db_nodes
web_nodes
```

```yml
---
-
    name: Execute a script on all web server nodes
    hosts: web_nodes
    tasks:
        -
            name: Execute a script on all web server nodes
            script: /tmp/install_script.sh
```

- Update the playbook to add a new task to start httpd services on all web nodes
  > use service module

```yml
---
-
    name: Execute a script on all web server nodes
    hosts: web_nodes
    tasks:
        -
            name: 'Execute a script on all web server nodes'
            script: /tmp/install_script.sh
        -
            name: Start httpd services
            service: 
              name: httpd
              state: started
```

- Update the playbook to add a new task in the beginning to add an entry into /etc/resolv.conf file for hosts. The line to be added is `nameserver 10.1.250.10` Note: The new task must be executed first, so place it accordingly.
  > Use the Lineinfile module

```yml
---
-
    name: Execute a script on all web server nodes
    hosts: web_nodes
    tasks:
        -
            name: update nameserver
            lineinfile:
              path: /etc/resolv.conf
              line: 'nameserver 10.1.250.10'
        -
            name: 'Execute a script on all web server nodes'
            script: /tmp/install_script.sh
        -
            name: Start httpd services
            service: 
              name: httpd
              state: started
```

- Update the playbook to add a new task at second position (right after adding entry to resolv.conf) to create a new web user. 
  > Use the user module for this. User details:<br>
  > Username: web_user<br>
  > uid: 1040<br>
  > group: developers<br>

```yml
---
-
    name: Execute a script on all web server nodes
    hosts: web_nodes
    tasks:
        -
            name: update nameserver
            lineinfile:
              path: /etc/resolv.conf
              line: 'nameserver 10.1.250.10'
        -
            name: create a new web user
            user:
              name: web_user
              uid: 1040
              group: developers
        -
            name: 'Execute a script on all web server nodes'
            script: /tmp/install_script.sh
        -
            name: Start httpd services
            service: 
              name: httpd
              state: started
```
